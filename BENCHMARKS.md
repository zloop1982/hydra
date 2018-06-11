# Benchmarks

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	1.5394 secs
  Slowest:	0.1045 secs
  Fastest:	0.0003 secs
  Average:	0.0148 secs
  Requests/sec:	6495.9987
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.011 [3636]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.021 [4464]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.032 [1281]	|■■■■■■■■■■■
  0.042 [407]	|■■■■
  0.052 [115]	|■
  0.063 [49]	|
  0.073 [22]	|
  0.084 [14]	|
  0.094 [10]	|
  0.104 [1]	|


Latency distribution:
  10% in 0.0025 secs
  25% in 0.0073 secs
  50% in 0.0144 secs
  75% in 0.0190 secs
  90% in 0.0270 secs
  95% in 0.0337 secs
  99% in 0.0519 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0003 secs, 0.1045 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0138 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0155 secs
  resp wait:	0.0141 secs, 0.0002 secs, 0.1044 secs
  resp read:	0.0004 secs, 0.0000 secs, 0.0214 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	24.3735 secs
  Slowest:	0.8469 secs
  Fastest:	0.0210 secs
  Average:	0.2360 secs
  Requests/sec:	410.2822
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.021 [1]	|
  0.104 [884]	|■■■■■■■■■■■
  0.186 [2323]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.269 [3320]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.351 [2106]	|■■■■■■■■■■■■■■■■■■■■■■■■■
  0.434 [889]	|■■■■■■■■■■■
  0.517 [286]	|■■■
  0.599 [115]	|■
  0.682 [53]	|■
  0.764 [14]	|
  0.847 [9]	|


Latency distribution:
  10% in 0.1073 secs
  25% in 0.1692 secs
  50% in 0.2154 secs
  75% in 0.2960 secs
  90% in 0.3821 secs
  95% in 0.4292 secs
  99% in 0.5854 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0210 secs, 0.8469 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0105 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0532 secs
  resp wait:	0.2354 secs, 0.0209 secs, 0.8467 secs
  resp read:	0.0003 secs, 0.0000 secs, 0.0531 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.7425 secs
  Slowest:	0.0446 secs
  Fastest:	0.0002 secs
  Average:	0.0072 secs
  Requests/sec:	13468.6682
  
  Total data:	2670000 bytes
  Size/request:	267 bytes

Response time histogram:
  0.000 [1]	|
  0.005 [3242]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.009 [3359]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.014 [2646]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.018 [510]	|■■■■■■
  0.022 [128]	|■■
  0.027 [37]	|
  0.031 [21]	|
  0.036 [39]	|
  0.040 [12]	|
  0.045 [5]	|


Latency distribution:
  10% in 0.0010 secs
  25% in 0.0033 secs
  50% in 0.0062 secs
  75% in 0.0104 secs
  90% in 0.0128 secs
  95% in 0.0151 secs
  99% in 0.0240 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0002 secs, 0.0446 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0124 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0124 secs
  resp wait:	0.0040 secs, 0.0001 secs, 0.0256 secs
  resp read:	0.0016 secs, 0.0000 secs, 0.0152 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.7160 secs
  Slowest:	0.0426 secs
  Fastest:	0.0002 secs
  Average:	0.0067 secs
  Requests/sec:	13965.8224
  
  Total data:	2650000 bytes
  Size/request:	265 bytes

Response time histogram:
  0.000 [1]	|
  0.004 [2664]	|■■■■■■■■■■■■■■■■■■■■■■
  0.009 [4759]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.013 [1907]	|■■■■■■■■■■■■■■■■
  0.017 [490]	|■■■■
  0.021 [62]	|■
  0.026 [28]	|
  0.030 [40]	|
  0.034 [13]	|
  0.038 [31]	|
  0.043 [5]	|


Latency distribution:
  10% in 0.0018 secs
  25% in 0.0039 secs
  50% in 0.0061 secs
  75% in 0.0088 secs
  90% in 0.0117 secs
  95% in 0.0137 secs
  99% in 0.0236 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0002 secs, 0.0426 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0079 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0213 secs
  resp wait:	0.0010 secs, 0.0001 secs, 0.0223 secs
  resp read:	0.0029 secs, 0.0000 secs, 0.0206 secs

Status code distribution:
  [200]	10000 responses



```