# openweather_exporter

A python prometheus exporter which collect data from [Open Weather's API]

## Badges

[![python][python-badge]][python-version] ![pylint-score]

## Requirements

* python >= 3.7*
* Open Weather API Key*

## Resolving Dependencies

```sh
pip3 install -r requirements.txt
```
## Config File

You have to fill these fields in the [YAML](https://en.wikipedia.org/wiki/YAML) Config File before your first run:

| Field   | Description |
|---------|-------------|
| **api_key** | Your api key |
| **cities** | List of city ID's from the cities you want to collect data |

> :warning: You have to find those ID's in the [City List] available in the Open Weather's website.

| Opção | Obrigatório? | Descrição |
|-------|--------------|-----------|
| **-p, --port** | No. Default: 8080 | App Port |
| **-c, --config** | Yes | Path to Config File |
| **-h, --help** | No | Show help like below |

```sh
# gtoj
usage: gtoj [-h] [-p [PORT]] [-c CONFIG]

optional arguments:
  -h, --help            show this help message and exit
  -p [PORT], --port [PORT]
                        Port, default: 8080
  -c CONFIG, --config CONFIG
                        Path to config file
```

## Prometheus Configuration

Add the job below to your Prometheus Configuration File to scrape the app metrics

### Open Weather

```
- job_name: 'open_weather'
    scheme: http # change to http if you don't have https
    metrics_path: '/metrics'
    static_configs:
      - targets:
        - mytarget
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:8080 # Change here with your real exporter address:port
```

## Grafana Dashboard

Import the file "openweather_dashboard.json" to your Grafana.

## Maintainer

 [Reinaldo Lima]

## License

See LICENSE file

## Acknowledgments

* [Rauklei Guimarães]: With any stupid quetion I have related to python things.
* [Mr_and_Mrs_D]: For show how to deal with command line default options.
* [Thomas Stringer]: For show how to log stuff to systemd

[//]: #

[Open Weather's API]: https://openweathermap.org/api
[python-badge]: https://img.shields.io/badge/python-3.7.5-blue
[python-version]: https://www.python.org/downloads/release/python-375/
[pylint-score]: https://mperlet.github.io/pybadge/badges/8.89.svg
[City List]: https://bulk.openweathermap.org/sample/city.list.json.gz
[Reinaldo Lima]: https://github.com/reimlima
[Rauklei Guimarães]: https://twitter.com/rauklei
[Mr_and_Mrs_D]: https://stackoverflow.com/questions/15301147/python-argparse-default-value-or-specified-value
[Thomas Stringer]: https://trstringer.com/systemd-logging-in-python/
