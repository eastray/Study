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

## X-Pack APIs

X-Pack은 광범위한 REST API를 제공하여 기능을 관리하고 모니터링 한다.

### Info API

Info API는 설치된 X-Pakc 기능에 대한 일반 정보를 제공한다.

`GET /_xpack`

```
curl -X GET "lcalhost:9200/_xpack?pretty"

---output---
{
  "build" : {
    "hash" : "053779d",
    "date" : "2018-07-20T05:25:16.206115Z"
  },
  "license" : {
    "uid" : "a1fe4b15-aee6-4fc8-aa3c-0a101a5619d0",
    "type" : "basic",
    "mode" : "basic",
    "status" : "active"
  },
  "features" : {
    "graph" : {
      "description" : "Graph Data Exploration for the Elastic Stack",
      "available" : false,
      "enabled" : true
    },
    "logstash" : {
      "description" : "Logstash management component for X-Pack",
      "available" : false,
      "enabled" : true
    },
    "ml" : {
      "description" : "Machine Learning for the Elastic Stack",
      "available" : false,
      "enabled" : true,
      "native_code_info" : {
        "version" : "6.3.2",
        "build_hash" : "903094f295d249"
      }
    },
    "monitoring" : {
      "description" : "Monitoring for the Elastic Stack",
      "available" : true,
      "enabled" : true
    },
    "rollup" : {
      "description" : "Time series pre-aggregation and rollup",
      "available" : true,
      "enabled" : true
    },
    "security" : {
      "description" : "Security for the Elastic Stack",
      "available" : false,
      "enabled" : true
    },
    "watcher" : {
      "description" : "Alerting, Notification and Automation for the Elastic Stack",
      "available" : false,
      "enabled" : true
    }
  },
  "tagline" : "You know, for X"
}
```

- build: 빌드 번호와 타임스템프 제공
- license: 현재 설치된 라이센스 정보 제공
- features: 현재 라이센스에서 사용 할 수 있거나 사용 가능한 기능 제공

사용할 수 있는 파라미터는 아래와 같다.

- categories: 선택적으로 정보를 볼 수 있으며, 콤마(,)를 통해 다중 선택이 가능하다.
  - `curl -X GET "localhost:9200/_xpack?categories=build,features&pretty"`
- human: 속성 중 설명을 제거한다. 기본 값이 `true`이다.
  - `curl -X GET "localhost:9200/_xpack?categories=build,features&pretty&human=false"`

### 



-----

### How monitoring works

모니터링은 엘라스틱서치 노드, 로그스테이시 노드, 키바나 인스턴스를 통해 데이터를 모은다. 모니터링 중인 엘라스틱서치 클러스터는 전체 스택에 대한 모니터링 메트릭이 저장되는 위치를 제어한다. 기본적으로 로컬 인덱스에 저장되며, 프로덕션 환경에서는 별도의 모니터링 클러스터를 사용하는 것이 좋다. 별도의 모니터링 클러스터를 사용하면 프로덕션 클러스터가 중단해도 모니터링 데이터에 접근하는데에는 영향을 미치지 않는다. 또한 모니터링 활동이 프로덕션 클러스터의 성능에 영향을 주는 것을 방지할 수 있다.

아래의 다이어그램에서는 별도의 프로덕션 및 모니터링 클러스터가 있는 일반적인 모니터링 아키텍처이다.

![엘라스틱서치모니터링아키텍처1](../Image/엘라스틱서치모니터링아키텍처1.png)

만일 골드 이상의 라이선스를 가지고 있다면, 하나의 모니터링 클러스터와 다수의 프로덕션 클러스터를 구성할 수 있다.

키바나가 스택의 일부분인 경우 단일 키바나 인스턴스를 사용하여 프로덕션 클러스터와 모니터링 클러스터에 모두 액세스하는 대신 모니터링용 키바나 인스턴스를 생성할 수 있다.

![엘라스틱서치모니터링아키텍처2](../Image/엘라스틱서치모니터링아키텍처2.png)

----

## Securing the Elastic Stack

X-Pack 보안을 사용하면 클러스터를 쉽게 보호할 수 있다. 보안을 통해 데이터를 암호로 보호하고 통신 암호화, 역할 기반 액세스 제어, IP 필터링 및 감사와 같은 고급 보안 수단을 구현할 수 있다.

