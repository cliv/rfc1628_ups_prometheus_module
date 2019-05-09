## Prometheus SNMP Exporter Module for RFC1628 Compliant UPS cards

I've generated a snmp.yml snippet for Prometheus' SNMP exporter to allow it to read mibs from RFC1628-Compliant UPS management interfaces 

### Working Devices
If you use this config with your UPS, and it works, please file an issue or pull request to add your hardware to this list
- Eaton 5P 1500RT (Network-MS Card)

### How to use the Snippet

1. Copy the contents of snmp.yml in this repository into the snmp.yml file that the prometheus snmp exporter is using, or use the snmp.yml file as your configuration file for prometheus's snmp exporter if all you care about are Eaton UPS metrics.
2. Look below at how to configure your prometheus.yml file to use the `rfc1628_ups` module.

### Prometheus.yml configuration

Add the below to your prometheus.yaml file, this will use the module that we added to the snmp exporter.

```
  - job_name: 'snmp'
    static_configs:
      - targets:
        - 'host.example.com'
    metrics_path: /snmp
    params:
      module: [rfc1628_ups]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9116  # The SNMP exporter's real hostname:port.
```

### Credits/Sources
- MIBs: https://powerquality.eaton.com/Support/Software-Drivers/Downloads/connectivity-firmware/eaton-network-connectivity-mib-files.zip
- Module Idea: https://github.com/billykwooten/idrac_promethus_snmp_module


