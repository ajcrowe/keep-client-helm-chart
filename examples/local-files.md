Local Files
===========

To read local files for `config.toml` and `wallet.json` you can place these within the chart and install from there with

```
git clone https://github.com/ajcrowe/keep-client-helm-chart
cd keep-client-helm-chart/keep-client
# ... place files locally
helm install my-keep-node . -f myvalues.yaml 
```

By default the chart will look for `config.toml` and `wallet.json`