- 암호 보호, 역할 기반 액세스 제어 및 IP 필터링을 통한 무단 액세스 방지
- 메시지 인증 및 SSL/TLS 암호화로 데이터 무결성 유지
- 감사 추적을 유지 관리하여 클러스터일의 대상과 클러스터에 지정된 데이터 확인

### Preventing Unauthorized Access

엘라스틱서치 클러스터에 대한 무단 액세스 방지를 위해 사용자를 이정하는 방법이 필요하다. 이것은 단순히 사용자가 자신이 주장하는 사람임을 확인하는 방법이 필요하다는 의미이며, 특정 인물이 특정 사용자로서 로그인을 할 수 있도록 해야 한다.

X-Pack 보안은 클러스터를 신속하게 암호로 보호할 수 있는 독립형 인증 메커니즘를 제공한다. LDAP, Active Directory, PKI를 사용하여 조직의 사용자를 관리하는 경우, X-Pack 보안은 해당 시스템과 통합되어 사용자 인증을 수행 할 수 있다.

사용자가 액세스할 수 있는 데이터와 수행 할 수 있는 작업을 제어하는 방법이 필요하며, X-Pack 보안을 사용하면 역할에 액세스 권한을 할당하고 해당 역할을 사용자에게 할당하여 사용자를 인증할 수 있다. 또한 IP 기반 이증을 지원하여, 특정 IP 주소 또는 서브넷을 허용 또는 차단하여 서버에 대한 네트워크 수준의 액세스를 제어할 수 있다.

### Preserving Data Integrity (데이터 무결성 유지)

보안은 기밀 데이터를 기밀로 유지하는 것이 중요하며, 엘라스틱서치에서는 실수로 인한 데이터 손실 및 손상 상지 기능이 내장되어 있다. 노드와의 통신을 암호화하여 데이터 무결성을 보존하고, 암호화 강도를 높이거나 클라이언트 트래픽을 노드 간 통신과 분리할 수도 있다.

### Maintaining an Audit Trail

감사 추적을 유지하기 위해 X-Pack 보안을 사용하면 클러스터에 액세스하는 사용자와 수행중인 사용자를 쉽게 볼 수 있고, 액세스 패턴과 클러스터 액세스 실패를 분석하여 시도한 공격과 데이터 유출에 대한 통찰력을 얻을 수 있다. 

```
감사 추적(Audit Trail)
감사를 위해 입력된 데이터가 어떤 변환 과정을 거쳐 출력되는지의 과정을 기록하여 추적하는 방법.
하나의 처리 과정 또는 하드웨어의 고장, 정전 동안에 일어나는 입출력 오류를 추적하고 각 단계의 이상 유무를 감정하는데 사용된다.
```

-----

## How Monitoring works

모니터링은 엘라스틱, 로그스테이시 및 키바나 인스턴스에서 데이터를 수집하며, 모니터링 중인 엘라스틱서치 클러스터는 전체 스택에 대한 모니터링 메트릭이 저장되는 위치를 제어한다. 기본적으로 로컬 인덱스에 저장되며, 프로덕션 환경에서는 별도의 모니터링 클러스터를 사용하는 것이 좋다. 별도의 모니터링 클러스터를 사용하면 프로덕션 클러스터의 중단을 방지할 수 있으며, 모니터링 활동이 프로덕션 클러스터의 성능에 영향을 주는 것을 방지할 수 있다.

```
일반적으로 모니터링 클러스터와 모니터링되는 클러스터는 동일한 버전의 스택을 실행해야 하며, 모니터링 클러스터는 새 버전의 스택을 실행하는 프로덕션 클러스터를 모니터할 수 없다.
```

별도의 프로덕션 클러스터와 모니터링 클러스터가 있는 일반적인 모니터링 아키텍처는 아래와 같다.

![전형적인 모니터링 아키텍처](../Image/전형적인 모니터링 아키텍처.png)

----

## Elasticsearch REST API

엘라스틱서치는 클러스터와의 상호 작용에 사용할 수 있는 REST API를 제공한다. 이 API를 통해 다양한 작업들을 수행할 수 있다.

