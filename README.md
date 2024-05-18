# An Empirical Evaluation of Columnar Storage Formats

This is the code for the paper titled "An Empirical Evaluation of Columnar Storage Formats" to be published in VLDB Vol 17, No 2.

## Directory structure

The code is split into several repositories. Some of them are forked from the main branch of corresponding formats and modified for profiling purposes.

```
~/EvaluationOfColumnarFormats/
    \____ OpenFormat 
           \____ benchmark (real-world data analysis and benchmark generator)
           \____ python (experiment automation scripts and plotting)
                \____ ...
                \____ cudf (experiment for GPU decoding)
           \____ vector_data (experiment scripts for ML workload)
    \____ arrow-private (including executables and profiling utilities for testing Parquet's scan performance, and also nested overhead for Parquet and ORC to Arrow)
    \____ orc (including executables and profiling utilities for testing ORC's scan and select performance)
    \____ arrow-rs (including executables for testing Parquet's select performance)
```

## Instructions

You need to follow each repo's build instructions to install the dependencies of arrow, orc, and arrow-rs, and then build each repo in release mode.

Arrow is built using cmake preset "openformat-release". An example build command:
```bash
cmake --preset openformat-release -B./out/build/openformat-release -G Ninja
cmake --build ./out/build/openformat-release --parallel 8 --target parquet-scan-columnbatch
```
If building shared libraries together with system builds causes problems, try with static build:
```bash
cmake --preset openformat-release-static -B./out/build/openformat-release-static -G Ninja -DARROW_DEPENDENCY_SOURCE=BUNDLED -DARROW_DEPENDENCY_USE_SHARED=OFF -DARROW_BUILD_SHARED=OFF
cmake --build ./out/build/openformat-release --parallel 8 --target parquet-scan-columnbatch
```

Some utilities from arrow-rs need to be installed globally (i.e., add to PATH) to run the experiments. You can go to arrow-rs directory and run `cargo install --path parquet --features=cli` to install them.

`OpenFormat` contains further instructions on how to run the workload and data generator, and how to reproduce the results.

## Pre-generated Data

We provide some pre-generated datasets (1 Mi rows and 20 columns) for testing, the urls are:

```rust
format!("{}/{}_r1000000_c20/gen_data/{}_r1000000_c20.csv", PREFIX, wl, wl)
```

where `PREFIX` is `https://d3m9osc9baovkk.cloudfront.net/` and `wl` is one of `core, bi, classic, geo, log, ml`
