# Hystrix

Netfix에서 Circuit Breaker Pattern을 구현한 라이브러리이다. 

-----

## Circuit Breaker (by. Martin Fopwler)

일반적으로 스프트웨어 시스템에서는 각기 다른 프로세스들에서 동작하는 소프트웨어를 원격 호출하도록 한다. 메모리 상에서의 호출과 원격 호출의 차이점 중 하나는, 원격 호출의 경우 fail이 일어날 수 있거나 어떤 타임아웃 제한에 다다를때 까지 응답을 주지않고 [Hang](#hang)이 걸릴 수 있다는 것이다. 이 문제는 응답이 없는 supplier에 많은 호출자가 묶인다면, 중요 자원의 고갈로 인해 여러 시스템에 거쳐 연쇄 Failure를 일으킬 수 있다. 이러한 치명적인 연쇄 Failure를 방지하기 위해 Circuit Breaker Pattern이 대중화 되었다.

Circuit Breaker의 기본적인 개념은 failure를 모니터링하는 Circuit Breaker Object에 protected function call을 래핑함으로써, failure가 특정 임계치에 다다르면, Circuit Breaker가 trip되고 그 후 추가 발생하는 Cirvuit Breaker를 호출하는 모든 호출들은 protected call이 전쳐 이루어 지지 않고 에러를 리턴한다.

![CircuitBreakerPattern](./Image/CircuitBreakerPattern.png)

-----

## Circuit Breaker Pattern

서비스 호출 중간, 즉 Service A와 Service B 사이에 Circuit Breaker를 설치하여 Service B로의 모든 호출이 Circuit Breaker를 통하게 되고, Service B가 정상적인 상황에서는 트래픽을 문제없이 통과시킨다.

![CircuitBreakerPattern(ByPass)](/Users/kimdonghwi/Documents/Personal/Study/BasicSense/Image/CircuitBreakerPattern(ByPass).png)

Service B와의 통신에서 문제가 발생하게 되는 경우, Circuit Breaker가 이를 감지하여 Service B로의 호출을 강제적으로 끊어 Service A의 스레드가 더 이상 응답을 기다리지 않도록하여 장애가 전파되는 것을 방지한다. 강제적으로 호출을 끊으면 에러 메시지가 Service A에서 발생하기 때문에 장애 전파는 막을 수 있지만, Service A에서 별도의 장애 처리 로직이 필요하다.

이를 발전 시킨 형태가 Fall-back Messaging이며, Circuit Breaker에서 Service B가 정상적인 응답을 할 수 없을 때, Circuit Breaker가 룰에 따라 다은 메시지를 리턴하는 방식이다. 

![CircuitBreakerPattern(Fallback)](/Users/kimdonghwi/Documents/Personal/Study/BasicSense/Image/CircuitBreakerPattern(Fallback).png)

```
example.
Service A가 상품 목록을 화면에 뿌려주는 서비스이고, Service B가 사용자에 대해 머신러닝을 이용하여 상품을 추천해주는 서비스라고 가정할 때, Service B에서 장애가 발생하면 상품 추천을 할 수 없다. 
이때 미리 추천 상품 목록을 설정해놓고 Service B에서 장애가 난 경우 Circuit Breaker에서 미리 설정해 놓은 추천 상품 목록을 제공해줌으로써, 머신러닝 알고리즘 기반의 상품 추천보다는 정확도가 떨어지지만 최소한 시스템이 장애가 나는 것을 방지할 수 있다.
```

이 패턴은 넷플릭스에서 자바 라이브러리인 Hystrix로 구현이 되었으며, Spring 프레임워크를 통해 손쉽게 적용할 수 있다.

-----

## Hang

컴퓨팅에서 컴퓨터 프로그램이나 시스템이 입력에 응답하지 않을 때 정지(hang || freeze)가 발생한다. 

hang에는 무한 루프, 장시간 중단할 수 없는 컴퓨팅, 자원 고갈 (thrashing), 성능이 좋지 않은 하드웨어(throttling), 느린 컴퓨터 네트워크와 같은 외부 이벤트, 잘못된 설정, 호환성 문제 등과 같이 소프트웨어 또는 하드웨어 문제를 비롯하여 다양한 원인과 증상이 있다.



----

#### Ref

- [마틴 파울러가 작성한 CircuitBreaker 내용 해석](http://egloos.zum.com/pulgrims/v/3047353)
- [Circuit breaker 패턴을 이용한 장애에 강한 MSA 서비스 구현하기 #1 - Circuit breaker와 넷플릭스 Hystrix [조대협의 블로그]](http://bcho.tistory.com/tag/하이스트릭스)
- [Netflix-How it Works](https://github.com/Netflix/Hystrix/wiki/How-it-Works)
- [Circuit Breaker](https://spring.io/guides/gs/circuit-breaker/)
- [javadoc-netflix-hystrix](http://netflix.github.io/Hystrix/javadoc/)