- 클러스터, 노드, 색인의 상태 및 통계 정보 확인
- 클러스터, 노드, 색인의 데이터 및 메타데이터 관리
- 색인에 대한 CRUD 및 검색 작업 수행
- 페이징, 정렬, 필터링, 스크립팅, 집계 등의 여러 검색 작업 실행

키바나의 콘솔 플러그인은 엘라스틱 서치의 REST API와 상호 작용할 수 있는 UI를 제공하며, 콘솔에는 엘라스틱서치에게 요청을 작업하는 편집기와 요청에 대한 응답을 표시하는 응답 창으로 나뉜다.

콘솔은 cURL과 유사한 구문으로 명령을 이해하며, 아래의 예제는 콘솔 명령과 엘라스틱서치의 _search API의 GET 요청과 같은 의미이다.

```
GET /_search
{
  "query": {
    "match_all": {}
  }
}
------------------------------------------------------------------------
curl -XGET "http://localhost:9200/_search" -d'
{
  "query": {
    "match_all": {}
  }
}
```

-----

## 인덱스 생성

도큐먼트를 넣어서 인덱스를 자동으로 생성할 수 있지만, 샤드 수 또 리플리카의 수 등의 매핑 정보를 지정(엘라스틱서치에서 사용하는 스키마) 할 수 있다. 인덱스의 매핑 정보를 지정할 때는 아래와 같이 별도로 생성해 주어야 한다.

```
PUT library
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}

PUT work
{
  "settings": {
    "number_of_shards": 5, 
    "number_of_replicas": 1
  }
}
```

-----

## 인덱스 검색

생성한 인덱스를 아래와 같이 검색할 수 있다. 여기서 match_all은 모든 인덱스를 검색할 때 사용하는 쿼리로, 검색의 기본 값이다. 별도의 스코어 계산을 하지 않는다.

```
GET library/_search
{
  "query": {
    "match_all": {}
  }
}
--- output---
{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 0,
    "max_score": null,
    "hits": []
  }
}

------

GET work/_search
{
  "query": {
    "match_all": {}
  }
}

--- output ---
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 1,
    "max_score": 1,
    "hits": [
      {
        "_index": "work",
        "_type": "books",
        "_id": "PI3pO2UBUQBeYSwbzDVR",
        "_score": 1,
        "_source": {
          "query": {
            "match_all": {}
          }
        }
      }
    ]
  }
}
```

-----

## Bulk 색인

대량의 도큐먼트를 색인할 때 사용하는 api로, 색인 생성 속도가 빨라질 수 있다. 엔드 포인트는 /_bulk 이어야 하며, 다음 줄에서 JSON 구조의 필드가 필요하다.

```
POST library/books/_bulk
{"index": {"_id": 1}}
{"title": "The quick brow fox", "price": 5, "colors":["red", "green", "blue"]}
{"index":{"_id": 2}}
{"title":"The quick brow fox jumps over the lazy dog", "price": 15, "colors":["blue", "yellow"]}
{"index":{"_id": 3}}
{"title":"The quick brow fox jumps over the quick dog", "price": 8, "colors":["red", "blue"]}
{"index":{"_id": 4}}
{"title":"brow fox brown dog", "price": 2, "colors":["black", "yellow", "red", "blue"]}
{"index":{"_id": 5}}
{"title":"Lazy dog", "price": 9, "colors":["red", "blue", "green"]}
```

## 개별 도큐멘트 조회

```
GET library/books/1
{
  "_index": "library",
  "_type": "books",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": {
    "title": "The quick brow fox",
    "price": 5,
    "colors": [
      "red",
      "green",
      "blue"
    ]
  }
}
```

-----

## 매핑 정보 검색

인덱스 또는 인덱스 / 타입에 대한 매핑 정의를 검색할 수 있다. 키워드 타입은 어그리케이션에서 사용된다. 

```
GET library/_mapping

--- output ---
{
  "library": {
    "mappings": {
      "books": {
        "properties": {
          "colors": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "price": {
            "type": "long"
          },
          "title": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
      }
    }
  }
}
```

## 특정 단어가 포함된 도큐먼트 검색

