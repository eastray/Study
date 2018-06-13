# OAuth (OAuth 2.0 Authorization Framework)

- 개요
- 개선 사항
- Ref

-----

## 개요

OAuth 2.0은 인증(Authentication)과 허가(Authorization)를 제공하는 서비스와 상호 연동하기 위한 프레임워크이다. 수많은 모바일 및 웹 어플리케이션에 폭 넓게 도입되었으며, OAuth 2.0에서 토큰의 형태를 규정하고 있지 않다.

OAuth의 구조는 액세스 토큰(Access Token)과 리프레쉬 토큰(Refresh Token)으로 두 가지 타입이 있다. 최초의 인증에서 사용자의 어플리케이션은 이 두 가지 토큰을 발급 받는다. 액세스 토큰은 비교작 짧은 시간으로, 무효화(expire)되도록 설정되어 있다. 최초의 액세스 토큰이 무효화되면 리프레쉬 토큰을 사용해서 새로운 토큰을 획득할 수 있다. 리프레쉬 토큰에도 유효 기간(Expiration)을 설정할 수 있다 액셋스 토큰과 리프레쉬 토큰 둘 다 내장된 보안을 가지고 있어 변조를 방지할 수 있다.

OAuth 2.0 인증 프레임워크르 사용하면 타사 응용 프로그램이 자원 소유자와 HTTP 서비스 간의 승인 상호 작용을 조정하여 자원 소유자를 대신하여 HTTP 서비스에 대한 제한된 액세스 권한을 얻거나 타사 응용 프로그램이 자체적으로 액세스 권한을 얻는다.

기존 클라이언트 - 서버 인증 모델에서 클라이언트는 자원 소유자의 신임 정보를 사용하여 서버로 인증하으로써 서버에서 액세스 제한 자원(보호 자원)을 요청한다. 제한된 자원에 대한 타사 응용 프로그램 액세스를 제공하기 위해 리소스 소유자는 해당 자격 증명을 타사와 공유한다.

![OAuthProtocolFlow](./Image/OAuthProtocolFlow.png)

-----

## OAuth

OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹 사이트 상의 자신들의 정보에 대해 웹 사이트나 어플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로 사용되는 접근 위음을 위한 개방형 표준이다.

아마본, 구글, 페이스북, 마이크로소프트, 트위터 등에서 사용되며, 사용자들이 타사 어플리케이션이나 웹 사이트의 계정에 관한 정보를 공유할 수 있게 허용한다.

기본 인증인 아이디와 비밀번호는 보안상 취한한 구조이다. 기본 인증이 아닐 경우 각 어플리케이션등이 각자의 개발한 회사의 방법대로 사용자를 확인했다. OAuth는 이렇게 제각각인 인증 방식을 표준화한 인증 방식으로, OAuth를 이용하면 이 인증을 공유하는 어플리케이션끼리 별도의 인증이 필요없다. 여러 어플리케이션을 통합하여 사용하는 것이 가능하다.

- 사용자(user): 서비스 공급자와 소비자를 사용하는 계정을 가지고 있는 개인
- 소비자(consumer): Open API를 이용하여 개발된 OAturh를 사용하여 서비스 제공자에게 접근하는 웹 사이트 또는 어플리케이션
- 서비스 제공자(service provider): OAuth를 통해 접근을 지원하는 웹 어플리케이션(Open API를 제공하는 서비스)
- 소비자 비밀번호(consumer secret): 서비스 제공자에서 소비자가 자신임을 인증하기 위한 키
- 요청 토큰(request token): 소비자가 사용자에게 접근 권한을 인증받기 위해 필요한 정보가 담겨있으며 후에 접근 토큰으로 변환된다.
- 접근 토큰(access token): 인증 후에 사요자가 서비스 제공자가 아닌 소비자를 통해서 보호된 자원에 접근하기 위한 키를 포함한 값.

-----

## OAuth 2.0 Authorization Grant Type

