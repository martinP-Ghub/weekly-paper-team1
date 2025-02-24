# AOP(Aspect Oriented Programming)

# 학습 목표

1. Spring에서 AOP(Aspect Oriented Programming)가 필요한 이유
2. 활용한 실제 애플리케이션 개발 사례에 대해 설명하세요.

---

# 학습 내용

# AOP(Aspect Oriented Programming)이란?

<aside>
💡

관점 지향 프로그래밍

Spring AOP는 Spring 프레임워크에서 AOP를 구현한 모듈로, 주로 로깅, 트랜잭션 관리, 보안 등과 같은 공통 기능을 분리하여 재사용할 수 있게 함

\*이름 그대로 **“어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화한다”** 는 것이다.

\*\* 모듈화 → 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것

여러 곳에서 반복되는 공통 기능를 별도의 “Aspect” 로 분리하여 코드중복을 줄이고 유지보수성을 높임

</aside>

## 주요 개념

1. Aspect : 공통 기능을 모듈화한 것
2. Join Point : Aspect가 적용될 수 있는 지점 (메서드 호출 시점이 대표적)
3. Advice : 특정 Join Point 에서 실행되는 코드
4. Point Cut : Advice가 적용될 Join Point 를 지정하는 표현식
5. Weaving : Aspect와 비즈니스 로직을 결합하는 과정

## AOP 필요한 이유

### 1. 중복 코드 제거 (코드 재사용성 증가)

- 여러 서비스 클래스에서 반복 **로깅, 보안 검사, 트랜잭션 관리** 코드를 추가하면 유지보수 어려움
  **→ AOP로 분리하여 한 곳에서 관리 가능**

### 2. 핵심 비즈니스 로직과 보조 기능을 분리 (관심사 분리, SRP 원칙 준수)

- 핵심 비즈니스 로직(Service)과 공통 기능(Logging, Security, Transaction 등) 분리
  **→ 서비스 코드 간결하게 유지**

### 3. 유지보수성 향상

- 공통 기능이 한 곳 (AOP)에서 관리되므로 변경 시 전체 코드 수정 없이 쉽게 적용 가능

### 4. 어플리케이션 성능 향상

- AOP를 활용하면 불필요한 메서드 호출을 줄이고 효율적으로 실행되는 공통 로직을 적용 가능

## AOP 적용 전후 코드 비교

### AOP 적용 전 (중복 코드 발생)

```java
@Service
public class PaymentService {
	private final PaymentReopository paymentRepository;

	public PaymentService(PaymentRepository paymentRepository) {
      this.paymentRepository = paymentRepository;
  }

	public void processPayment(int amount) {
		System.out.println("[LOG] 결제 프로세서 시작");

		paymentRepository.save(amount);

		System.out.println("[LOG] 결제 프로세서 종료");
	}
}

// 문제점
// -> 로깅 기능이 여러 서비스 클래스에 반복됨
// -> 유지보수가 어려워짐
```

### AOP 적용 후 (중복 제거)

```java
@Aspect
@Component
public class LoggingAspect {

	@Before("execution(* com.example.service.*.*(..))")
	public void breforeMethod() {
		System.out.println("[LOG] 결제 프로세서 시작");
	}

	@After("execution(* com.example.service.*.*(..))")
	public void afterMethod() {
		System.out.println("[LOG] 결제 프로세서 종료");
	}
}
```

```java
@Service
public class PaymentService {
	private final PaymentReopository paymentRepository;

	public PaymentService(PaymentRepository paymentRepository) {
      this.paymentRepository = paymentRepository;
  }

	public void processPayment(int amount) {
		paymentRepository.save(amount);
	}
}

// 개선점
// -> 로깅 코드가 서비스 클래스에서 사라지고, LogginAspect 에서 한 번만 정의
// -> 코드가 간결해지고 유지보수가 쉬워짐
```

## Spring AOP 의 동작 방식

