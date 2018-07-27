Tutorial 1

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

키바나 서버는 `kibana.yml` 파일로부터의 프로퍼티를 읽는다. 기본적으로 설정 파일 위치는 어떻게 설치하느냐에 따라 다르지만, `.tar.gz`또는 `.zip`의 경우 `$KIBANA_HOME/config` 에 위치한다. 기본적으로 `localhost:5601`에서 운영되며, 웹 브라우저를 통해 접근할 수 있다. 호스트와 포트 번호를 바꾸거나, 다른 머신에서 운영중인 엘라스틱서치와 연결하기 위해서는 `kibana.yml`를 수정해야 한다.

[기본적인 키바나 설정 참조](https://www.elastic.co/guide/en/kibana/current/settings.html)

키바나 접근

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

키바나를 사용하기 전에 어떤 엘라스틱서치 인덱스를 탐색할 것인지 알려주어야 한다. 키바나에 접근하면, 색인  중 하나 이상의 이름과 일치하는 색인 패턴(`index pattern`)을 정의하라는 메시지가 나온다.

1. 엘라스틱서치와 키바나를 구동하고, 브라우저를 통해 키바나에 접근한다. 기본적으로 `localhost:5601` 이 기본이다. 

![키바나UI](../Image/키바나UI.png)

2. 하나 이상의 이름과 부합하는 엘라스틱서치 인덱스의 인덱스 패턴을 지정한다. 패턴에 별표(*, asterisk)를 포함할 수 있으며, 0 개 이상의 문자가 일치하게 하는 와일드카드로 사용할 수 있다. 
3. `Next Step` 버튼을 클릭하면 시간 기준 비교를 수행하는 데 사용할 타임스템프를 포함하는 인덱스 필드 선택란이 있다. 키바나는 인덱스 매핑을 읽고 타임스템프가 있는 모든 필드를 나열한다. 만일 인덱스가 시간별 데이터가 없으면, `I don't want to use the Time Filter` 옵션을 선택한다. 시간 필터는 이 필드를 사용하여 시간별로 데이터를 필터링 한다. 
4. `Create index patten` 을 클릭하여 인데스 패턴을 추가한다. 처음 추가하는 패턴의 경우 자동으로 구성이 되며, 인덱스 패턴을 추가적으로 생성하는 경우 `Management > Index Patterns` 에서 이용할 수 있다. 

-----

## Bootstrap Checks

중요한 설정을 구성하지 않으면 예기치 않은 문제를 겪는다. 엘라스틱서치의 이전 버전은 설정의 일부가 잘못 구성이 된 경우, 경고로 기록이 되었으며 사용자는 이러한 로그를 놓칠 수 밖에 없었다. 이를 예방하기 위해 엘라스틱서치는 시작할 때 부트스트랩 검사를 받는다. 부트스트랩 검사는 다양한 엘라스틱서치 및 시스템 설정을 검사한다. 엘라스틱서치가 개발 모드에 있으면 엘라스틱서치 로그에 경고로 표시되는 모든 부트스트랩 검사가 표현되며, 프로덕션 모드의 경우 부트스트랩 검사가 실패하면 엘라스틱서치가 시작되지 않는다.

-----

## Set up X-Pack

X-Pack은 보안, 경고, 모니터링, 보고, 머신 러닝 및 기타 여러 기능을 제공하는 엘라스틱서치의 확장이며, 기본적으로 엘라스틱서치를 설치하면 X-Pack이 설치된다.

Configuring Monitoring in Elasticsearch

기본적으로 X-Pack 모니터링은 사용할 수 있지만 데이터 수집은 불가능하다. 고급 모니터링 설정을 사용하면 데이터 수집 빈도 제어, 제한 시간 설정, 로컬에 저장된 모니터링 인덱스의 보존 기간 설정 등이 가능한다. 모니터링 데이터가 표시되는 방법을 조정할 수도 있다.

Configuring security in Elasticsearch

X-Pack 보안을 사용하면 클러스터를 쉽게 보호할 수 있다. X-Pack 보안을 통해 데이터를 암호로 보호하고, 통신 암호화, 역할 기반(role-based) 액세스 제어, IP 필터링 및 회계 감사 등의 고급 보안 수단을 구현할 수 있다.

보안은 엘라스틱서치 클러스터를 다음과 같이 보호한다.

- 암호 보호, 역할 기반 액세스 제어 및 IP 필터링을 통한 무허가 액세스 방지
- 메시지 인증 및 SSL/TLS 암호화로 데이터 무결성 유지
- 감사 추적을 위지 관리하여 클러스터의 대상과 클러스터에 저장된 데이터 확인

Configuring X-Pack Java Clients

X-Pack이 설치된 클러스터에서 자바 전송 클라이언트를 사용하려면 X-Pack 전송 클라이언트를 다운로드하여 구성해야 한다.

```
X-Pack은 엘라스틱서치, 키바나, 로그스테이시, 비트(Beats)에 단일 팩으로 설치됨으로써, 엘라스틱서치에 위치하는 데이터의 보안이나 키바나를 통한 로그인 화면 추가 등의 추가적인 기능을 제공한다.
```

-----

### How monitoring works

모니터링은 엘라스틱서치 노드, 로그스테이시 노드, 키바나 인스턴스를 통해 데이터를 모은다. 모니터링 중인 엘라스틱서치 클러스터는 전체 스택에 대한 모니터링 메트릭이 저장되는 위치를 제어한다. 기본적으로 로컬 인덱스에 저장되며, 프로덕션 환경에서는 별도의 모니터링 클러스터를 사용하는 것이 좋다. 별도의 모니터링 클러스터를 사용하면 프로덕션 클러스터가 중단해도 모니터링 데이터에 접근하는데에는 영향을 미치지 않는다. 또한 모니터링 활동이 프로덕션 클러스터의 성능에 영향을 주는 것을 방지할 수 있다.

아래의 다이어그램에서는 별도의 프로덕션 및 모니터링 클러스터가 있는 일반적인 모니터링 아키텍처이다.

![엘라스틱서치모니터링아키텍처1](../Image/엘라스틱서치모니터링아키텍처1.png)

만일 골드 이상의 라이선스를 가지고 있다면, 하나의 모니터링 클러스터와 다수의 프로덕션 클러스터를 구성할 수 있다.

키바나가 스택의 이반 부분인 경우 단일 키바나 인스턴스를 사용하여 프로덕션 클러스터와 모니터링 클러스터에 모두 액세스하는 대신 모니터링용 키바나 인스턴스를 생성할 수 있다.

![엘라스틱서치모니터링아키텍처2](../Image/엘라스틱서치모니터링아키텍처2.png)

[How monitoring works](https://www.elastic.co/guide/en/elastic-stack-overview/6.3/how-monitoring-works.html)

