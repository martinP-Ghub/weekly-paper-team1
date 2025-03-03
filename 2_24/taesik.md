# 웹 API 발전 과정

# 학습 목표

웹 API의 발전 과정에서 SOAP에서 REST로의 전환이 일어난 이유와 그 장단점에 대해 설명하세요.

---

# 학습 내용

# SOAP API란?

> Simple Object Access Protocol (간단한 객체 접근 프로토콜) API
> 
> 서로 다른 애플리케이션 및 시스템 간에 정보 교환을 위해 사용

## 특징

### XML 기반

- HTTP, HTTPS, SMTP 등을 통해 XML 기반 메시지 교환
- 요청과 응답에서 데이터를 구조화하기 위해 XML에 의존
- 일관되고 잘 정의된 커뮤니케이션 형식을 보장

### 구조적이고 공식적

- 다른 API 스타일들과 비교해 SOAP API가 더 구조적이고 공식적
- WSDL(웹 서비스 설명 언어) 문서 파일을 사용
- 매개변수 및 예상되는 응답 구조를 명확하게 정의

### 신뢰성 및 보안

- 암호화 및 디지털 서명을 사용하여 데이터 전송의 안정성을 보장

---

# REST API란?


> Representational State Transfer (표현 상태 전달) API
>
> REST의 특징을 기반으로 서비스 API
>
> HTTP 프로토콜을 기반으로 리소스를 표현하고 조작하는 아키텍쳐 스타일

## REST (REpresentational State Transfer)란?

자원을 이름으로 구분해 해당 자원의 상태(정보)를 주고 받는 모든 것

자원(resource)의 표현(representation)에 의한 상태 전달을 의미

- 자원 : 해당 소프트웨어가 관리하는 모든 것(문서, 그림, 데이터, 해당 소프트웨어 자체 등등)
- 표현 : 자원을 표현하기 위한 이름(DB의 학생 정보가 자원이면 ‘students’를 자원의 표현으로 정함)
- 상태 전달 : 데이터가 요청되는 시점에 자원의 상태를 전달(JSON, XML 를 통해 데이터를 주고받음)

REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 사용하기 때문에 웹의 장점을 최대한 활용 가능한 아키텍쳐 스타일이다.

## REST 구성 요소 및 특징

### 자원(Resource) - URI

- 모든 것은 자원으로 표현됨
- 자원은 URI로 식별됨
    - URI(Uniform Resource Identifier) : 인터넷 상의 자원을 식별하기 위한 문자열의 구성*** URL을 포함**
    - URL(Uniform Resource Locator) : 인터넷 상 장원의 위치를 의미

### 행위(Verb) - Method

- HTTP 메서드를 사용하여 자원에 대한 작업을 수행
- CRUD(Create, Read, Update, Delete)

### 표현(Repersentation of Resource)

- 자원의 상태(데이터)는 특정 **형식(Represectation)**으로 표현됨
- 일반적으로 JSON 또는 XML을 사용하여 표현

### 상태(State)와 무상태성(Stateless)

- REST는 Stateless 원칙을 따름
- 클라이언트의 요청 간 서버 상태를 유지하지 않음 → 모든 요청은 독립적이어야 함
- 인증 정보는 요청마다 포함해야 함(ex. JWT, OAuth 토큰 등)

### 연결(Hypermedia)

- HATEOAS(Hypermedia As The Engine Of Application State) 원칙을 적용
    - 클라이언트가 API 응답을 통해 탐색가능한 링크를 제공받아서
    추가적인 요청을 동적으로 수행할 수 있도록 하는 개념
- 클라이언트가 응답 내의 링크를 따라가며 추가적인 작업을 수행할 수 있음

---

# SOAP vs REST

## 공통점

- 데이터 요청을 작성, 처리 및 응답하는 방식에 대한 규칙과 표준을 설명
- 표준화된 인터넷 프로토콜인 HTTP를 사용하여 정보를 교환
- 안전하고 암호화된 통신을 위해 SSL/TLS를 지원
- 안전하고 확장가능하며 내결함성이 있는 분산 시스템을 구축 가능

