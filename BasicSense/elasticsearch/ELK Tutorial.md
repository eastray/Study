# ELK Tutorial

## 엘라스틱서치와 키바나

엘라스틱서치와 키바나 설치

엘라스틱서치 설치

```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.1.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.1.tar.gz.sha512
shasum -a 512 -c elasticsearch-6.3.1.tar.gz.sha512 
tar -xzf elasticsearch-6.3.1.tar.gz
cd elasticsearch-6.3.1/ 
```

엘라스틱서치 실행

```
./bin/elasticsearch
```

엘라스틱서치 실행 확인

```
curl -X GET localhost:9200/

>>>
{
  "name" : "vTALQFz",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "znxXiJyBRRiTNADIPLGzqA",
  "version" : {
    "number" : "6.3.1",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "eb782d0",
    "build_date" : "2018-06-29T21:59:26.107521Z",
    "build_snapshot" : false,
    "lucene_version" : "7.3.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

키바나 설치

```
wget https://artifacts.elastic.co/downloads/kibana/kibana-6.3.1-linux-x86_64.tar.gz
shasum -a 512 kibana-6.3.1-linux-x86_64.tar.gz 
tar -xzf kibana-6.3.1-linux-x86_64.tar.gz
cd kibana-6.3.1-linux-x86_64/ 
```

키바나 구성

키바나 서버는 `kibana.yml` 파일로부터의 프로퍼티를 읽는다. 기본적으로 설정 파일 위치는 어떻게 설치하느냐에 따라 다르지만, `.tar.gz`또는 `.zip`의 경우 `$KIBANA_HOME/config` 에 위치한다. 

키바나는 기본적으로 `localhost:5601`에서 운영되며, 호스트와 포트 번호를 바꾸거나, 다른 머신에서 운영중인 엘라스틱서치와 연결하기 위해서는 `kibana.yml`를 수정해야 한다.

[기본적인 키바나 설정 참조](https://www.elastic.co/guide/en/kibana/current/settings.html)

키바나 접근

키바나는 기본적으로 5601 포트를 사용하며, 웹 브라우저를 통해 접근할 수 있다.

```
localhost:5601
```

키바나 상태 체크

```
localhost:5601/status

curl -X GET http://localhost:5601/api/status

{"name":"dhkimui-MacBook-Pro.local","uuid":"05c348f7-a6a7-4252-8e7a-6436d509d23c","version":{"number":"6.3.1","build_hash":"cb8398243cee7be0dddd118cd657d1f17be4e4be","build_number":17276,"build_snapshot":false},"status":{"overall":{"state":"green","title":"Green","nickname":"Looking good","icon":"success","since":"2018-07-15T15:08:08.575Z"},"statuses":[{"id":"plugin:kibana@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.575Z"},{"id":"plugin:elasticsearch@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.009Z"},{"id":"plugin:xpack_main@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.084Z"},{"id":"plugin:searchprofiler@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.085Z"},{"id":"plugin:ml@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.086Z"},{"id":"plugin:tilemap@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.087Z"},{"id":"plugin:watcher@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.087Z"},{"id":"plugin:license_management@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.686Z"},{"id":"plugin:index_management@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.088Z"},{"id":"plugin:timelion@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.845Z"},{"id":"plugin:graph@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.089Z"},{"id":"plugin:monitoring@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.849Z"},{"id":"plugin:security@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.089Z"},{"id":"plugin:grokdebugger@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.090Z"},{"id":"plugin:dashboard_mode@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.891Z"},{"id":"plugin:logstash@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.090Z"},{"id":"plugin:apm@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.908Z"},{"id":"plugin:console@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.913Z"},{"id":"plugin:console_extensions@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.915Z"},{"id":"plugin:metrics@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:08.919Z"},{"id":"plugin:reporting@6.3.1","state":"green","icon":"success","message":"Ready","since":"2018-07-15T15:08:10.091Z"}]},"metrics":{"last_updated":"2018-07-15T15:10:22.693Z","collection_interval_in_millis":5000,"uptime_in_millis":149183,"process":{"mem":{"heap_max_in_bytes":128102400,"heap_used_in_bytes":118010744}},"os":{"cpu":{"load_average":{"1m":1.892578125,"5m":2.33056640625,"15m":1.8994140625}}},"response_times":{"avg_in_millis":null,"max_in_millis":0},"requests":{"total":0,"disconnects":0,"status_codes":{}},"concurrent_connections":6}}

```

엘라시틱서치와 키바나 연결





https://www.elastic.co/guide/en/kibana/current/connect-to-elasticsearch.html

