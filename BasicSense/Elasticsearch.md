# Elasticsearch

엘라스틱서치는 Java 8부터 지원하며, Java 8 릴리스 시리즈에 Java 버전 1.8.0_131 이상 버전을 설치하는 것이 좋다.

[elasticsearch 다운로드 및 설치 참조](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html#install-targz)

-----

## Enable automatic creation of X-Pack indices

X-Pack은 엘라스틱서치 안에서 많은 색인들을 자동적으로 생성하기위해 시도한다. 기본적으로, 엘라스틱서치는 자동 인덱스 생성(automatic index creation)을 허락하도록 설정되어 있고, 추가적은 단계를 요구하지 않는다.

자동 인덱스 생성을 사용하지 않을 경우, ``elasticsearch.yml`` 파일의 ``action.auto_create_index`` 요소를 수정해주어야 한다.

로그스테이스 또는 비트스를 사용하는 경우 action.auto_create_index 설정에 추가 색인 이름이 필요하다. 정확한 값은 로컬 구성에 따라 다르다. 사용자의 환경에 정확한 값이 확실하지 않다면, * 키워드를 통해 모든 색인의 자동 생성을 허가해야 한다.

```
X-Pack은 엘라스틱서치, 키바나, 로그스테이시, 비트(Beats)에 단일 팩으로 설치됨으로써, 엘라스틱서치에 위치하는 데이터의 보안이나 키바나를 통한 로그인 화면 추가 등의 추가적인 기능을 제공한다. 
```

-----

## Running Elasticsearch from the command line

```
./bin/elasticsearch
```

위의 명령어를 통해 엘리스틱서치를 동작시킨다. 기본적으로 엘라스틱서치는 foreground에서 운영되고, 표준 출력으로 로그를 출력하며, ``Ctrl-C``으로 멈춘다.

```
jhlee-pc:~ kimdonghwi$ curl -X GET http://localhost:9200/
{
  "name" : "GK-YN8B",
  "cluster_name" : "elasticsearch_kimdonghwi",
  "cluster_uuid" : "4fIWDb_4RUe2Macmi0jsxg",
  "version" : {
    "number" : "6.2.4",
    "build_hash" : "ccec39f",
    "build_date" : "2018-04-12T20:37:28.497551Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

-----

## Configuring Elasticsearch

기본적인 설정이 거의 필요하지 않으며, 대부분의 설정은 Cluster Update Settings API를 사용하여 운영중인 클러스터에서 변경할 수 있다. 설정 파일(Configuration File)은 node-specific(``node.name``, ``paths``, etc..)과 클러스터와 결합을 위한 ``cluster.name``, ``network.host`` 등으로 구성되어 있다.

- 엘라스틱서치 설정을 위한 elasticsearch.yml
- 엘라스틱서치 JVM 설정을 위한 jvm.options
- 엘라스틱서치 로깅 구성을 위한 log4j2,properties

### Config File Format

``` 
path:
	data: /var/lib/elasticsearch
	logs: /var/log/elasticsearch
------------------------- The top and bottom are the same ----------------------------
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
```

### Environment Variable Substitution

```
name.name: ${HOSTNAME}
network.host: ${ES_NETWORK_HOST}
```

### Prompting for Settings

설정 파일에 저정하지 않으려는 설정의 경우 ``${prompt.text}`` 또는 ``${prompt.secret}`` 값을 사용하고, 포그라운드(fore ground)에서 엘라스틱서치를 실행할 수 있다. ``${prompt.secret}`` 터미널에 입력된 값이 표시되지 않도록 비활성화 되어있으며, ``{prompt.text}`` 는 입력할 때 값을 볼 수 있다.

```
node: 
	name: ${prompt.text}
```

실제 프롬프트로 입력할 경우 아래와 같다.

```
Enter value for [node.name]:
```

-----

### elasticsearch.yml

```
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
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
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when new node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
#discovery.zen.ping.unicast.hosts: ["host1", "host2"]
#
# Prevent the "split brain" by configuring the majority of nodes (total number of master-eligible nodes / 2 + 1):
#
#discovery.zen.minimum_master_nodes:
#
# For more information, consult the zen discovery module documentation.
#
# ---------------------------------- Gateway -----------------------------------
#
# Block initial recovery after a full cluster restart until N nodes are started:
#
#gateway.recover_after_nodes: 3
#
# For more information, consult the gateway module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Require explicit names when deleting indices:
#
#action.destructive_requires_name: true
```

-----









-----

### Important Elasticsearch Configuration

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

기본적으로, 엘라스틱서치는 노드 id로 UUID를 랜덤하게 생성한 7개의 문자를 처음으로 사용할 서이다.

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

#### Discovery Settings

엘라스틱서치는 노드 간 클러스터링 및 마스터 선출을 위해 "Zen Discovery"라는 사용자 정의 검색 구현을 사용한다.

프로덕션 환경으로 이동하기 전에 구성해야 하는 주용한 검색 설정이 두 가지 있다.









-----

### ref

- [Elasticsearch Reference 6.3](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)



