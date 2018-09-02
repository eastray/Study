# Tutorial 2



beats는 가볍고 제한된 기능을 제공하며, Logstash는 다양한 기능을 지원한다. 

## Dataflow 엔진

데이터 흐름을 위한 중앙 처리 엔진

파이프라인을 구축하여 이벤트 제이터 변환 및 스트림 설정

## Data Source Discovery



로그나 파일의 경우 비츠를 사용하여 수집하는 것이 편리하다.



AWS, Web Apps, MQs(Message Queue), DB, IoT 등의 데이터를 로그스테이시로 모을 수 있다. 



## 파이프라인 강화

- 데이터스트림 실시간 변경, 제거 및 정규화
- 조건을 이용한 데이터 흐름 분할 및 라우팅
- 파이프라인 성능 및 활성 상태 모니터링
- 사용자 인증 및 SSL/TLS를 이용한 파이프라인 보안 적용

## Codec (인코딩, 디코딩)

- JSOn
- Avro
- msgpack
- Netflow
- CloudTrail



## logstash.yml

### pipeline.workers: 

더 많은 CPU 코어를 로그스테이시에게 할당할 수 있다.

### pipeline.output.workers

더 많은 CPU 코어를 로그스테이시 output에 할당할 수 있다.

### pipeline.batch.delay: 5

5/1000초마다 내가 만든 배치를 목적지로 던지겠다는 뜻





















