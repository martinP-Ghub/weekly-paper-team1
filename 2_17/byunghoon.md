# 1. Spring에서 AOP(Aspect Oriented Programming)가 필요한 이유와 실제 애플리케이션 개발 사례

## AOP 란?

- **관점 지향 프로그래밍**으로, 애플리케이션의 **공통된  기능**을 모듈화하여 코드 중복을 줄이고, 핵심 비즈니스 로직에 집중할 수 있게 도와주는 프로그래밍 패러다임을 말한다.

## AOP의 필요성

- **중복 코드 제거**: 로깅, 인증, 트랜잭션 관리 등 여러 클래스에서 공통적으로 사용되는 코드를 분리하여 재사용할 수 있다.
- **핵심 비즈니스 로직 집중**: 부가 기능을 분리함으로써 핵심 비즈니스 로직의 가독성과 유지보수성을 향상시킬 수 있다.
- **모듈화(Modularization)**: 횡단 관심사(Cross-Cutting Concerns)를 독립적인 모듈로 분리하여 효율적으로 관리할 수 있다.
    
    횡단 관심사? : 애플리케이션의 여러 부분에 **공통적으로 적용되는 부가 기능**을 말한다.
    

## 실제 활용 사례(횡단 관심사의 예)

- **로깅 (Logging)**: 메서드 호출 전후에 자동으로 로그를 기록한다.
- **트랜잭션 관리**: 메서드 실행 시 트랜잭션을 자동으로 시작하고, 종료 시점에 커밋 또는 롤백을 수행한다.
- **보안 (Security)**: 메서드 접근 시 사용자의 인증 상태와 권한을 검사한다.
- **성능 모니터링**: 메서드의 실행 시간을 측정하여 성능을 분석한다.

---

# 2. Spring MVC에서 클라이언트의 요청 처리 흐름

## @Controller의 요청 처리 흐름

<img width="713" alt="스크린샷 2025-02-24 오전 10 44 41" src="https://github.com/user-attachments/assets/ab0fd31c-d157-497f-8d78-6eb36f5431cc" />


1. **클라이언트 요청 수신**: 클라이언트가 웹 브라우저 등을 통해 서버로 요청을 전송합니다.
2. **DispatcherServlet이 요청 수신**: 스프링의 프론트 컨트롤러인 `DispatcherServlet`이 모든 요청을 최초로 수신
3. **HandlerMapping을 통해 컨트롤러 검색**: `DispatcherServlet`은 `HandlerMapping`을 통해 요청 URL에 맞는 적절한 컨트롤러를 찾음
4. **컨트롤러 호출**: `DispatcherServlet`이 `HandlerMapping`이 찾은 컨트롤러를 호출하여 요청을 처리
5. **ModelAndView 반환**: 컨트롤러가 비즈니스 로직을 수행하고 그 결과를 `ModelAndView` 객체에 담아 반환
6. **ViewResolver를 통해 View 결정**: `DispatcherServlet`이 `ViewResolver`를 사용해 논리적 뷰 이름을 실제 뷰 객체로 변환
7. **뷰 렌더링 및 응답 반환**: 선택된 뷰가 모델 데이터를 사용해 HTML 등의 형식으로 응답을 생성하여 클라이언트에게 반환

## @RestController의 요청 처리 흐름

<img width="778" alt="스크린샷 2025-02-24 오전 10 45 21" src="https://github.com/user-attachments/assets/1ca943df-d096-4305-8b5b-af82fa75aa78" />


1. **클라이언트 요청 수신**: 클라이언트가 웹 브라우저나 API 클라이언트를 통해 서버로 HTTP 요청을 전송
2. **DispatcherServlet이 요청 수신**: 스프링의 프론트 컨트롤러인 `DispatcherServlet`이 모든 요청을 최초로 수신
3. **HandlerMapping을 통해 컨트롤러 검색**: `DispatcherServlet`이 요청 URL을 기반으로 `HandlerMapping`을 통해 적절한 컨트롤러를 찾음
4. **컨트롤러 호출**: `DispatcherServlet`이 `HandlerMapping`이 찾은 `@RestController`를 호출해 요청을 처리
5. **데이터 반환**: `@RestController`가 비즈니스 로직을 수행하고 결과 데이터를 객체로 반환
6. **HttpMessageConverter를 통한 직렬화**: `HttpMessageConverter`가 반환된 객체를 JSON이나 XML 형식으로 변환
7. **응답 반환**: 변환된 데이터를 HTTP 응답으로 클라이언트에게 전송

## @Controller vs @RestController 차이점

| **특징** | **@Controller** | **@RestController** |
| --- | --- | --- |
| **응답 형식** | View(JSP, HTML 등) | JSON, XML (데이터 자체) |
| **사용 어노테이션** | `@Controller`, `@ResponseBody` 필요 | `@RestController` 단독 사용 |
| **주요 용도** | 웹 애플리케이션 (UI 제공) | RESTful API 개발 |
| **데이터 직렬화** | 수동으로 `@ResponseBody` 사용 | 자동 직렬화 |

---

## 출처

 https://dev-coco.tistory.com/84

https://mangkyu.tistory.com/49
