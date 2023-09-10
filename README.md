# MLflow adapter for CrateDB


## About

An adapter wrapper for [MLflow] to use [CrateDB] as a storage database
for [MLflow Tracking].


## Setup

Install the most recent version of the `mlflow-cratedb` package.
```shell
pip install --upgrade 'git+https://github.com/crate-workbench/mlflow-cratedb#egg=mlflow-cratedb[examples]'
```

To verify if the installation worked, you can inspect the version numbers
of the software components you just installed.
```shell
mlflow-cratedb --version
mlflow-cratedb cratedb --version
```


## Usage

This documentation section explains how to use this software successfully,
please read it carefully.

### Introduction

In order to spin up a CrateDB instance without further ado, you can use
Docker or Podman.
```shell
docker run --rm -it --publish=4200:4200 --publish=5432:5432 \
  --env=CRATE_HEAP_SIZE=4g crate \
  -Cdiscovery.type=single-node
```

The repository includes a few [example programs](./examples), which can be used
to exercise the MLflow setup, and to get started.

The `MLFLOW_TRACKING_URI` environment variable defines whether to record outcomes
directly into the database, or by submitting them to an MLflow Tracking Server.

```shell
# Use CrateDB database directly
export MLFLOW_TRACKING_URI="crate://crate@localhost/?schema=examples"

# Use MLflow Tracking Server
export MLFLOW_TRACKING_URI=http://127.0.0.1:5000
```

### Standalone

In order to instruct MLflow to submit the experiment metadata directly to CrateDB,
configure the `MLFLOW_TRACKING_URI` environment variable to point to your CrateDB
server.

```shell
export MLFLOW_TRACKING_URI="crate://crate@localhost/?schema=mlflow"
python examples/tracking_dummy.py
```

### Tracking Server

Start the MLflow server, pointing it to your CrateDB instance, running on
`localhost`.
```shell
mlflow-cratedb server --backend-store-uri='crate://crate@localhost/?schema=mlflow' --dev
```

In order to instruct MLflow to submit the experiment metadata to the MLflow Tracking
Server, configure the `MLFLOW_TRACKING_URI` environment variable to point to it.

```shell
export MLFLOW_TRACKING_URI="http://127.0.0.1:5000"
python examples/tracking_dummy.py
```

### Remarks

Please note that you need to invoke the `mlflow-cratedb` command, which
runs MLflow amalgamated with the necessary changes to support CrateDB.

Also note that we recommend to use a dedicated schema for storing MLflow's
tables. In that spirit, CrateDB's default schema `"doc"` is not populated
by any tables of 3rd-party systems.


## Development

Acquire source code and install development sandbox. The authors recommend to
use a Python virtualenv.
```shell
git clone https://github.com/crate-workbench/mlflow-cratedb
cd mlflow-cratedb
python3 -m venv .venv
source .venv/bin/activate
pip install --editable='.[examples,develop,docs,test]'
```

Run linters and software tests, skipping slow tests:
```shell
poe check-fast
```

Exclusively run "slow" tests.
```shell
pytest -m slow
```


## Acknowledgements

[Siddharth Murching], [Corey Zumar], [Harutaka Kawamura], [Ben Wilson], and
all other contributors for conceiving and maintaining [MLflow].

[Andreas Nigg] for contributing the [tracking_merlion.py](./examples/tracking_merlion.py)
ML experiment program, which is using [Merlion].


[Andreas Nigg]: https://github.com/andnig
[Ben Wilson]: https://github.com/BenWilson2
[Corey Zumar]: https://github.com/dbczumar
[CrateDB]: https://github.com/crate/crate
[CrateDB Cloud]: https://console.cratedb.cloud/
[Harutaka Kawamura]: https://github.com/harupy
[Merlion]: https://github.com/salesforce/Merlion
[MLflow]: https://mlflow.org/
[MLflow Tracking]: https://mlflow.org/docs/latest/tracking.html
[Siddharth Murching]: https://github.com/smurching
