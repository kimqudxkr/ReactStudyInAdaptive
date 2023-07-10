# 2023-07-11 (목)
- - - 
### __Layered Architecture Pattern__
  어떻게 코드를 적절히 분리하고 관리할 것인지 

* Controller
  * DTO : 클라이언트의 데이터를 받는 역할
* Service
  * model : 비즈니스 데이터를 담는 역할
* Persistence
  * entity : DB 테이블, 스키마를 표현
* DB
- - - 
### __REST API__
#### REpresentational State Transfer

REST 제약조건 (준수해야 함)
* Client - Server
  * 서버와 클라이언트를 분리하여 의존하지 않는 구조
  * Server : 리소스 관리
  * Client : 리소스 소비를 위해 서버에 접근 
* Stateless
  * 다음 페이지로 라우팅될 때 이전 페이지의 정보에 영향을 받지 않아야 함
  * 세션과 같은 인증이 필요하다면 클라이언트가 토큰과 같이 인증에 필요한 정보를 서버에 전달해야 함
* Cacheable
  * 클라이언트로부터 동일한 요청이 들어올 경우 응답 데이터를 재사용
  * 캐시 가능 여부 명시 필요
* Uniform Interface
  * 리소스를 가져오기 위한 인터페이스가 일관적이어야 함
* Layered System
  * 여러 다른 서버를 거쳐 데이터를 가져올 수 있음
  * 클라이언트를 거치는 서버의 존재 유무를 알 수 없어야 없다
* Code-On-Demand (필수 X)
  * 클라이언트는 서버에 코드를 요청 가능
  * 해당 코드를 서버로부터 받아 실행 가능
- - -
### Controller
#### @RequestMapping
해당 컨트롤러가 어느 URL과 매핑될 것인지 지정
```java
@RestController
@RequestMapping("test")
public class TestControler {
    ...
}
```
#### @GetMapping("url")
요청 URL(/url)로 GET 메소드가 요청될 경우 처리하는 부분
```java
@RestController
@RequestMapping("test")
public class TestControler {
    @GetMapping
    public String test() {
        return "hello world!";
    }
}
```
- - -
#### 매개변수 전달받을 때
* @PathVariable
  ```java
  @RestController
    @RequestMapping("test")
    public class TestControler {
        @GetMapping("/{id}")
        public String testControllerWithPath(@PathVariable(required = false) int id) {
            return "hello world! test get mapping! "+id;
        }
    }
    ``` 
  * localhost:8080/test/__123__
  * @GetMapping에 선언된 매개변수 이름과 동일한 데이터를 변수에 할당
* @RequestParam
  ```java
  @RestController
    @RequestMapping("test")
    public class TestControler {
        @GetMapping("/testRequestParam")
        public String testControllerRequestParam(@RequestParam(required = false) int id) {
            return "hello world! ID : "+id;
        }
    }
    ``` 
  * localhost:8080/testRequestParam?__id=123__
  * URL 마지막 부분에 물음표와 함께 "파라미터명"="파라미터값" 으로 값 전달 
* @RequestBody
  ```java
  @RestController
    @RequestMapping("test")
    public class TestControler {
        @GetMapping("/testRequestBody")
        public String testControllerRequestBody(@RequestBody TestRequestBodyDTO testRequestBodyDTO) {
            return "hello world!\nID : "+testRequestBodyDTO.getId()+"\nmessage : "+testRequestBodyDTO.getMessage();
        }
    }
    ``` 
  * 미리 선언해두었던 TestRequestBodyDTO 형태의 객체를 전달받을 수 있다.
  ```json
  {
    "id" : 123,
    "message" : "test"
  }
  ```
  * 전달 시 위와 같은 JSON 형태로 전달하며, TestRequestBodyDTO에 선언되어 있는 필드들과 동일한 형태의 파라미터를 포함한 객체를 전달하여야 한다
- - -
#### 매개변수 전달해줄 때
* @ResponseBody
* 