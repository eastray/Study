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

```

```

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



-----



-----



-----





-----

### ref

- [Elasticsearch Reference 6.3](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [java keytool 사용법 - Keystore 생성, 키쌍 생성, 인증서 등록 및 관리](https://www.lesstif.com/pages/viewpage.action?pageId=20775436)