Spring AOP는 주로 프록시 기반으로 동작하며 실제 객체 대신 프록시 객체를 생성하여 Aspect를 적용함

이를 통해 공통 기능을 비즈니스 로직과 분리할 수 있음

**프록시 객체**

```java
+--------+          +---------+          +---------------+
| Client | ----->   |  Proxy  | ----->   | Target Object |
+--------+          +---------+          +---------------+
                      ↑    |
                      |    |
               [Advice (트랜잭션 관리)]
```

- Target Object의 메서드를 대신 호출하는 중간 객체
- 클라이언트의 요청을 가로채서 Advice를 실행한 후, 실제 Target Object의 메서드를 호출
- 프록시는 Target Object와 동일한 인터페이스를 구현하여 클라이언트가 프록시를 통해 Target Object에 접근하는 것처럼 보이게 함

## Spring AOP가 프록시 방식을 사용하는 이유

### **1. 런타임 시점의 삽입 가능:**

Spring AOP는 컴파일 시점이 아니라 런타임 시점에 프록시 객체를 생성하여 부가적인 기능을 주업무 코드에 삽입

이는 AOP를 적용하는 대상 클래스의 바이트코드를 수정하지 않고, 부가적인 기능을 주입할 수 있게 함

따라서 애플리케이션을 실행 중에 동적으로 관심사를 적용 가능

### **2. 코드 재사용과 모듈화:**

여러 클래스나 메서드에서 공통적으로 필요한 기능(예: 로깅, 보안 체크, 트랜잭션 관리 등)을 하나의 공통 모듈로 분리하여 관리 가능

이를 AOP로 구현한 프록시 객체를 여러 클래스에서 재사용할 수 있으며, 이로 인해 코드 중복을 줄이고, 유지보수성을 향상시킴

### **3. 관심사의 분리:**

AOP는 관심사를 주업무 코드와 분리하여 코드의 가독성을 높이고 유지보수를 용이하게 함

주업무 코드는 핵심 비즈니스 로직에만 집중할 수 있고, 부가적인 기능은 프록시 객체가 담당하게 됨

이로 인해 코드의 응집도가 증가하고 결합도가 감소하여 코드의 이해와 관리가 쉬워

### **4. 런타임 선택 가능:**

AOP를 프록시 방식으로 구현하면 필요한 관심사를 선택적으로 적용 가능

어떤 메서드에만 특정 기능을 적용하거나, 특정 조건을 만족하는 경우에만 부가적인 기능을 실행하도록 설정 가능

---

# AOP 활용 실제 어플리케이션 개발 사례

## 1. 로깅(Logging) 시스템 적용

### 사례

- 대규모 트래픽을 처리하는 웹 어플리케이션에서 API 요청 및 응답 로그를 남겨야 할 때
- @Before , @AfterReturning 을 사용하여 각 메스드의 실행 로그를 자동으로 기록

### 적용 코드

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.controller.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("[LOG] 요청 메서드: " + joinPoint.getSignature().getName());
    }

    @AfterReturning("execution(* com.example.controller.*.*(..))")
    public void logAfter(JoinPoint joinPoint) {
        System.out.println("[LOG] 요청 완료: " + joinPoint.getSignature().getName());
    }
}
```

### 효과

- API 호출 로그가 자동으로 기록됨 → 디버깅 및 문제 해결이 쉬워짐

## 2. 트랜잭션 관리 ([@Transactional](https://www.notion.so/Transactional-19f86243d9998029a725fc5bf465fcbf?pvs=21))

### 사례

- 데이터베이스 연산 중 예외가 발생하면 롤백(Rollback)해야 할 경우
- Spring의 @Transactional을 AOP로 적용하여 모든 서비스 메서드에 일관된 트랜잭션 관리 가능

### 적용 코드

```java
@Aspect
@Component
public class TransactionAspect {