## 차이점

- SOAP는 프로토콜, REST는 아키텍쳐 스타일 ( 동작방식이 다름 )

### 설계 방식 차이

- SOAP : 함수 또는 작업을 노출
- REST : 데이터 기반

### 유연성

- SOAP : 엄격하고 어플리케이션 간 XML 메시징만 허용, 서버는 각 클라이언트의 상태를 유지
            새 요청을 처리할 때 이전 요청을 모두 기억해야 함
- REST : SOAP 보다 유연하며 어플리케이션에서 일반 텍스트, HTML, XML, JSON 등으로 데이터 전송
           무상태로서 모든 새 요청을 이전 요청과 독립적으로 처리

### 성능

- SOAP : 메시지가 크고 복잡하여 전송 및 처리 속도가 느림
- REST : 메시지가 작아 SOAP 보다 빠르고 효율적. 자주 액세스 하는 데이터는 캐싱처리로 시간 단축

### 확장성

- SOAP : 어플리케이션 요청 간에 상태를 저장해야하므로 대역폭과 요구사항이 증가
            비용이 많이 들고 확장이 어려워짐
- REST : 무상태 및 계층회된 아키텍처를 하므로 확장성이 뛰어남

### 보안

- SOAP : HTTPS와 함께 사용하려면 추가 WS-Security 계층이 필요
            이로 인한 통신 오버헤드가 추가되고 성능에 부정적 영향을 줌
    - WS-Security : 추가 헤더 컨텐츠를 사용해 지정된 서버의 지저오딘 프로세스만 SOAP 메시지 컨텐츠를 읽도록함
    - 
- REST : 추가 오버헤드 없이 HTTPS를 지원

### 신뢰성

- SOAP : 오류 처리 로직이 내장되어 있으며 높은 안정성을 제공
- REST : 통신 장애 발생 시 다시 시도해야 하며 안정성이 떨어짐

|  | SOAP | REST |
| --- | --- | --- |
| 프로토콜 | 어플리케이션 간 통신을 위한 프로토콜
XML 기반 메시지 교환 프로토콜 | 통신 인터페이스를 설계하기 위한 아키텍처 스타일
HTTP 프로토콜 기반 |
| 데이터 형식 | XML | JSON, XML, HTML 등 |
| 사용 방식 | 원격 프로시저 호출(RPC) 방식 | 리소스 중심 (URI + HTTP 메서드) |
| 성능 | 무거운 XML 메시지로 인해 상대적으로 느림 | 경량 JSON 데이터로 빠름 |
| 보안 | WS-Security, ACID 트랜잭션 지원 |  HTTPS, OAuth 등 활용(WS-Security 미지원) |
| 확장성 | 상대적으로 낮음 (상태 유지 필요) | Stateless 구조로 확장성 높음 |
| 웹 친화성 | 웹과 독립적 | 웹 환경에 최적화 |
| 사용 사례 | 금융, 결제 시스템 등 보안이 중요한 서비스 | 일반적인 웹 서비스, 모바일 백엔드 |

---

# SOAP에서 REST로 전환되는 이유

> 웹 API가 발전하면서 REST가 SOAP보다 더 널리 사용되게 된 이유는 단순성, 성능, 확장성 때문입니다

## 복잡도 감소

- SOAP : XML 기반의 메시지를 사용하고 WSDL과 같은 추가 정의가 필요
          → 구현이 복잡하고 유지 보수가 어려움
- REST : URI와 HTTP 메서드를 활용하여 데이터를 표현하므로 더 직관적이고 간결함

## 성능 향상

- SOAP : XML 메시지와 함께 헤더 정보가 많아 데이터가 무거움
- REST : JSON과 같은 경량 데이터 형식을 사용하여 성능이 뛰어남

## 웹 표준 활용

