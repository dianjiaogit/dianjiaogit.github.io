---
layout: post
title:  "LogQL 学习笔记"
date:   2021-11-22 00:37:39 +0800
categories: [Work]
---

### Arithmetic operators

- \+ (addition)
- \- (subtraction)
- \* (multiplication)
- / (division)
- % (modulo)
- ^ (power/exponentiation)

### Logical and set operators

- and (intersection)
- or (union)
- unless (complement)

### Comparison operators

- == (equality)
- != (inequality)
- \> (greater than)
- \>= (greater than or equal to)
- < (less than)
- <= (less than or equal to)


# Log queries

All LogQL queries contain a log stream selector. Optionally, the log stream selector can be followed by a log pipeline.

example:
```
{container="query-frontend",namespace="loki-dev"} |= "metrics.go" | logfmt | duration > 10s and throughput_mb < 500
```




## Log Stream Selector


label matching operator:
- =: exactly equal
- !=: not equal
- =~: regex matches
- !~: regex does not match  

```
{app="mysql",name="mysql-backup"}
```

## Log pipeline


Log pipeline expressions fall into one of three categories:

- Filtering expressions: line filter expressions and label filter expressions
- Parsing expressions
- Formatting expressions: line format expressions and label format expressions

### Line filter expression

- \|=: Log line contains string
- !=: Log line does not contain string
- \|~: Log line contains a match to the regular expression
- !~: Log line does not contain a match to the regular expression  

```
{job="mysql"} |= "error" != "timeout"
```

### Parser expression


### JSON

For instance, the pipeline \| json will produce the following mapping:

```
{ "a.b": {c: "d"}, e: "f" }
```

->
```
{a_b_c="d", e="f"}
```
1. without parameters \| json:  
Note: Arrays are skipped.  
2. with parameters \| json label="expression", another="expression"  
https://grafana.svc.monitoring.gke.mintelcloud.io/goto/wOXzVl57z

### logfmt

For example the following log line:
```
at=info method=GET path=/ host=grafana.net fwd="124.133.124.161" service=8ms status=200
```
will get those labels extracted:
```
"at" => "info"
"method" => "GET"
"path" => "/"
"host" => "grafana.net"
"fwd" => "124.133.124.161"
"service" => "8ms"
"status" => "200"
```

### Pattern

Consider this NGINX log line.  

```
0.191.12.2 - - [10/Jun/2021:09:14:29 +0000] "GET /api/plugins/versioncheck HTTP/1.1" 200 2 "-" "Go-http-client/2.0" "13.76.247.102, 34.120.177.193" "TLSv1.2" "US" ""
```
This log line can be parsed with the expression  
\<ip\> - - \<_\> "\<method\> \<uri\> \<_\>" \<status\> \<size\> \<_\> "\<agent\>" \<_\>  
to extract these fields:  

```
"ip" => "0.191.12.2"
"method" => "GET"
"uri" => "/api/plugins/versioncheck"
"status" => "200"
"size" => "2"
"agent" => "Go-http-client/2.0"
```

### Regular expression

For example the parser `| regexp "(?P<method>\\w+) (?P<path>[\\w|/]+) \\((?P<status>\\d+?)\\) (?P<duration>.*)"`will extract from the following line:
```
POST /api/prom/api/v1/query_range (200) 1.5s
```
those labels:
```
"method" => "POST"
"path" => "/api/prom/api/v1/query_range"
"status" => "200"
"duration" => "1.5s"
```

### Label filter expression

A predicate contains a label identifier, an operation and a value to compare the label with.
```
cluster="namespace"
```
Support multiple value types which are automatically inferred from the query input.
- String is double quoted or backticked such as "200" or `us-central1`.
- Duration is a sequence of decimal numbers, each with optional fraction and a unit suffix, such as “300ms”, “1.5h” or “2h45m”. Valid time units are “ns”, “us” (or “µs”), “ms”, “s”, “m”, “h”.
- Number are floating-point number (64bits), such as250, 89.923.
- Bytes is a sequence of decimal numbers, each with optional fraction and a unit suffix, such as “42MB”, “1.5Kib” or “20b”. Valid bytes units are “b”, “kib”, “kb”, “mib”, “mb”, “gib”, “gb”, “tib”, “tb”, “pib”, “pb”, “eib”, “eb”.

Using Duration, Number and Bytes will convert the label value prior to comparision and support the following comparators:
- == or = for equality.
- != for inequality.
- \> and \>= for greater than and greater than or equal.
- < and <= for lesser than and lesser than or equal.  

```
logfmt | duration > 1m and bytes_consumed > 20MB
```

### Line format expression

The line format expression can rewrite the log line content by using the text/template format. It takes a single string parameter `| line_format "{ {.label_name} }"`, which is the template format. All labels are injected variables into the template and are available to use with the \{\{.label_name}} notation.

example:
If we have the following labels `ip=1.1.1.1`, `status=200` and `duration=3000(ms)`, we can divide the duration by 1000 to get the value in seconds.  

```
{container="frontend"} | logfmt | line_format "{ {.ip} } { {.status} } { {div .duration 1000} }"
```

### Labels format expression

The `| label_format` expression can rename, modify or add labels. It takes as parameter a comma separated list of equality operations, enabling multiple operations at once.
Note: 
The renaming form dst=src will drop the src label after remapping it to the dst label. However, the template form will preserve the referenced labels, such that dst="\{\{.src\}\}" results in both dst and src having the same value.


# Examples:


## Multiple filtering

```
{cluster="ops-tools1", namespace="loki-dev", job="loki-dev/query-frontend"} |= "metrics.go" !="out of order" | logfmt | duration > 30s or status_code!="200"
```

## Multiple parsers

```
level=debug ts=2020-10-02T10:10:42.092268913Z caller=logging.go:66 traceID=a9d4d8a928d8db1 msg="POST /api/prom/api/v1/query_range (200) 1.5s"
```

```
{job="cortex-ops/query-frontend"} | logfmt | line_format "{ {.msg} }" | regexp "(?P<method>\\w+) (?P<path>[\\w|/]+) \\((?P<status>\\d+?)\\) (?P<duration>.*)"
```