권한 부여는 액세스 토큰을 얻기 위해 클라이언트가 사용하는 (보호 자원에 액세스 하기 위한) 자원 소유자의 권한을 나타내는 권한 정보이다. Authorization Code, Implicit, Resource Owner Password Credentials, Client Credentials와 같은 네가지 부여 유형과 추가 유형 정의를 위한 확장성 메커니즘이 있다.

### Authorization Code

권한 부여 코드(Authorization Code)는 클라이언트와 자원 소유자 사이에서 중개인으로 authorization server를 사용함으로써 얻을 수 있다. 클라이언트는 자원 소유자를 권한 서버로 향하게 하고 권한 서버는 자원 소유자를 권한 코드로 다시 클라이언트에게 보낸다.

### Implicit

Implicit Grant는 JavaScript와 같은 스크립팅 언어를 사용하여 브라우저에 구현된 클라이언트에 대해 최적화 및 단순회된 인증 코드 흐름이다. 클라이언트에게 권한 코드를 발행하는 대신 클라이언트에 직접 액세스 토큰이 발행된다.

### Resource Owner Password Credentials

자원 소유자 비밀번호 자격 증명은 액세스 권한을 얻기 위해 권한 부여로 직접 사용될 수 있다. 자격 증명은 리소스 소유자와 클라이언트가 다른 권한 부여 유형을 사용할 수 없고, 높은 신뢰도를 요구할 때 사용된다.

### Client Credentials

권한 부여 범위가 이전에 권한 서버와 함께 배치된 보호 자원 또는 클라이언트이 제어하에 있는 보호된 자원으로 제한되어 있는 경우 클라이언트 자격 증명을 권한 부여로 사용할 수 있다.

-----

## Access Token, Refresh Token

```
Access tokens are credentials used to access protected resources.  An access token is a string representing an authorization issued to the client.  The string is usually opaque to the client.  Tokens represent specific scopes and durations of access, granted by the resource owner, and enforced by the resource server and authorization server.

   The token may denote an identifier used to retrieve the authorization information or may self-contain the authorization information in a verifiable manner (i.e., a token string consisting of some data and a signature).  Additional authentication credentials, which are beyond the scope of this specification, may be required in order for the client to use a token.

   The access token provides an abstraction layer, replacing different authorization constructs (e.g., username and password) with a single token understood by the resource server.  This abstraction enables issuing access tokens more restrictive than the authorization grant used to obtain them, as well as removing the resource server's need to understand a wide range of authentication methods.

   Access tokens can have different formats, structures, and methods of utilization (e.g., cryptographic properties) based on the resource server security requirements.  Access token attributes and the methods used to access protected resources are beyond the scope of this specification and are defined by companion specifications such

```

```
Refresh tokens are credentials used to obtain access tokens.  Refresh tokens are issued to the client by the authorization server and are used to obtain a new access token when the current access token becomes invalid or expires, or to obtain additional access tokens with identical or narrower scope (access tokens may have a shorter lifetime and fewer permissions than authorized by the resource owner).  Issuing a refresh token is optional at the discretion of the authorization server.  If the authorization server issues a refresh token, it is included when issuing an access token (i.e., step (D) in Figure 1).

   A refresh token is a string representing the authorization granted to the client by the resource owner.  The string is usually opaque to the client.  The token denotes an identifier used to retrieve the authorization information.  Unlike access tokens, refresh tokens are intended for use only with authorization servers and are never sent to resource servers.
```

![RefreshingAnExpiredAccessToken](./Image/RefreshingAnExpiredAccessToken.png)



-----

### ref

- [JWT 자바 가이드](https://medium.com/@OutOfBedlam/jwt-%EC%9E%90%EB%B0%94-%EA%B0%80%EC%9D%B4%EB%93%9C-53ccd7b2ba10)
- [OAuth2에 대해 알아보자](https://swalloow.github.io/about-oauth2)
- [OAuth](https://ko.wikipedia.org/wiki/OAuth)
- [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)