- SOAP : 자체적인 규칙과 프로토콜 추가로 구현해야 함
- REST : HTTP 프로토콜을 그대로 활용하므로, 웹 환경에서 더 효율적

## 확장성과 유연성

- SOAP : 상태 정보를 유지해야 하는 경우가 많아 확장성이 낮음
- REST : 클라이언트-서버 구조를 유지하며 확장성이 뛰어남

## 모바일 친화적

- SOAP : XML을 사용하여 데이터 크기가 커지고 파싱 속도가 느림
- REST : JSON을 기본적으로 사용해야 하므로 모바일 환경에서 빠르게 처리 가능


---

# HTTP 요청 및 응답

# 학습 목표

Spring Boot에서 @RestController로 들어온 HTTP 요청이 처리되어 응답으로 변환되는 전체 과정을 설명하세요. 특히 HTTP 메시지 컨버터가 동작하는 시점과 역할을 포함해서 설명하세요.

---

# 학습 내용

# @RestController HTTP 처리 과정

![image](https://github.com/user-attachments/assets/14ad7163-e461-4451-b3d3-03de6d4423a6)

## 1. HTTP 요청 수신 (클라이언트 → DispatcherServlet)

- 클라이언트가 HTTP 요청을 보냄 (ex. GET /users/1)
- 프론트 컨트롤러인 `DispatcherServlet` 이 먼저 받음

## 2. Handler Mapping 을 통해 컨트롤러 검색

- `DispatcherServlet`은  `HandlerMapping`을 통해 적절한 컨트롤러(**@RestController**)를 찾음
- 요청된 URL에 해당하는 컨트롤러 메서드에 매핑

## 3. Handler Adapter를 통해 컨트롤러 실행

- 선택된 컨트롤러를 실행할 수 있도록 적절한 어댑터를 결정
- REST 요청의 경우 → `RequestMappingHandlerAdapter`가 사용됨
    
    ### RequestMappingHandlerAdapter
    
    - `HandlerAdapter`는 `DispatcherServlet`이 컨트롤러를 실행할 수 있도록 도와주는 역할
    - `@RequestMapping`, `@GetMapping`, `@PostMapping` 등 사용하는 컨트롤러를 처리하는 handlerAdapter
- Spring에서는 다양한 방식의 컨트롤러를 지원
    - @Contorller, @RestController, @SimpleControllerHandlerAdapter(구형 Contorller)
    - @HttpRequestHandlerAdapter(HttpRequestHandler 인터페이스)
    - @SimpleServletHandlerAdapter (서블릿 직접 사용)

## 4. @RestController 실행

- 찾은 컨트롤러의 특정 메서드가 실행
- ex) `@GetMapping(”/users/{id}”)` 메서드가 실행 → service 레이어를 호출

```java
@RestController
@RequestMapping("/users")
public class UserController {
	private final UserService userService;
	
	@GetMapping("/{id}")
	public ResponseEntity<UserDto> gerUser(@PathVariable Long id) {
		UserDto user = userService.getUserById(id);
		return ResponseEntity.ok(user);
	}
}
```

## 5. Service & Repository 호출 ( 비즈니스 로직 실행 )

- `UserService` 가 `UserRepository`를 호출하여 DB에서 데이터를 가져옴
- DB에서 가져온 데이터를 DTO나 Entity로 반환하여 응답 반환

```java
@Service
public class UserService {
	private final UserRepository userRepository;

	public UserDto getUserById(Long id) {
		User user = userRepository.findById(id).orElseThrow();
		return new UserDto(user);
	}
}
```

## 6. 컨트롤러에서 응답 반환 (ResponseEntity)

- `ResponseEntity<UserDto>` 같은 객체가 컨트롤러에서 반환
- 현 위치에서는 HTTP 응답 형태(JSON/XML 등)로 반환되지 않은 상태임

## 7. HTTP 메시지 컨버터 동작

