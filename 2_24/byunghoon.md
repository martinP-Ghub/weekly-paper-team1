
## **SOAP에서 REST로의 전환 배경**

- SOAP(Simple Object Access Protocol)에서 REST(Representational State Transfer)로 전환된 이유는 웹 기술의 발전과 API 설계의 단순화 및 확장성 때문

### **1. 웹 친화성과 단순성**

- SOAP은 XML 기반의 무거운 메시지 구조와 WSDL을 사용해야 했다.
- REST는 HTTP 프로토콜을 활용하여 `GET`, `POST`, `PUT`, `DELETE` 등의 메서드를 사용하며, JSON과 같은 가벼운 데이터 형식을 지원해 웹 친화적이다.

### **2. 오버헤드 감소 및 성능 향상**

- SOAP은 XML을 사용하고 메시지 헤더가 커서 데이터 크기가 증가해 성능이 저하된다.
- REST는 가벼운 JSON을 사용하고 HTTP 기본 기능을 활용해 *오버헤드*를 줄일 수 있다.
    - **? 오버헤드**
        
        : 어떤 작업을 수행할 때 추가적으로 발생하는 비용이나 리소스를 의미
        
        **SOAP 방식** (XML 기반으로 크고 무거운 메시지)
        
        ```xml
        <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
           <soapenv:Header/>
           <soapenv:Body>
              <getUser>
                 <userId>12345</userId>
              </getUser>
           </soapenv:Body>
        </soapenv:Envelope>
        ```
        
        **REST 방식** (가벼운 JSON 형식)
        
        ```json
        GET /users/12345 HTTP/1.1
        Host: api.example.com
        Content-Type: application/json
        ```
        
        응답 (JSON)
        
        ```json
        {
           "id": 12345,
           "name": "John Doe",
           "email": "johndoe@example.com"
        }
        ```
        
        → SOAP은 XML을 사용하여 데이터 크기가 커지는 반면, REST는 JSON을 사용하여 가볍고 직관적
        

### **3. 유연한 데이터 포맷 지원**

- SOAP은 XML만 지원하지만, REST는 JSON, XML 등 다양한 포맷을 지원해 더 많은 클라이언트 환경과의 호환성이 높다.

### **4. 확장성과 마이크로서비스 아키텍처에 적합**

- REST는 *리소스 중심 설계*를 채택해 마이크로서비스 아키텍처와의 연계가 용이하다.
- 클라이언트-서버 간 결합도를 낮추고 확장성을 높여 클라우드 및 모바일 환경에서도 유리하다.
    - ? 리소스 중심 설계
        
        → **모든 데이터가 "리소스(Resource)"** 로 간주된다. 리소스는 고유한 **URI(Uniform Resource Identifier)** 로 식별되며, HTTP 메서드를 사용하여 조작된다.
        

---

## **REST의 단점**

### **1. Over-fetching & Under-fetching 문제**

- **Over-fetching**: 필요한 데이터보다 더 많은 데이터를 받아야 하는 경우
- **Under-fetching**: 한 번의 요청으로 원하는 데이터를 가져오지 못해 여러 번 API 호출이 필요한 경우

### **2. 상태 관리의 어려움 (Stateless의 한계)**

- REST는 Stateless(무상태) 원칙을 따르므로, 세션이나 사용자 인증 관리가 어렵다.
    - ? Stateless(무상태) 원칙
        
        → **서버가 클라이언트의 상태를 저장하지 않는 것**을 의미한다. REST API에서는 이 원칙을 따르며, 각 요청은 독립적으로 처리된다.
        

### **3. 보안 기능 부족**

- SOAP은 WS-Security를 통해 강력한 보안 기능을 제공하지만, REST는 기본적으로 HTTPS만 제공한다.

### **4. API 버전 관리의 어려움**

- REST는 URI 기반 리소스 관리로 인해 API 변경 시 버전 관리가 필요하다.

---
## **@RestController**

![image](https://github.com/user-attachments/assets/c8a5a323-8159-455e-8854-48255c10c757)

- 참고
    
    파란색 박스 : 스프링이 제공하는 기능 
    
    빨간색 박스 : 개발자가 구현해야하는 기능 
    
    녹색 박스 : 외부 시스템 (예 데이터 베이스)
    

### **1. 요청 수신 (Client → DispatcherServlet)**

- 클라이언트(웹 브라우저, 모바일 앱, API 클라이언트)가 HTTP 요청을 보냄
- `DispatcherServlet`이 요청을 수신하고, 이를 적절한 컨트롤러로 전달하기 위한 작업을 시작

### **2. 핸들러 매핑 (Handler Mapping)**

- `DispatcherServlet`은 요청 URI와 매핑된 컨트롤러를 찾기 위해 `HandlerMapping`을 호출
- `HandlerMapping`은 요청과 매핑된 컨트롤러(`@RestController`)를 찾음
- 찾은 컨트롤러 정보를 `DispatcherServlet`에게 전달

### **3. 컨트롤러 실행 (RestController)**

- 컨트롤러 실행 전에 `HttpMessageConverter`가 동작하여 JSON 데이터를 Java 객체로 변환
- `HandlerAdapter`는 매핑된 `@RestController`의 메서드를 실행

### **4. 비즈니스 로직 처리 (Service, Repository, Database)**

- 컨트롤러는 비즈니스 로직을 실행하기 위해 **Service**를 호출

### **5. 응답 변환 (Response Entity & HttpMessageConverter)**

- 컨트롤러에서 반환한 객체(`User`)는 `ResponseEntity`로 감싸져 응답
- 이때, `HttpMessageConverter`가 동작하여 객체를 JSON 또는 XML 등의 형식으로 변환

### **6. DispatcherServlet으로 응답 반환 후 클라이언트로 응답 전송**

- 변환된 응답이 `DispatcherServlet`으로 다시 전달
- `DispatcherServlet`이 최종적으로 변환된 데이터를 HTTP 응답으로 클라이언트에게 전송
- 클라이언트는 JSON 형식의 응답을 받게 됨