```
GET library/_search
{
  "query": {
    "match": {
      "title": "fox"
    }
  }
}

--- output ---
{
  "took": 23,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 4,
    "max_score": 0.32575765,
    "hits": [
      {
        "_index": "library",
        "_type": "books",
        "_id": "1",
        "_score": 0.32575765,
        "_source": {
          "title": "The quick brow fox",
          "price": 5,
          "colors": [
            "red",
            "green",
            "blue"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "4",
        "_score": 0.32575765,
        "_source": {
          "title": "brow fox brown dog",
          "price": 2,
          "colors": [
            "black",
            "yellow",
            "red",
            "blue"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "2",
        "_score": 0.23044494,
        "_source": {
          "title": "The quick brow fox jumps over the lazy dog",
          "price": 15,
          "colors": [
            "blue",
            "yellow"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "3",
        "_score": 0.23044494,
        "_source": {
          "title": "The quick brow fox jumps over the quick dog",
          "price": 8,
          "colors": [
            "red",
            "blue"
          ]
        }
      }
    ]
  }
}
```

기본적으로 `match` 쿼리를 사용하여 검색할 수 있으며, 띄어쓰기를 통해 `or` 검색이 가능하다.

```
GET library/_search
{
  "query": {
    "match": {
      "title": "quick dog"
    }
  }
}

--- output ---
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 5,
    "max_score": 0.8634703,
    "hits": [
      {
        "_index": "library",
        "_type": "books",
        "_id": "3",
        "_score": 0.8634703,
        "_source": {
          "title": "The quick brow fox jumps over the quick dog",
          "price": 8,
          "colors": [
            "red",
            "blue"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "2",
        "_score": 0.66220284,
        "_source": {
          "title": "The quick brow fox jumps over the lazy dog",
          "price": 15,
          "colors": [
            "blue",
            "yellow"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "1",
        "_score": 0.6103343,
        "_source": {
          "title": "The quick brow fox",
          "price": 5,
          "colors": [
            "red",
            "green",
            "blue"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "5",
        "_score": 0.39033517,
        "_source": {
          "title": "Lazy dog",
          "price": 9,
          "colors": [
            "red",
            "blue",
            "green"
          ]
        }
      },
      {
        "_index": "library",
        "_type": "books",
        "_id": "4",
        "_score": 0.32575765,
        "_source": {
          "title": "brow fox brown dog",
          "price": 2,
          "colors": [
            "black",
            "yellow",
            "red",
            "blue"
          ]
        }
      }
    ]
  }
}
```

특정 단어가 들어간 도큐멘트가 모두 검색되며, `_score` 값을 보면 모두 값이 상이하다. 검색하고자 하는 특정 단어가 가지고 있는 도큐멘트와 더 많이 부합할 경우, `_score` 의 점수가 올라가며 검색에서 맨 위에 나오게 된다. 

특정 단어가 아닌 정확한 구문이 포함된 도큐멘트를 검색하는 경우 `match_phrase` 쿼리를 사용한다.

```
GET library/_search
{
  "query": {
    "match_phrase": {
      "title": "quick dog"
    }
  }
}

--- output ---
{
  "took": 15,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 1,
    "max_score": 0.6622029,
    "hits": [
      {
        "_index": "library",
        "_type": "books",
        "_id": "3",
        "_score": 0.6622029,
        "_source": {
          "title": "The quick brow fox jumps over the quick dog",
          "price": 8,
          "colors": [
            "red",
            "blue"
          ]
        }
      }
    ]
  }
}
```

엘라스틱서치에서는 relevance (정확도) 알고리즘을 사용하며, 이를 이용한 랭킹을 적용할 수 있다. 랭킹은 스코어를 기반으로 정렬되며, 엘라스틱서치에서는 랭킹 알고리즘이 중요하며, 일반적으로 Term Frequency(TF)와 Inverse Document Frequency(IDF)를 많이 사용한다.

- Term Frequency: 찾는 검색어가 문서에 많을 수록 해당 문서의 정확도를 높인다.
- Inverse Document Frequency: 전체 문서에서 많이 출현한 단어일수록 점수가 낮다.

-----

## bool 쿼리

bool 쿼리를 통해 다른 쿼리를 사용할 수 있으며, must, should, must_not을 제공한다.

- must : and 조건으로 must에 있는 것 사용
- must_not: 포함하지 않는 문서 검색
- should: 기본적인 개념은 or 조건과 비슷하나, 매칭되면 더 높은 스코어가 부여된다 가중치 조걸이 가능하며 "boost" 를 톨해 가중치를 조정할 수 있다.