- `DispatcherServlet`은 컨트롤러에서 반환한 객체를 적절한 HTTP 응답으로 반환해야 함
- `HttpMessageConverter`가 동작하여 객체를 JSON/XML 등의 형식으로 반환
- Spring Boot 에서는 `MappingJackson2HttpMessageConverter`를 사용하여 JSON 변환을 수행함

## 8. 최종 HTTP 응답 반환

- 반환된 응답 데이터가 HTTP 응답 바디에 포함되어 클라이언트에게 전달
- 응답의 `Content-type` 은 `application/json` 으로 설정

---

# HTTP 메시지 컨버터

> Spring MVC 에서 요청(Request)과 응답(Response)를 처리하는 과정인 두 가지 시점에서 HttpMessageConverter가 동작


## 요청(Request) 처리 시점

- `클라이언트 → 서버` 로 요청이 들어올 때, 요청 본문(body)을 `@RequestBody`를 이용해 변환하는 과정에서 동작
- JSON, XML, Form 데이터 등의 요청 데이터를 Java 객체로 변환

### 동작 과정

1. 클라이언트가 JSON 데이터를 POST 요청으로 보냄
2. `DispatcherServlet`이 요청을 `HandlerAdapter`에 전달
3. `RequestMappingHandlerAdapter`가 컨트롤러 메서드를 실행하려고 함
4. 컨트롤러의 `@RequestBody` 어노테이션이 붙은 파라미터를 확인
5. `HttpMessageConverter`가 JSON 데이터를 Java 객체로 변환
6. 변환된 객체를 컨트롤러의 메서드 파라미터로 전달

### 동작 흐름

```java
[클라이언트] JSON 요청 → [DispatcherServlet] → [HandlerAdapter] → [RequestMappingHandlerAdapter] 
→ [HttpMessageConverter] (JSON → Java 객체) → 컨트롤러 메서드 실행

```

## 응답(Response) 처리 시점

- 컨트롤러 메서드에서 반환한 Java 객체를 HTTP 응답 본문(body)으로 변환할 때 동작
- Java 객체를 JSON, XML, TEXT 등 클라이언트가 요청한 형식으로 변환

### 동작 과정

1. 컨트롤러가 Java 객체(ResponseEntity<UserDto> 등)를 반환
2. `RequsetMappingHandlerAdapter`는 해당 객체를 확인
3. `HttpMessageConverter`가 Java 객체를 JSON 또는 XML로 변환
4. 변환된 데이터가 HTTP 응답 본문(body)에 포함되어 클라이언트로 반환

### 동작 흐름

```java
컨트롤러에서 Java 객체 반환 → [RequestMappingHandlerAdapter] → [HttpMessageConverter] (Java 객체 → JSON) 
→ HTTP 응답 본문에 포함 → 클라이언트로 응답 전송
```

---

# Spring Boot 기본 HTTP 메시지 컨버터

> Spring Boot는 여러 가지 **`HttpMessageConverter`**를 기본 제공
>
> 요청과 응답의 **`Content-Type`**을 기반으로 적절한 컨버터를 자동 선택


| **HTTP 메시지 컨버터** | **요청 처리 (Request)** | **응답 처리 (Response)** | **사용되는 MIME 타입** |
| --- | --- | --- | --- |
| **`MappingJackson2HttpMessageConverter`** | JSON → Java 객체 변환 | Java 객체 → JSON 변환 | **`application/json`** |
| **`StringHttpMessageConverter`** | String → Java 객체 변환 | Java 객체 → String 변환 | **`text/plain`** |
| **`FormHttpMessageConverter`** | **`application/x-www-form-urlencoded`** → Java 객체 변환 | Java 객체 → Form 데이터 변환 | **`application/x-www-form-urlencoded`** |
| **`Jaxb2RootElementHttpMessageConverter`** | XML → Java 객체 변환 | Java 객체 → XML 변환 | **`application/xml`** |

---

## 추후 공부하면 좋을 듯

https://velog.io/@woo00oo/HTTP-%EB%A9%94%EC%8B%9C%EC%A7%80-%EC%BB%A8%EB%B2%84%ED%84%B0
