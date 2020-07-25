KEEP Network Helm Charts
========================

[![CircleCI](https://circleci.com/gh/ajcrowe/keep-client-helm-chart/tree/master.svg?style=svg)](https://circleci.com/gh/ajcrowe/keep-client-helm-chart/tree/master)

These charts provide a simple way to deploy KEEP Network clients into Kubernetes and make for easier and simplier upgrade processes

## Prerequisites

* Kubernetes 1.9+
* Support for `PersistentVolume`

## Using the Chart

### Required Config

To install the chart you need to provide a few essential configuration options, these are:

* Node type `ecdsa` or `core`
* Node config information
* Ethereum wallet JSON
* Ethereum wallet password

There are multiple ways of passing these data to the chart including:

* Inline `toml` and `json`, ([example](examples/inline-resources.yaml))
* Existing secrets ([example](examples/existing-secrets.yaml))
* Reading from the filesystem ([example](examples/loca-files.md))

It's recommended you place these parameters in a local values file and install the chart with the following command

### Install

```
helm install my-keep-node keep-client -f myvalues.yaml --set keyPass.password="mypassword"
```

## Configuration Options

Below are configuration options for the chart

| Parameter                       | Description                                                                                | Default                                            |
|---------------------------------|--------------------------------------------------------------------------------------------|----------------------------------------------------|
| affinity                        | Affinity for pod assignment                                                                | `{}`                                               |
| config.announcedAddrs           | List of addresses to announce connectivity on                                              | `['"/ip4/127.0.0.1/tcp/3919"']`                    |
| config.contractAddrs            | Key/value pairs of contracts to add to config                                              | See [`values.yaml`](keep-client/values.yaml)       |
| config.ethereum.account         | Ethereum account address                                                                   | `"0x0000000000000000000000000000000000000000"`     |
| config.ethereum.keyFile         | Keyfile name to load from `config.paths.keyStore`                                          | `wallet.json`                                      |
| config.ethereum.url             | Ethereum WS Url                                                                            | `"wss://ropsten.infura.io/ws/v3/mykey"`            |
| config.ethereum.urlRpc          | Ethereum HTTP RPC Url                                                                      | `"https://ropsten.infura.io/v3/mykey"`             |
| config.logLevel                 | Node logging level                                                                         | `""`                                               |
| config.paths.config             | Path to store config                                                                       | `/mnt/config`                                      |
| config.paths.data               | Path to persist data                                                                       | `/mnt/persistence`                                 |
| config.paths.keyStore           | Path to store keys                                                                         | `/mnt/keystore`                                    |
| config.peers                    | List of peers to bootstrap network                                                         | See [`values.yaml`](keep-client/values.yaml)       |
| config.port                     | Port to listen for p2p traffic                                                             | `3919`                                             |
| config.sanctionedApplications   | List of sanctioned contracts                                                               | `['"0x14dC06F762E7f4a756825c1A1dA569b3180153cB"']` |
| configFile.enabled              | Enable reading local `toml` file                                                           | `false`                                            |
| configFile.localFile            | Local `toml` file name                                                                     | `config.toml`                                      |
| configYaml                      | Raw `config.toml` to place in `ConfigMap`                                                  | `{}`                                               |
| coreNode.enabled                | Enable Core client                                                                         | `false  `                                          |
| coreNode.image.repository       | KEEP Core image repository                                                                 | `keepnetwork/keep-client`                          |
| coreNode.image.tag              | KEEP Core image tag                                                                        | `"v1.2.4-rc"`                                      |
| ecdsaNode.enabled               | Enable ECDSA client                                                                        | `true`                                             |
| ecdsaNode.image.repository      | KEEP ECDSA image repository                                                                | `keepnetwork/keep-ecdsa-client`                    |
| ecdsaNode.image.tag             | KEEP ECDSA image tag                                                                       | `"v1.1.2"`                                         |
| fullnameOverride                | Custom fullname override for chart                                                         | `""`                                               |
| imagePullSecrets                | Image pull secrets                                                                         | `[]`                                               |
| keyFile.existingSecret          | Existing secret to use for keyfile (ensure the filename is set to config.ethereum.keyFile) | `{}`                                               |
| keyFile.localFile               | Local `json` wallet filename                                                               | `wallet.json`                                      |
| keyFileJson                     | Raw JSON keyfile to place into secret                                                      | `{}`                                               |
| keyPass.existingSecret          | Existing secret containing `KEEP_ETHEREUM_PASSWORD` value                                  | ``                                                 |
| keyPass.password                | Password for keyfile                                                                       | `"keepnetworkclient"`                              |
| metrics.enabled                 | Enable metrics                                                                             | `false`                                            |
| metrics.port                    | Set metrics port for node                                                                  | `8080`                                             |
| metrics.networkMetricsTick      | Set tick for network metrics collection                                                    | `60`                                               |
| metrics.ethereumMetricsTick     | Set tick for ethereum metrics collection                                                   | `600`                                              |
| nameOverride                    | Custom name override for chart                                                             | `""`                                               |
| nodeSelector                    | Node labels for controller pod assignment                                                  | `{}`                                               |
| persistentDisk.accessModes      | Access mode for the persistent volume                                                      | `ReadWriteOnce`                                    |
| persistentDisk.enabled          | Enable persistent disk for node                                                            | `true`                                             |
| persistentDisk.size             | Size of persistent disk to provision                                                       | `1Gi`                                              |
| persistentDisk.storageClass     | Storage Class to provision disk with                                                       | `""`                                               |
| podAnnotations                  | Annotations to be added to the node pod                                                    | `{}`                                               |
| podLabels                       | Labels to be added to node pod                                                             | `{}`                                               |
| podSecurityContext              | Set security context for pod                                                               | `{}`                                               |
| preParamsGenerationTimeout      | Set pre-params generations timeout for ECDSA nodes                                         | `""`                                               |
| probesEnabled                   | Enable readiness and liveness probes                                                       | `false`                                            |
| pullPolicy                      | Image pull policy                                                                          | `IfNotPresent`                                     |
| replicaCount                    | Replica count for `StatefulSet`                                                            | `1`                                                |
| resources                       | Resource limits and requests for node                                                      | `{}`                                               |
| securityContext                 | Set security context for node container                                                    | `{}`                                               |
| service.name                    | Set service port name                                                                      | `p2p`                                              |
| service.port                    | Set service port number                                                                    | `3919`                                             |
| service.type                    | Set service type                                                                           | `ClusterIP`                                        |
| serviceAccount.annotations      | Service account annotations                                                                | `{}`                                               |
| serviceAccount.create           | If `true` a service account with created and used                                          | `true`                                             |
| serviceAccount.name             | Existing service account to use                                                            | `""`                                               |
| serviceMonitor.enabled          | Enable service monitor for metrics                                                         | `false`                                            |
| serviceMonitor.additionalLabels | Additional labels for service Montitor                                                     | `{}`                                               |
| serviceMonitor.annotations      | Annotations for service monitor                                                            | `{}`                                               |
| serviceMonitor.interval         | Interval for metrics scraping                                                              | `""`                                               |
| serviceMonitor.scrapeTimeout    | Timeout for scraping metrics                                                               | `""`                                               |
| tolerations                     | Controller pod toleration for taints                                                       | `[]`                                               |
