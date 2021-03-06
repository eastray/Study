# Elastic Stack Tutorial

엘라스틱서치에서는 키바나를 사용하여 데이터를 검색하고 시각화 할 수 있다. 기본 설정에 이어 추가 구문 분석을 위해서는 로그스테이시를 설치해야 한다.

## 엘라스틱서치 (Elasticsearch)

#### 엘라스틱서치 설치 및 실행

```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.2.tar.gz
tar -xzvf elasticsearch-6.3.2.tar.gz
cd elasticsearch-6.3.2
./bin/elasticsearch
```

#### 엘라스틱서치 실행 확인

엘라스틱서치 데몬의 실행을 테스트하기 위해서, HTTP의 GET 요청을 보내어 확인할 수 있다.

```
curl http://127.0.0.1:9200
```

```
jhlee-pc:~ kimdonghwi$ curl http://127.0.0.1:9200
{
  "name" : "wgsC6Tz",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "yCRScHCXRPatSYyiAQfBcg",
  "version" : {
    "number" : "6.3.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "053779d",
    "build_date" : "2018-07-20T05:20:23.451332Z",
    "build_snapshot" : false,
    "lucene_version" : "7.3.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

-----

## Important Elasticsearch Configuration

엘라스틱서치에는 구성이 겅의 필요하지 않으며, 제작에 들어가지 전에 고려해야할 설정이 있다.

#### path.data and path.logs

만일 `.zip` 또는 `tar.gz` 아카이브를 사용한다면, `data` 와 `logs`디렉토리는 `$ES_HOME` 의 서브 디렉토리여야 한다.

```v
config/elasticsearch.yml 중
-----------------------------------------------------
path:
	logs: /var/log/elasticsearch
	data: /var/data/elasticsearch
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
#path.data: /path/to/data
#
# Path to log files:
#
#path.logs: /path/to/logs
```

#### cluster.name

노드는 `cluster.name` 을 클러스터의 다른 모든 노드와 공유할 때만 클러스터에 참가할 수 있다.

```
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
#cluster.name: my-application
```

#### node.name

기본적으로, 엘라스틱서치는 노드 id로 UUID를 랜덤하게 생성한 7개의 문자를 처음으로 사용할 수 있다.

```
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
#node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
```

아래와 같이 서버의 HOSTNAME을 `node.name` 으로 설정할 수 있다.

```
node.name: ${HOSTNAME}
```

#### network.host

기본적으로, 엘라스틱서치는 127.0.0.1과 같은 루프백 주소로 바인딩되며(IPv4 or IPv6), 이것은 서버에서 싱글 개발 노드로 운영되기 충분하다.

다른 서버에 노드가 있는 클러스터를 형성하고 통신하기 위해서는 노드가 비 루프백 주소에 바인딩되어야 한다.

```
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
#network.host: 192.168.0.1
#
# Set a custom port for HTTP:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
```



### Bootstrap Checks

중요한 설정을 구성하지 않으면 예기치 않은 문제를 겪는다. 엘라스틱서치의 이전 버전은 설정의 일부가 잘못 구성이 된 경우, 경고로 기록이 되었으며 사용자는 이러한 로그를 놓칠 수 밖에 없었다. 이를 예방하기 위해 엘라스틱서치는 시작할 때 부트스트랩 검사를 받는다. 부트스트랩 검사는 다양한 엘라스틱서치 및 시스템 설정을 검사한다. 엘라스틱서치가 개발 모드에 있으면 엘라스틱서치 로그에 경고로 표시되는 모든 부트스트랩 검사가 표현되며, 프로덕션 모드의 경우 부트스트랩 검사가 실패하면 엘라스틱서치가 시작되지 않는다.

-----

## 키바나 (Kibana)

#### 키바나 설치 및 실행

키바나는 엘라스틱서치와 함께 동작하도록 설계된 오픈소스로, 분석 및 시각화 플랫폼이다. 엘라스틱서치 색인에 저장된 데이터를 검색, 보기 및 상호 작용을 하는데 사용되며, 데이터 분석을 쉽게 하고, 다양한 차트, 테이블, 맵 등을 제공한다.

키바나는 엘라스틱서치와 동일한 서버에 설치하는 것이 좋지만, 필수는 아니다. 다른 서버에서 엘라스틱서치와 키바나를 각자 동작시키는 경우, 키바나의 설정 파일인 `kibana.yml` (config/kibana.yml)에서 엘라스틱서치가 있는 서버의 IP와 port 등의 URL을 설정해야 한다.

```
curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-6.3.2-darwin-x86_64.tar.gz
tar xzvf kibana-6.3.2-darwin-x86_64.tar.gz
cd kibana-6.3.2-darwin-x86_64/
./bin/kibana
```

키바나의 웹 인터페이스를 시작하려면 브라우저에서 `localhost:5601(http://127.0.0.1:5601)`로 접근한다.

-----

## 비트 (Beats)

![BeatFamily](/Users/kimdonghwi/Documents/Personal/Study/BasicSense/Image/BeatFamily.png)

엘라스틱서치에서는 다양한 유형의 데이터에 대한 다양한 유형의 수집기를 제공한다. 서버에 상주하는 프로그램으로 엘라스틱서치로 데이터를 수집하며, 추가적인 처리의 경우 변환 및 구문 분석을 위해 beats는 로그스테이시로 데이터를 수집 할 수 있다.

#### 메트릭비트 설치 (Metricbeat)

```
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.3.2-darwin-x86_64.tar.gz
tar xzvf metricbeat-6.3.2-darwin-x86_64.tar.gz
```

메트릭비트는 시스템 단위별 CPU 사용량, 메모리, 파일 시스템, 디스크 IO 및 네트워크 IO 통계뿐만 아니라, 시스템에서 실행되는 모든 프로세스에 대한 상위 항목과 같은 통계를 제공한다.

메트릭비트를 통해 CPU 사용량, 메모리, 파일 시스템, 디스크 IO 및 네트워크 IO 통계와 시스템에서 실행중인 모든 프로세스의 최상 통계 등을 수집할 수 있다. 

아래와 같이 시스템 모듈을 사용할 수 있도록 설정한다.

```
./metricbeat modules enable system
```

```
jhlee-pc:metricbeat-6.3.2-darwin-x86_64 kimdonghwi$ ./metricbeat modules enable system
Module system is already enabled
```

아래와 같이 `-e` 플래그를 사용하여 초기 환경을 설정한다. `setup` 명령은 키바나의 대시 보드를 로드하며, 대시 보드가 이미 설정된 경우 이 명령어를 생략할 수 있다. `-e` 플래그는 선택적으로 사용할 수 있으며, syslog 대신 표준 오류로 출력을 내보낸다.

```
./metricbeat setup -e
```

메트릭비트 시작은 아래와 같다.

```
./metricbeat -e
```

-----

## 키바나의 시스템 메트릭 시각화

시스템 메트릭을 시각화하기 위해서는, 키바나에서 `Dashboard` 탭을 클릭하고 리스트 중 `[Metricbeat System] Overview` 를 클릭한다.

![키바나_메트릭비트1](../Image/키바나_메트릭비트1.png)

`Host Overview`를 클릭함으로서 선택된 호스트의 메트릭을 자세히 볼 수 있다.

![키바나_메트릭비트2](../Image/키바나_메트릭비트2.png)

-----

## 로그스테이시 (Logstash)

#### 로그스테이시 설치

```
curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-6.3.2.tar.gz
tar -xzvf logstash-6.3.2.tar.gz
```

로그스테이시는 다양한 입력에서 읽을 수 있는 입력 플러그인을 제공한다. 비트의 입력을 수신하여 이벤트를 엘라스틱서치에게 출력으로 보내는 로그스테이시의 파이프라인 구성은 아래와 같다.

로그스테이시 config 디렉토리에 `.conf` 라는 새로운 로그스테이시 파이프라인 구성 파일을 만든 후, 아래의 항목을 추가한다.

```
input {
  beats {
    port => 5044
  }
}

# The filter part of this file is commented out to indicate that it
# is optional.
# filter {
#
# }

output {
  elasticsearch {
    hosts => "localhost:9200"
    manage_template => false
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
  }
}
```

위의 파이프라인 구성을 통해 로그스테이시가 시작하면 비트의 이벤트가 로그스테이시를 통해 라우팅된다. 로그스테이시에서는 데이터 수집, 강화 및 변형을 위한 로그스테이시 지능에 대한 모든 액세스 권한을 갖는다.

#### 로그스테이시 실행

```
./bin/logstash -f metrics-pipeline.conf
```

로그스테이시는 비티의 입력에서 이벤트를 수신하기 시작하며, 메트릭비트를 로그스테이시에 이벤트를 전송하도록 설정해야 한다.

#### 로그스테이시에 이벤트를 전송하기 위한 메트릭비트 설정

메트릭비트는 기본적으로 엘라스틱서치에게 이벤트를 보내도록 설정되어 DLT으며, 이를 로그스테이시로 전송하기 위해서는 메트릭비트의 설정 파일 (metricbeat.yml)을 수정해야 한다. 

```
#-------------------------- Elasticsearch output ------------------------------
output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]

  # Optional protocol and basic auth credentials.
  #protocol: "https"
  #username: "elastic"
  #password: "changeme"

#----------------------------- Logstash output --------------------------------
#output.logstash:
  # The Logstash hosts
  hosts: ["localhost:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  #ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]

  # Certificate for SSL client authentication
  #ssl.certificate: "/etc/pki/client/cert.pem"

  # Client Certificate Key
  #ssl.key: "/etc/pki/client/cert.key"
```

파일을 저장한 후 메트릭비트를 다시 시작하여 변경 사항을 적용시킨다. 로그스테이시는 비트으 입력을 읽고 엘라스틱서치로 이벤트를 인덱싱한다. 

#### 필드를 통해 데이터를 추출하기 위한 필터 정의

엘라스틱서치는 Grok 필터 플러그인을 지원하며, 이를통해 전체 커멘드라인 인수를 보내는 대신 명령의 경로만 보낼 수 있다. 

경로 추출을 위해 로그스테이시 설정 파일(logstash.yml)의 입력과 출력 사이에 아래와 같이 Grok 필터를 추가한다.

```
filter {
  if [system][process] {
    if [system][process][cmdline] {
      grok {
        match => { 
          "[system][process][cmdline]" => "^%{PATH:[system][process][cmdline_path]}"
        }
        remove_field => "[system][process][cmdline]" 
      }
    }
  }
}
```

`comline_path` 라는 필드를 통해 경로를 저장한다.

`cmdline` 을 제거함으로써 엘라스틱서치에서 인덱싱되지 않는다.

-----

## Setting UP X-Pack

```
X-Pack은 엘라스틱서치, 키바나, 로그스테이시, 비트(Beats)에 단일 팩으로 설치됨으로써, 엘라스틱서치에 위치하는 데이터의 보안이나 키바나를 통한 로그인 화면 추가 등의 추가적인 기능을 제공한다.
```

### Installing X-Pack

기본적으로 엘라스틱서치, 키바나, 로그스테이시를 설치할 때, X-Pack도 같이 설치된다.



### Enabling and Disabling X-Pack Features 

기본적으로 모든 기본 X-Pack 기능이 사용되며, elasticsearch.yml, kibana.yml, logstash.yml 설정 파일에서 특정 X-Pack 기능을 사용하거나 사용하지 않도록 설정할 수 있다.

- xpack.graph.enabled
- xpack.ml.enabled
- xpack.monitoring.enabled
- xpack.reporting.enabled
- xpack.security.enabled
- xpack.watcher.enabled

위의 기능들을 false로 설정하면 각각의 기능을 비활성화 할 수 있다. 각각의 설정 파일에서 어떤 설정이 있는지는 아래의 링크를 참조한다.

[X-Pack Settings](https://www.elastic.co/guide/en/elastic-stack-overview/6.3/xpack-settings.html)

### 6.3 버전과 6.2 버전의 차이 (X-Pack)

6.3 버전부터는 X-Pack이 포함되어 있지만 6.2 버전 이하부터는 포함되어 잇지 않아 별도의 설치가 필요하다.

```
// 6.3 Version
jhlee-pc:elasticsearch-6.3.1 kimdonghwi$ ./bin/elasticsearch-plugin install x-pack
ERROR: this distribution of Elasticsearch contains X-Pack by default

// 6.2.4 Version
jhlee-pc:elasticsearch-6.2.4 kimdonghwi$ ./bin/elasticsearch-plugin install x-pack
-> Downloading x-pack from elastic
[====>                                            ] 10%
```

-----

## Breaking Changes

특정 버전에서 다른 버전으로 어플리케이션을 마이그레이션할 때 알아야 할 변경 사항은 아래와 같다.

```
마이그레이션 (Migration)
서버가 특정 단말과 관련된 요청을 다른 서버로 위임하는 동작.
서버에서 특정 요청을 처리할 수 없는 상황이라 판단할 경우 다른 서버로 해당 요청을 위임.
커넥션 마이그레이션과 트랜잭션 마이그레이션이 있다.

데이터 마이그레이션 (Data Migration)
데이터베이스의 검색 성능이 향상되도록 데이터의 사용 빈도계에 따라 데이터의 저장 곤간이나 저장 형태를 조정하는 일
```

일반적으로 마이너 버전(5.x to 5.y) 사이의 호환성을 유지하도록 노력하며, 메이저 버전간(5.x to 6.y)의 변경 사항이 있을 수 있다. 

- - [참조]()

[Breaking Changes](https://www.elastic.co/guide/en/elastic-stack-overview/6.3/xpack-breaking-changes.html)

-----

## X-Pack APIs

X-Pack은 광범위한 REST API를 제공하여 기능을 관리하고 모니터링 한다.

그래프 탐색 API를 사용하면 엘라스틱서치 색인에서 문서 및 용어에 대한 정보를 추출하고 요약할 수 있다. 

이 API의 동작을 이해하는 가장 쉬운 방법은 그래프 UI를 사용하여 연결을 탐색하는 것이다.

```
Graph: Graph is unavailable for the current basic license. Please upgrade your license.
```

https://www.elastic.co/guide/en/elasticsearch/reference/6.3/graph-explore-api.html



-----

### ref

- [Elastic Stack Overview](https://www.elastic.co/guide/en/elastic-stack-overview/6.3/index.html)