```
GET library/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "quick"
          }
        },
        {
          "match_phrase": {
            "title": "lazy dog"
          }
        }
      ]
    }
  }
}
```

```
GET library/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "title": "lazy"
          }
        },
        {
          "match_phrase": {
            "title": "quick dog"
          }
        }
      ]
    }
  }
}
```

```
GET library/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match_phrase": {
            "title": "quick dog"
          }
        },
        {
          "match_phrase": {
            "title": {
              "query": "lazy dog",
              "boost": 3
            }
          }
        }
      ]
    }
  }
}
```

### must와 should를 복합 사용

우선순위는 must가 먼저, should는 매칭될 필요는 없지만 가중치를 조절할 수 있다.

```
GET library/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "title": "lazy"
          }
        }
      ],
      "must": [
        {
          "match": {
            "title": "dog"
          }
        }
      ]
    }
  }
}
```

결과는 기본적으로 must를 통해 특정 단어가 항상 들어가 있으며, 그중 should를 통해 지정한 단어가 들어간 도큐멘트의 경우 가중치를 높인다.

-----

## Highlight

검색 값이 크고 여러 필드를 사용하는 경우 유용하다.

```
GET /library/_search
{
  "query": {
    "bool": {
      "should": [
        {
            "match_phrase": {
            "title": {
              "query": "quick dog",
              "boost": 2
            }
          }
        },
        {
          "match_phrase": {
            "title": "lazy dog"
          }
        }
      ]
    }
  },
  "highlight": {
    "fields": {
      "title": {}
    }
  }
}
```

-----

## filter

특정 필드에서 지정한 텀이 들어있는 문서와 매치된다. 여기서 텀은 각각의 고유한 검색어를 가리키는 용어이다. 스코거 계산 없이 캐싱되며, 속도가 빠르다.

필터의 경우 bool 조건이 해당할 경우에만 사용된다. (must + filter)

```
GET /library/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "dog"
          }
        }
      ],
      "filter": {
        "range": {
          "price": {
            "gte": 5,
            "lte": 10
          }
        }
      }
    }
  }
}
```

특정 단어를 가지며, 문서의 요소 중 5 보다 크고 10보다 작은 도큐멘트를 가져온다.

### 필터만 사용하고 스코어가 필요없는 경우

```
GET library/_search
{
  "query": {
    "bool": {
      "filter": {
        "range": {
          "price": {
            "gt": 8
          }
        }
      }
    }
  }
}
```

gt는 초과를 의미하며, gte는 이상을 의미한다.

## Analysis(_analyze)

텍스트 필드를 가공하여 검색에 사용할 수 잇는 값으로 바꾸어 준다.

```
GET library/_analyze
{
  "tokenizer": "standard",
  "text": "Brown fox brown dog"
}
```

분석 과정은 토크나이저()와 토큰 필터로 구성되며, 토크나이저는 데이터를 분리하는 역할을 하며 이렇게 나누어진 단어를 텀(term)이라 한다. 토큰 필터는 텀들을 특정 조건으로 재가공되는 과정이다. 

```
GET library/_analyze
{
  "tokenizer": "standard",
  "filter": ["lowercase"], 
  "text": "Brown fox brown dog"
}
```

lowecase: 소문자로 변경

unique: 중복되는 텀을 제거



토크나이저와 필터 대신 아날라이저를 사용할 수 있다. 아날라이저는 토크나이저와 필터를 합쳐서 저장이 가능하며, 인덱스에 저장하여 사용할 수 있다. 기본적으로 엘라스틱서치에서 제공하는 에널라이저가 있다.

```
GET library/_analyze
{
  "analyzer": "standard",
  "text": "Brown fox brown dog"
}
```



복합적인 문장을 분석하는 경우

qhrgkqwjrdls answkddmf anstj

```
GET library/_analyze
{
  "tokenizer": "standard",
  "filter": ["lowercase"],
  "text": "THE quick.brown_FOx jumped! $19.95 @ 3.0"
}
```

```
GET library/_analyze
{
  "tokenizer": "letter",
  "filter": ["lowercase"],
  "text": "THE quick.brown_FOx jumped! $19.95 @ 3.0"
}
```

letter 토크나이저는 의미가 있는 단어들로만 분리를 한다. 알파벳만 허용한다.

