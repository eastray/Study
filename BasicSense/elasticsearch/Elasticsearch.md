# Elasticsearch

엘라스틱서치는 Java 8부터 지원하며, Java 8 릴리스 시리즈에 Java 버전 1.8.0_131 이상 버전을 설치하는 것이 좋다.

[elasticsearch 다운로드 및 설치 참조](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html#install-targz)

```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.1.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.1.tar.gz.sha512
shasum -a 512 -c elasticsearch-6.3.1.tar.gz.sha512 
tar -xzf elasticsearch-6.3.1.tar.gz
cd elasticsearch-6.3.1/ 
```

-----

## Concept

### 클러스터 (Cluster)

클러스터는 하나 이상의 노드(server)가 모인 것으로, 이를 통해 전체 데이터를 저장하고 모든 노드를 통합 색인화 및 검색 기능을 제공한다. 클러스터는 유니크한 이름으로 식별되며 기본으로 `elasticsearch`를 사용한다. 어떤 노드가 특정 클러스트에 포함되기 위해서 이름을 사용하여 설정되기 대문에 클러스터의 이름은 중요하다.

### 노드 (Node)

노드는 데이터를 저장하는 클러스터의 일부이며, 클러스터의 인덱싱 및 검색 기능에 참여하는 단일 서버이다. 클러스터와 같이 노드는 이름으로 식별되며, 기본적으로 노드를 실행하면 램덤으로 UUID(Universally Unique IDentifier)가 할당된다. 

노드는 클러스터의 이름을 통해 특정 클러스터의 일부로 구성이 될 수 있으며, 기본적으로 각 노드는 `elasticsearch` 라는 이름의 클러스터에 포함되도록 설정되어 있다. 네트워크에서 다수의 노드를 시작하고 서로 발견할 수 있다고 가정하면 자동으로 `elasticsearch` 라는 단일 클러스터를 형성하고 결합한다.

단일 클러스터에서는 원하는 만큼의 노드를 가질 수 있다, 또한 현재 네트워크에서 실행중인 다른 엘라스틱서치 노드가 없는 경우 단일 노드를 시작하면 기본적으로 `elasticsearch`라는 새로운 당일 노드 클러스터가 형성된다.

### 인덱스 (Index)

색인(인덱스)은 비슷한 특성을 가진 문서들의 모음이다. 고객 정보에 대한 색인, 상품 카탈로그의 색인, 주문 정보에 대한 색인을 예로 들 수 있다. 색인은 이름에 의해 식별되며, 이 이름은 색인된 문서를 색인, 검색, 업데이트 및 삭제할 때 색인을 나타내는데 사용된다. 

### 타입 (Type)

하나의 색인에서 하나 이상의 유형을 정의할 수 있습니다. 유형이란 색인을 논리적으로 분류/구분한 것이며 그 의미 체계는 전적으로 사용자가 결정합니다. 일반적으로 여러 공통된 필드를 갖는 문서에 대해 유형이 정의됩니다. 예를 들어 블로그 플랫폼을 운영하고 있는데 모든 데이터를 하나의 색인에 저장한다고 가정합니다. 이 색인에서 사용자 데이터, 블로그 데이터, 댓글 데이터에 대한 유형을 각각 정의할 수 있습니다.

 

### 도큐먼트

문서는 색인화할 수 있는 기본 정보 단위입니다. 예를 들어 어떤 단일 고객, 단일 제품, 단일 주문에 대한 문서가 각각 존재할 수 있습니다. 이 문서는 [JSON](http://json.org/)(JavaScript Object Notation) 형식인데, 이는 널리 사용되는 인터넷 데이터 교환 형식입니다.

하나의 색인/유형에 원하는 개수의 문서를 저장할 수 있습니다. 문서가 물리적으로는 어떤 색인 내에 있더라도 문서는 색인화되어 색인에 포함된 어떤 유형으로 지정되어야 합니다.

 

### 샤드 & 리플리카

색인은 방대한 양의 데이터를 저장할 수 있는데, 이 데이터가 단일 노드의 하드웨어 한도를 초과할 수도 있습니다. 예를 들어 10억 개의 문서로 구성된 하나의 색인에 1TB의 디스크 공간이 필요할 경우, 단일 노드의 디스크에서 수용하지 못하거나 단일 노드에서 검색 요청 처리 시 속도가 너무 느려질 수 있습니다.

Elasticsearch는 이러한 문제를 해결하고자 색인을 이른바 샤드(shard)라는 조각으로 분할하는 기능을 제공합니다. 색인을 생성할 때 원하는 샤드 수를 간단히 정의할 수 있습니다. 각 샤드는 그 자체가 온전한 기능을 가진 독립적인 "색인"이며, 클러스터의 어떤 노드에서도 호스팅할 수 있습니다.

 

샤딩은 무엇보다도 2가지 이유로 중요합니다.

 

- 콘텐츠 볼륨의 수평 분할/확장이 가능해집니다.
- 작업을 (어쩌면 여러 노드에 위치한) 여러 샤드에 분산 배치하고 병렬화함으로써 성능/처리량을 늘릴 수 있습니다.

 

샤드가 분산 배치되는 방식 및 그 문서가 다시 검색 요청으로 집계되는 방식의 메커니즘은 모두 Elasticsearch에서 관리하며 사용자에게는 투명하게 이루어집니다.

언제든 오류가 일어날 가능성이 있는 네트워크/클라우드 환경에서는 어떤 이유에서든 샤드/노드가 오프라인 상태가 되거나 사라지게 될 경우에 대비하여 페일오버 메커니즘을 마련하는 것이 매우 유익하고 바람직합니다. 이러한 취지에서 Elasticsearch에서는 색인의 샤드에 대해 하나 이상의 복사본을 생성할 수 있는데, 이를 리플리카 샤드(replica shard), 줄여서 리플리카라고 합니다.

이처럼 리플리카를 만드는 복제는 무엇보다도 2가지 이유로 중요합니다.

 

- 샤드/노드 오류가 발생하더라도 고가용성을 제공합니다. 따라서 리플리카 샤드는 그 원본인 기본 샤드와 동일한 노드에 배정되지 않습니다.
- 모든 리플리카에서 병렬 방식으로 검색을 실행할 수 있으므로 검색 볼륨/처리량을 확장할 수 있습니다.

 

요약하자면 각 색인은 여러 개의 샤드로 분할할 수 있습니다. 또한 하나의 색인은 복제하지 않거나(리플리카 없음) 1회 이상 복제할 수 있습니다. 복제되면 각 색인은 기본 샤드(복제 원본 샤드)와 리플리카 샤드(기본 샤드의 복사본)를 갖습니다. 샤드 및 리플리카의 수는 색인별로, 색인 생성 시점에 정의할 수 있습니다. 색인이 생성된 다음 언제라도 탄력적으로 리플리카의 수를 변경할 수 있으나, 샤드 수는 사후 변경이 불가합니다.

기본적으로 Elasticsearch의 각 색인은 기본 샤드 5개, 리플리카 1개를 갖습니다. 따라서 클러스터에 최소한 2개의 노드가 있다면 색인은 기본 샤드 5개, 리플리카 샤드 5개(완전한 리플리카 1개)를 가지므로 색인당 총 10개의 샤드가 존재하게 됩니다.

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

### Setting JVM Options

Java Virtual Machine(JVM)의 옵션을 변경하는 것은 힙(heap) 사이즈 세팅을 변경하는 것과 같이 드문 일이다. JVM 옵션을 변겨해야 할 경우, ``jvm.options`` 구성 파일의 세팅하고, 이 파일의 기본 위치는 ``config/jvm.options``에 있다. 

``-``로 시작하는 줄은 JVM 버전과 독립적으로 적용되는 JVM 옵션으로 처리된다.

숫자로 시작하여 바로 뒤에 ``:`` 와 ``-`` 로 시작하는 행은 JVM 버전과 숫자가 같은 경우에만 적용되는 JVM 옵션으로 처리된다.

숫자로 시작하여 바로 뒤에 ``:``와 ``-``가 오는 행은 JVM 버전이 숫자보다 크거나 같은 경우에만 적용되는 JVM 옵션으로 처리된다.

숫자 사이에 ``-``가 오는 행은 JVM 버전이 두 숫자 사이의 범위에 해당하는 경우에만 적용되는 JVM 옵션으로 처리된다.

### Secure Setting

일부 설정은 예민하며 파일 시스템 권한을 사용하여 값을 보호하는 것으로 충분하지 않다. 이 경우 Elasticsearch는 키스토어(keystore)에서에서 설정을 관리하기 위해 elasticsearch-keystore와 keystore를 제공한다.

```
Keystore
키스토어(Keystore)는 지갑을 사용하기 위해 Private Key를 비밀번호로 암호화한 텍스트 또는 파일이다.
Java는 KeyStore라는 인터페이스를 통해 Encryption/Decryption, Digital Signature에 사용되는 Private Key, Public Key, Certificate를 추상화하여 제공한다.
키스토어를 구현한 Provider에 따라 실제 개인 키가 저장되는 곳이 로컬 디스크, HSM 같은 별로의 하드웨어, Windows의 CertStore, OSX의 KeyChain 등의 장소와는 상관없이 사용자는 별도의 소스 코드를 수정할 필요없이 키와 인증서를 가져올 수 있다. 이를 이용해 데이터의 암복호화, 전자서명들을 수행한다.
```

- Creating the keystore

  ``elasticsearch.keystore``를 생성하기 위해서는 ``create`` 명령어를 사용한다. ``elasticsearch.keystore``는 ``elasticsearch.yml``과 나란히 생성될 것이다.

  ```
  bin/elasticsearch-keystore create
  ```

- Listing Settings in the keystore

  keystore의 설정 목록은 list 명령어를 사용한다.

  ```
  bin/elasticsearch-keystore list
  ```

- Adding string setting

  클라우드 플러그인의 인증 자격 증명과 같은 중요한 문자열 설정은 add 명령을 사용해 추가할 수 있으며, 설정 값을 묻는 메시지를 표시할 수 있다. stdin을 통해 값을 전달할 경우 ``--stdin`` 플래그를 사용한다.

  ```
  bin/elasticsearch-keystore add the.setting.name.to.set

  cat /file/containing/setting/value | bin/elasticsearch-keystore add --stdin the.setting.name.to.set
  ```

- Removing settings

  키스토어의 설정을 제거하기 위해서는 ``remove`` 명령을 사용한다.

  ```
  bin/elasticsearch-keystore remove the.setting.name.to.remove
  ```

### Logging Configuration

엘라스틱서치는 로깅을 위해 Log4j2를 사용한다. Log4j2는 ``log4j2.properties`` 파일을 사용하여 구성할 수 있으며, 엘라스틱서치는 로그 파일의 위치를 결정하기 위해 구성 파일에서 세 개의 속성을 갖는다. 

- `${sys:es.logs.base_path}`: 로그 디렉토리로 해석
- `{sys:es.logs.cluster_name}`: 클러스터 이름으로 해석
- `{sys:es.logs.node_name}`: 노드 이름으로 해석

[속성별 해석](https://www.elastic.co/guide/en/elasticsearch/reference/current/logging.html#CO6-1)

#### Configuring logging levels

로깅 레벨을 구성하는 데는 네 가지 방법이 있다.

- Command-line

  싱글 노드에서 일시적으로 문제를 디버깅 할 때 적합하다.

  ```
  -E <name of logging hierarchy> = <level>
  // example
  -E looger.org.elasticsearch.transpot = trace
  ```

- elasticsearch.yml

  일시적인 문제를 디버깅할 때 사용되지만, command-line으로 엘라스틱서치를 시작하지 않거나 로깅 수준을 영구적으로 조정하여 사용할 때 적합하다.

  ```
  <name of logging hierarchy>: <level>
  // example
  logger.org.elasticsearch.transport: trace
  ```

- cluster settings

  실제 실행중인 클러스터에서 로깅 수준을 동적으로 저정해야 할 때 적합하다.

  ```
  PUT /_cluster/settings
  {
      "transient": {
          "<name of logging hierarchy>": "<level>"
      }
  }

  // example
  PUT /_cluster/settings
  {
      "transient": {
          "logger.org.elasticsearch.transport": 
  "trace"
      }
  }
  ```

- log4j2.properties

  로거에 대한 세분화된 제어가 필요한 경우 적합하다.

  ```
  logger.<unique_identifier>.name = <name of logging hierarchy>
  logger.<unique_identifier>.level = <level>

  // example
  logger.transport.name = org.elasticsearch.transport
  logger.transport.level = trace
  ```

#### Deprecation logging

더 이상 사용되지 않는 작업을 로깅할 수 있다. 예를 들어, 향후 특정 기능을 마이그레이션해야 하는 경우 이를 미리 결정할 수 있다. 기본적으로 Deprecation Logging은 모든 로그 메시지가 방출되는 WARN 레벨에서 사용된다.

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

## Important System Configuration

이상적으로, 엘라스틱서치는 서버에서 혼자 운영되어야 하며, 사용할 수 있는 모든 자원을 사용해야 한다. 그러기 위해서는 엘라스틱서치가 운영되면서 기본적으로 허용되는 것보다 더 많은 자원에 접근할 수 있도록 해당 운영체제의 설정이 필요하다. 



-----

6.3 버전과 6.2 버전의 차이

6.3 버전부터는 X-Pack이 포함되어 있지만 6.2 버전 이하부터는 포함되어 잇지 않아 별도의 설치가 필요하다.

```
jhlee-pc:elasticsearch-6.3.1 kimdonghwi$ ./bin/elasticsearch-plugin install x-pack

ERROR: this distribution of Elasticsearch contains X-Pack by default


jhlee-pc:elasticsearch-6.2.4 kimdonghwi$ ./bin/elasticsearch-plugin install x-pack
-> Downloading x-pack from elastic
[====>                                            ] 10%
```



-----





-----

### ref

- [Elasticsearch Reference 6.3](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [java keytool 사용법 - Keystore 생성, 키쌍 생성, 인증서 등록 및 관리](https://www.lesstif.com/pages/viewpage.action?pageId=20775436)
- [Basic Concept](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html)



