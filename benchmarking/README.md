# Benchmarking

This folder contains a set of tools to benchmark the gRPC and REST APIs of
`mlserver`.
These load tests are run locally against a local server.

## Current results

|      | Requests/sec | Average (ms) | Slowest (ms) | Fastest (ms) |
| ---- | ------------ | ------------ | ------------ | ------------ |
| gRPC | 3031.56      | 32.93        | 78.90        | 0.92         |
| REST | 1849.48      | 54.0         | 168.4        | 16.4         |

## Setup

The gRPC benchmark uses [`ghz`](https://ghz.sh/) and the HTTP benchmark uses
[`hey`](https://github.com/rakyll/hey).
To install the pre-requisites, you can run:

```shell
make install-dev
```

## Data

You can find pre-generated requests under the [`/data`](./data) folder.
These are formed by payloads with a single input tensor, which varies in length from `1024` to `65536`.

> **NOTE**: To work around some limitations of
> [`hey`](https://github.com/rakyll/hey), we will only be using the smallest
> payload (i.e. with `1024` tensors) as the payload for both gRPC and HTTP.

### Generate

You can re-generate the test requests by using the
[`generator.py`](./generator.py) script:

```shell
python generator.py
```

## Usage

For any load test, the first step will be to spin up the test server, which we
can do with:

```shell
make start-testserver
```

The test server will start both the REST and gRPC APIs and will pre-load a test
model which sums over all the elements of the input and returns the total as a
result.

### HTTP

To run the HTTP benchmark:

```shell
make benchmark-rest
```

This will run 100000 requests across 100 workers.

### gRPC

To run the gRPC benchmark:

```shell
make benchmark-grpc
```

This will run 100000 requests across 50 connections shared by 100 workers.