## 어그리게이션 (Aggregation, 집계)

도크멘트의 키워드 필트 값을 기본적으로 사용한다. 쿼리랑 같은 레벨에서 사용 가능하며, 쿼리와 같이 사용할 수 있다.

```
GET library/_search
{
  "size": 0,
  "aggs": {
    "popular-colors": {
      "terms": {
        "field": "colors.keyword"
      }
    }
  }
}
```

1차적으로 쿼리를 먼저 실행하며, 그 결과 범위에서 집계를 시행한다.

```
GET library/_search
{
  "query": {
    "match": {
      "title": "dog"
    }
  },
  "aggs": {
    "popular-colors": {
      "terms": {
        "field": "colors.keyword"
      }
    }
  }
}
```

어그리케이션은 여러 개를 사용하며 sub-aggs를 사용한다.

```
GET library/_search
{
  "size": 0,
  "aggs": {
    "price-statistics": {
      "stats": {
        "field": "price"
      }
    },
    "popular-colors": {
      "terms": {
        "field": "colors.keyword"
      },
      "aggs": {
        "avg-price-per-color": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

## 도큐멘트 업데이트

기존 데이터를 대체할 수 있다.

```
POST library/books/1
{
  "title": "The quick brow fox",
  "price": 10,
  "colors": ["red", "green", "blue"]
}
```

_update api를 이용할 수 있다.

```
POST library/books/1/_update
{
  "doc": {
    "title": "The quick fantastic fox"
  }
}
```

업데이트시 버전(_version)의 값이 올라간다.



## 매핑

들어온 데이터를 확인하면서 값의 필드를 알아서 판단한다. 임의의 패핑을 지정하고자 하는경우 사용된다. 

세팅과 매핑으로 나누어 지며, 세팅에는 인덱스의 설정값이 담긴다.

이미 만들어 놓은 인덱스는 수정할 수 없으며, 인덱스 전부 삭제 후 다시 생성해야 한다.

```
PUT famous-librarians
{
  "settings": {
    "number_of_replicas": 0,
    "number_of_shards": 2,
    "analysis": {
      "analyzer": {
        "my-analyzer": {
          "type": "custom",
          "tokenizer": "uax_url_email",
          "filter": [
            "lowercase"
            ]
        }
      }
    }
  },
  "mappings": {
    "librarian": {
      "properties": {
        "name": {
          "type": "text"
        },
        "favourite-colors": {
          "type": "keyword"
        },
        "birth-date": {
          "type": "date",
          "format": "year_month_day"
        },
        "hometown": {
          "type": "geo_point"
        },
        "description": {
          "type": "text",
          "analyzer": "my-analyzer"
        }
      }
    }
  }
}
```



아래와 같이 값을 넣을 수 있다.

```
PUT famous-librarians/librarian/1
{
  "name": "Fernando Chiwoo",
  "favourite-colors": ["Yellow", "light-gret"],
  "birth-date": "1990-05-10",
  "hometown": {
    "lat": 33.333333,
    "lon": 55.555555
  },
  "description": "This example is example for Elasticsearch Tutorial"
}
```



쿼리를 통해 생성한 도큐메트를 검색할 수 잇다.

```
GET famous-librarians/_search
{
  "query": {
    "query_string": {
      "fields": ["favourite-colors"],
      "query": "yellow or black"
    }
  }
}
```



range를 통해 범위를 지정할 수 있으며, now 키워드를 지원한다.

```
GET famous-librarians/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_all": {}
        }
      ],
      "filter": {
        "range": {
          "birth-date": {
            "gte": "now-200y",
            "lte": "2000-01-01"
          }
        }
      }
    }
  }
}
```

geo-point로도 범위를 지정할 수 있다. 위치 정보와 관련된 검색들도 가능하다.

```
GET famous-librarians/_search
{
  "query": {
    "bool": {
      "filter": {
        "geo_distance": {
          "distance": "100km",
          "FIELD": {
            "lat": 40.73,
            "lon": -74.1
          }
        }
      }
    }
  }
}
```















https://www.elastic.co/guide/en/elastic-stack-overview/6.3/monitoring-production.html





[How monitoring works](https://www.elastic.co/guide/en/elastic-stack-overview/6.3/how-monitoring-works.html)