    @Around("@annotation(org.springframework.transaction.annotation.Transactional)")
    public Object manageTransaction(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("[TRANSACTION] 시작");
        Object result;
        try {
            result = joinPoint.proceed();
            System.out.println("[TRANSACTION] 커밋");
        } catch (Exception e) {
            System.out.println("[TRANSACTION] 롤백");
            throw e;
        }
        return result;
    }
}
```

### 효과

- 모든 @Transactional 메소드가 자동으로 트랜잭션 관리됨
- 데이터 무결성을 유지할 수 있음

## 3. 보안(Security) - 권한 체크

### 사례

- 사용자의 역할(Role)에 따라 특정 API 접근 제한해야 할 때
- AOP를 활용하여 모든 컨트롤러 요청 전에 사용자 인증 및 권한 검사 가능

### 코드

```java
@Aspect
@Component
public class SecurityAspect {

    @Before("execution(* com.example.controller.*.*(..))")
    public void checkUserAuthentication() {
        String userRole = getCurrentUserRole(); // 가정: 현재 로그인한 사용자의 역할 가져오기
        if (!"ADMIN".equals(userRole)) {
            throw new SecurityException("[SECURITY] 접근 거부 - 관리자만 접근 가능");
        }
    }

    private String getCurrentUserRole() {
        return "USER"; // 예제: 실제로는 인증 시스템과 연동
    }
}

```

### 효과

- 모든 API 요청 전에 자동으로 보안 검사를 수행
- 개발자가 매번 `if (userRole ≠ “ADMIN”)` 같은 코드를 작성할 필요 없음

---

# Spring MVC - @Controller @RestController

# 학습 목표

1. Spring MVC에서 클라이언트의 요청 처리 흐름을 @Controller와 @RestController의 차이점을 중심으로 각각의 처리 과정과 특징을 포함하여 설명하세요.

---

# 학습 내용

# Spirng MVC

## Spring MVC 구성요소

### DispatcherServlet

- 클라이언트의 요청을 받아들이고 요청을 처리하기 위해 다른 구성 요소들을 호출
- 클라이언트의 요청을 전달받아 요청에 맞는 컨트롤러가 리턴한 값을 View에 전달하여 알맞은 응답을 생성

### HandlerMapping

- 요청 URL과 일치하는 컨트롤러를 찾아주는 역할

### Controller

- 요청을 처리하는 비즈니스 로직을 수행하는 컴포넌트
- 클라이언트의 요청을 처리한 후, 결과를 DispatcherServlet 에게 리턴

### Model

- 컨트롤러가 처리한 결과를 저장하는 객체

### ViewResolver

- View 이름을 실제 View 객체로 변환해 주는 역할

### View

- 컨트롤러가 처리한 결과를 보여주는 역할

![image.png](/Users/taesikpark/Downloads/mvc/image.png)

## 기본 요청 처리 방식

1. **`클라이언트(웹 브라우저)`**가 서버에 요청하면 **`DispatcherServlet`**이라는 클래스에게 요청이 전달
2. **`DispatcherServlet`**은 클라이언트 요청을 처리할 **`Controller`**에 대한 검색을 **`HandlerMapping`** 인터페이스에게 요청
3. **`HandlerMapping`**은 클라이언트 요청에 해당하는 **`Controller`**를 찾고 해당 **`Controller`** 정보를 다시 **`DispatcherServlet`**에게 응답
   1. **Contorller 정보** → 해당 **`Controller`** 안에 **`Handler`** 메소드 정보도 가지고 있음
   2. **Handler 메소드 →** **`Controller`** 클래스 안에 구현된 요청을 처리해주는 메소드
4. 요청을 처리할 **`Controller`**를 찾아 클라이언트 요청을 처리할 **`Handler`** 메소드를 찾아 호출
   **`DispatcherServlet`**은 **`Handler`** 메소드를 직접 호출하지 않고 **`HandlerAdapter`**에게 **`Handler`** 메소드 호출 책임을 넘김
5. **`HandlerAdapter`**은 **`DispatcherServlet`**의 요청과 더불어 **`Controller`** 정보를 가지고 해당 **`Contorller`**의 **`Handler`** 메소드를 호출
6. **`Controller`**의 **`Handler`** 메소드는 비즈니스 로직을 처리한 후 리턴받은 **`Model`** 데이터를 **`HandlerAdapter`**에게 반환
7. **`HandlerAdapter`**은 반환받은 **`Model`** 데이터와 **`View`** 이름을 다시 **`DispathcerServlet`**에게 반환
8. **`DispatcherServlet`**은 반환받은 2개의 데이터 중 **`View`** 이름을 **`VIewResolver`**에게 해당 **`View`**를 반환 요청
9. **`ViewResolver`**는 **`View`** 정보에 해당하는 **`View`**를 찾아 **`DispatcherServlet`**에게 반환
10. **`DispatcherServlet`**은 **`ViewResolver`**에게 받은 **`View`** 객체에게 **`Model`** 데이터를 넘겨주며 **`클라이언트`**에게 전달할 응답 데이터를 생성 요청
11. **`View`**는 응답 데이터를 생성해 다시 **`DispatcherServlet`**에게 반환
12. **`DispatcherServlet`**은 **`View`**로 부터 받은 응답데이터를 마지막으로 **`클라이언트`**에게 응답

---

# @Controller 와 @RestController 의 차이 및 요청 처리 과정

## 1. @Controller → View(HTML) 반환

### 특징

- 주로 웹 어플리케이션에서 HTML을 랜더링할 때 사용
- `return “뷰 이름”` 을 반환하면 View Resolver가 실행되어 HTML 페이지를 렌더링 함

### 요청 처리 흐름

1. 클라이언트 요청(ex : `GET /hello`)
2. `DispatcherServlet`이 `@Controller`로 요청 전달
3. `Controller`가 처리 후 뷰 이름 반환 (`return “hello”`)
4. `View Resolver`가 실행되어 `hello.html` 랜더링
5. 최종 HTML이 클라이언트에게 응답으로 전달

### 코드 예제

```java
@Controller
public class MyController {

