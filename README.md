Repository for reproducible benchmarking of database-like operations in single-node environment.  
Benchmark report is available at [h2oai.github.io/db-benchmark](https://h2oai.github.io/db-benchmark).  
We focused mainly on portability and reproducibility. Benchmark is routinely re-run to present up-to-date timings. Most of solutions used are automatically upgraded to their stable or development versions.  
This benchmark is meant to compare scalability both in data volume and data complexity.  
Contribution and feedback are very welcome!  

# Tasks

  - [x] groupby
  - [ ] join
  - [ ] sort
  - [ ] read

# Solutions

  - [x] [dask](https://github.com/dask/dask)
  - [x] [data.table](https://github.com/Rdatatable/data.table)
  - [x] [dplyr](https://github.com/tidyverse/dplyr)
  - [x] [DataFrames.jl](https://github.com/JuliaData/DataFrames.jl)
  - [x] [pandas](https://github.com/pandas-dev/pandas)
  - [x] [(py)datatable](https://github.com/h2oai/datatable)
  - [x] [spark](https://github.com/apache/spark)
  - [x] [cuDF](https://github.com/rapidsai/cudf)
  - [x] [ClickHouse](https://github.com/yandex/ClickHouse)

# Reproduce

## Batch benchmark run

- edit `path.env` and set `julia` and `java` paths
- if solution uses python create new `virtualenv` as `$solution/py-$solution`, example for `pandas` use `virtualenv pandas/py-pandas --python=/usr/bin/python3.6`
- install every solution (if needed activate each `virtualenv`)
- edit `run.conf` to define solutions and tasks to benchmark
- generate data, for `groupby` use `Rscript groupby-datagen.R 1e7 1e2 0 0` to create `G1_1e7_1e2_0_0.csv`, re-save to binary data where needed, create `data` directory and keep all data files there
- edit `data.csv` to define data sizes to benchmark using `active` flag
- start benchmark with `./run.sh`

## Single solution benchmark interactively

- generate data (see related point above)
- set data name env var, for example in `groupby` use something like `export SRC_GRP_LOCAL=G1_1e7_1e2_0_0`
- if solution uses python activate `virtualenv` of a solution
- enter interactive console and run lines of script interactively

## Exceptions

- `cuDF`
  - use `conda` instead of `virtualenv`
- `ClickHouse`
  - generate data having extra primary key column according to `clickhouse/setup-clickhouse.sh`
  - follow "reproduce interactive environment" section from `clickhouse/setup-clickhouse.sh`

# Example environment

- setting up r3-8xlarge: 244GB RAM, 32 cores: [Amazon EC2 for beginners](https://github.com/Rdatatable/data.table/wiki/Amazon-EC2-for-beginners)  
- (slightly outdated) full reproduce script on clean Ubuntu 16.04: [repro.sh](https://github.com/h2oai/db-benchmark/blob/master/repro.sh)

# Acknowledgment

- Timings for some solutions might be missing for particular datasizes or questions. Some functions are not yet implemented in all solutions so we were unable to answer all questions in all solutions. Some solutions might also run out of memory when running benchmark script which results the process to be killed by OS. Lastly we also added timeout for single benchmark script to run, once timeout value is reached script is terminated.  
