# Logstash Plugin

This is a plugin for [Logstash](https://github.com/elastic/logstash). This plugin behave like a [fluent-plugin-dstat](https://github.com/shun0102/fluent-plugin-dstat) on logsash.


## install

- Install plugin
```sh
bin/logstash-plugin install logstash-input-dstat
```

## Configuration

```
input {
  dstat {
    option => "-c"
    interval => 30
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
  }
}
```

### parameters

* option
    * option for dstat
    * default: empty
* interval
    * interval for executing dstat (seconds)
    * default: 30
* stat_hash
    * set custom stat mapping
    * ex.) stat_hash => {'total cpu usage' => {'usr' => 'hogehoge' 'sys' => 'fugafuga'}}

### default stat mapping

``` ruby
{
  'load avg' => {'1m' => 'loadavg-short', '5m' => 'loadavg-middle', '15m' => 'loadavg-long'},
  'total cpu usage' => {'usr' => 'cpu-usr', 'sys' => 'cpu-sys', 'idl' => 'cpu-idl', 'wai' => 'cpu-wai', 'hiq' => 'cpu-hiq', 'siq' => 'cpu-siq'},
  'net/total' => {'recv' => 'net-recv', 'send' => 'net-send'},
  '/' => {'used' => 'disk-used', 'free' => 'disk-free'},
  'memory usage' => {'used' => 'mem-used', 'buff' => 'mem-buff', 'cach' => 'mem-cach', 'free' => 'mem-free'},
  'dsk/total' => {'read' => 'dsk-read', 'writ' => 'dsk-writ'},
  'paging' => {'in' => 'paging-in', 'out' => 'paging-out'},
  'system' => {'int' => 'sys-int', 'csw' => 'sys-csw'},
  'swap' => {'used' => 'swap-used', 'free' => 'swap-free'},
  'procs' => {'run' => 'procs-run', 'blk' => 'procs-blk', 'new' => 'procs-new'}
}
```

### Output

Output example of when using logstash-output-elasticsearch.


``` json
curl -XGET localhost:9200/logstash-*/_search

{
  "took": 32,
  "timed_out": false,
  "_shards": {
    "total": 3,
    "successful": 3,
    "failed": 0
  },
  "hits": {
    "total": 2,
    "max_score": 3.2686834,
    "hits": [
      {
        "_index": "logstash-2016.07.06",
        "_type": "logs",
        "_id": "AVW-_erHoa7OXPDibi4W",
        "_score": 3.2686834,
        "_source": {
          "stat": "cpu-usr",
          "value": "73.810",
          "host": "localhost.localdomain",
          "@version": "1",
          "@timestamp": "2016-07-06T06:55:58.890Z"
        }
      },
      {
        "_index": "logstash-2016.07.06",
        "_type": "logs",
        "_id": "AVW-_hX2oa7OXPDibi4c",
        "_score": 3.2686834,
        "_source": {
          "stat": "cpu-usr",
          "value": "0.0",
          "host": "localhost.localdomain",
          "@version": "1",
          "@timestamp": "2016-07-06T06:56:09.950Z"
        }
      }
    ]
  }
}
```