    @GetMapping("/hollo")
    public String hello(Model model) {
        model.addAttribute("message", "Hello, Spring!");
        return "hello";
    }
}
```

## 2. @RestController → JSON 데이터 반환 (API)

### 특징

- RESTful API 개발 시 사용
- HTML을 랜더링하지 않고, JSON 또는 XML 데이터를 직접 반환
- `@ResponseBody` 가 자동 포함되어 있어 메서드 반환값이 HTTP 응답 본문(body)으로 직접 전달

### 요청 처리 흐름

1. 클라이언트 요청 (ex : `GET /api/hello`)
2. `DispatcherServlet`이 `@RestController`로 요청 전달
3. `@RestContorller`가 데이터를 JSON 형식으로 변환하여 반환
4. 클라이언트가 JSON 데이터를 직접 받음

### 코드 예제

```java
@RestController
public class MyRestController {

    @GetMapping("/api/hello")
    public Map<String, String> hello() {
        Map<String, String> response = new HashMap<>();
        response.pub("message", "Hello, REST API!");
    }
}
```

### 실행 결과 (`GET /api/hello` 요청)

```java
{
    "message" : "Hello, REST API!"
}
```

---

# 정리

| 구분                    | @Controller             | @RestContorller                   |
| ----------------------- | ----------------------- | --------------------------------- |
| 주 사용 목적            | HTML 페이지 반환(웹 뷰) | JSON / XML 데이터 반환 (REST API) |
| View Resolver 실행 여부 | 실행 (뷰 템플릿 사용)   | 실행 안 됨 (데이터 직접 반환)     |
| ResponseBody 포함 여부  | 없음 (뷰 이름 반환)     | 자동 포함 (데이터 반환)           |
| 주요 사용 예제          | JSP, Thymeleaf          | RESTful API                       |
