# Validator Hosting

Below are the suggestions for properly hosting a validator node in a PMC Chain network. Please pay careful attention to all the items listed below to make sure that your validator setup is properly configured to be secure, stable and performant.

### Knowledge base

Before trying to run the validator node, please read through this document thoroughly.\
Additional docs that might be helpful are:

* Installation
* Cloud setup
* CLI commands
* Server config file
* Private keys
* Prometheus metrics
* Secret managers
* Backup/Restore

### Minimum system requirements

| Type | Value                                                                                          | Influenced by                                                                                                                |
| ---- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| CPU  | 2 cores                                                                                        | <ul><li>Number of JSON-RPC queries</li><li>Size of the blockchain state</li><li>Block gas limit</li><li>Block time</li></ul> |
| RAM  | 2 GB                                                                                           | <ul><li>Number of JSON-RPC queries</li><li>Size of the blockchain state</li><li>Block gas limit</li></ul>                    |
| Disk | <ul><li>10 GB root patition</li><li>30 GB root partition with LVM for disk extension</li></ul> | <ul><li>Size of the blockchain state</li></ul>                                                                               |

### Service configuration

`PMC-Chain` binary needs to run as a system service automatically upon established network connectivity and have start / stop / restart functionalities. We recommend using a service manager like `systemd.`

Example `systemd` system configuration file:

```
[Unit]
Description=PMC Chain Server
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=10
User=ubuntu
ExecStart=/usr/local/bin/PMC-Chain server --config /home/ubuntu/polygon/config.yaml

[Install]
WantedBy=multi-user.target
```

#### Binary

In production workloads `PMC-Chain` binary should only be deployed from pre-built GitHub release binaries - not by manually compiling. :::info By manually compiling `develop` GitHub branch, you may introduce breaking changes to your environment.\
For that reason it is recommended to deploy PMC Chain binary exclusively from releases, as it will contain information about breaking changes and how to overcome them. :::

Please refer to Installation for a complete overview of installation method.

#### Data storage

The `data/` folder containing the entire blockchain state should be mounted on a dedicated disk / volume allowing for automatic disk backups, volume extension and optionally mounting the disk/volume to another instance in case of failure.

#### Log files

Log files need to be rotated on a daily basis (with a tool like `logrotate`). :::warning If configured without log rotation, log files could use up all the available disk space which could disrupt the validator uptime. :::

Example `logrotate` configuration:

```
/home/ubuntu/polygon/logs/node.log
{
        rotate 7
        daily
        missingok
        notifempty
        compress
        prerotate
                /usr/bin/systemctl stop PMC-Chain.service
        endscript
        postrotate
                /usr/bin/systemctl start PMC-Chain.service
        endscript
}
```

Refer to the Logging section below for recommendations on log storage.

#### Additional dependencies

`PMC-Chain` is statically compiled, requiring no additional host OS dependencies.

### Maintenance

Below are the best practices for maintaining a running validator node of a PMC Chain network.

#### Backup

There are two types of backup procedures recommended for PMC Chain nodes.

The suggestion is to use both, whenever possible, with the PMC Chain backup being an always available option.

* _**Volume backup**_:\
  Daily incremental backup of the `data/` volume of the PMC Chain node, or of the complete VM if possible.
* _**PMC Chain backup**_:\
  Daily CRON job which does regular backups of PMC Chain and moves the `.dat` files to an offsite location or to a secure cloud object storage is recommended.

The PMC Chain backup should ideally not overlap with the Volume backup described above.

Refer to Backup/restore node instance for instructions on how to perform backups of PMC Chain.

#### Logging

The logs outputted by the PMC Chain nodes should:

* be sent to an external data store with indexing and searching capabilities
* have a log retention period of 30 days

If this is your first time setting up a PMC Chain validator, we recommend to start the node with the `--log-level=DEBUG` option to be able to quickly debug any issues you might face.

:::info The `--log-level=DEBUG` will make the node's log output be as verbose as possible.\
Debug logs will drastically increase the size of the log file which must be taken into account when setting up log rotation solution. :::

#### OS security patches

Administrators need to ensure that the validator instance OS is always updated with the latest patches at least once every month.

### Metrics

#### System metrics

Administrators need to setup some kind of system metrics monitor, (e.g. Telegraf + InfluxDB + Grafana or a 3rd party SaaS).

Metrics that need to be monitored and that need to have alarm notifications setup:

| Metric name           | Alarm threshold               |
| --------------------- | ----------------------------- |
| CPU usage (%)         | > 90% for more than 5 minutes |
| RAM utilization (%)   | > 90% for more than 5 minutes |
| Root disk utilization | > 90%                         |
| Data disk utilization | > 90%                         |

#### Validator metrics

Administrators need to setup collection of metrics from PMC Chain's Prometheus API to be able to monitor the blockchain performance.

Refer to Prometheus metrics to understand which metrics are being exposed and how to set up Prometheus metric collection.

Extra attention needs to be paid to the following metrics:

* _**Block production time**_ - if block production time is higher than normal, there is a potential problem with the network
* _**Number of consensus rounds**_ - if there is more than 1 round, there is a potential problem with the validator set in the network
* _**Number of peers**_ - if the number of peers drop, there is a connectivity issue in the network

### Security

Below are the best practices for securing a running validator node of a PMC Chain network.

#### API services

* _**JSON-RPC**_ - Only API service that needs to be exposed to the public ( via load balancer or directly ).\
  This API should run on all interfaces or on a specific IP address ( example: `--json-rpc 0.0.0.0:8545` or `--json-prc 192.168.1.1:8545` ). :::info As this is publicly facing API it is recommended to have a load balancer / reverse proxy in front of it to provide security and rate limiting. :::
* _**LibP2P**_ - This is the networking API used by nodes for peer communication. It needs to run on all interfaces or on a specific ip address ( `--libp2p 0.0.0.0:1478` or `--libp2p 192.168.1.1:1478` ). This API should not be publicly exposed, but it should be reachable from all other nodes. :::info If ran on localhost ( `--libp2p 127.0.0.1:1478` ) other nodes will not be able to connect. :::
* _**GRPC**_ - This API is used only for running operator commands and noting else. As such it should run exclusively on localhost ( `--grpc-address 127.0.0.1:9632` ).

#### PMC Chain secrets

PMC Chain secrets ( `ibft` and `libp2p` keys ) should not be stored on a local file system.\
Instead, a supported Secret Manager should be used.\
Storing secrets to local file system should only be used in non-production environments.

### Update

Following is the desired update procedure for validator nodes, described as step-by-step instructions.

#### Update procedure

* Download the latest PMC Chain binary from the official GitHub [releases](https://github.com/0xPolygon/PMC-Chain/releases)
* Stop the PMC Chain service ( example: `sudo systemctl stop PMC-Chain.service` )
* Replace the existing `PMC-Chain` binary with the downloaded one ( example: `sudo mv PMC-Chain /usr/local/bin/` )
* Check if correct `PMC-Chain` version is in place by running `PMC-Chain version` - it should correspond to the release version
* Check the release documentation if there are any backwards compatibility steps needed to be done before starting `PMC-Chain` service
* Start `PMC-Chain` service ( example: `sudo systemctl start PMC-Chain.service` )
* Finally, check the `PMC-Chain` log output and make sure everything is running without any `[ERROR]` logs

:::warning When there is a breaking release, this update procedure must be performed on all nodes as the currently running binary is not compatible with the new release.

This means that the chain must be halted for a short period of time ( until `PMC-Chain` binaries are replaced and service restarted ) so plan accordingly.

You can use tools like [**Ansible**](https://www.ansible.com/) or some custom script to perform the update efficiently and minimize chain downtime. :::

### Startup procedure

Following is the desired flow of the startup procedure for PMC Chain validator

* Read through the docs listed in Knowlege Base section
* Apply the latest OS patches on the validator node
* Download the latest `PMC-Chain` binary from the official GitHub [releases](https://github.com/0xPolygon/PMC-Chain/releases) and place it in local instance `PATH`
* Initialize one of the supported secrets managers using `PMC-Chain secrets generate` CLI command
* Generate and store secrets using `PMC-Chain secrets init` CLI command
* Take note of `NodeID` and `Public key (address)` values
* Generate `genesis.json` file as described in cloud setup using `PMC-Chain genesis` CLI command
* Generate default config file using `PMC-Chain server export` CLI command
* Edit `default-config.yaml` file to accommodate local validator node environment ( file paths, etc. )
* Create a PMC Chain service ( `systemd` or similar ) where `PMC-Chain` binary will run the server from a `default-config.yaml` file
* Start PMC Chain server by starting the service ( example: `systemctl start PMC-Chain` )
* Check the `PMC-Chain` log output and make sure the blocks are being generated and that there are no `[ERROR]` logs
* Check the chain functionality by calling a JSON-RPC method like `eth_chainId`
