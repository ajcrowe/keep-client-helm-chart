Local Files
===========

To read local files for `config.toml` and `wallet.json` you can place these within the chart and install from there with

```
cd keep-client
helm install my-keep-node . -f myvalues.yaml 
```

By default the chart will look for `config.toml` and `wallet.json`
