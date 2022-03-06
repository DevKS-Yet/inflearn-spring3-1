### 2022-03-06
#### 프로젝트 생성
- start.spring.io에서 프로젝트 생성
- welcome page 만들기 (https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#web.servlet.spring-mvc.welcome-page)

#### 로깅 간단히 알아보기
- `@RestController`를 사용하면 해당 스트링이 그대로 반환됨
- `@RestController`는 Restful API의 핵심적인 것
- `application.properties`에서 로그의 레벨에 따라서 남기도록 설정 가능
  - `trace` -> `debug` -> `info` -> `warn` -> `error` 순으로
  - 개발 서버는 debug 출력
  - 운영 서버는 info 출력
  - 기본적으로는 info로 되어있음
- `@Slf4j`를 넣으면 롬복으로 인해 알아서 로거 생성자 넣어짐
- `log.trace("trace log = " + 변수명)`으로 출력을 해도 동일하게 출력이 되지만.. 작성을 하지 않더라도 자바 언어의 특성으로 연산 작업을 하게 되기에 꼭 `log.trace("trace log = {}", 변수명)`으로 작성하여 사용하지 않을 때는 해당 연산이 일어나지 않도록 하자
- 로거를 사용하면 콘솔에 출력이 아니라 파일로도 남길 수가 있음(분핢 및 백업 설정도 가능)
- 성능 자체가 일반 `System.out`보다 좋음. (내부 버퍼링, 멀티 쓰레드 등등)
- 더 공부할려면
  - SLF4J - http://www.slf4j.org
  - Logback - http://logback.qos.ch
  - spring boot log - https://docs.spring.io/spring-boot-docs/current/reference/html/spring-boot-features.html#boot-features-logging

#### 요청 매핑
- `@RestController`
  - 반환 값으로 뷰를 찾는 것이 아니라 HTTP 메시지 바디에 바로 입력 한다
- `@RequestMapping(경로)`
  - 해당 경로 값으로 URL 호출이 오면 해당 메서드 실행
  - 대부분의 속성은 배열로 제공하므로 다중 설정이 가능. (예: `{"/hello-basic", "/hello-go"`)
- 원칙상으로는 `/hello-basic`과 `/hello-basic/`은 다른 URL이지만 스프링에서는 자동

#### 요청 매핑 - API 예시
- 회원 관리 API

| 기능       | Method |Mapping|
|----------|--------|---|
| 회원 목록 조회 | GET    | `/users` |
| 회원 목록 | POST   | `/users` |
| 회원 조회 | GET    | `/users/{userId}` |
| 회원 수정 | PATCH  | `/users/{userId}` |
| 회원 삭제 | DELETE | `/users/{userId}` |

### 2022-03-07
#### HTTP 요청 - 기본, 헤더 조회
- locale 관련해서는 locale resolver에 대해서 공부해보자
- `MultiValueMap`은 하나의 키에 여러 값을 받을 수 있다.(???... 이거 카카오 코딩테스트에 사용 쌉가능... 하 괜히 힘들게 Map이랑 List로 뻘짓했네)
- `@Controller`의 사용 가능한 파라미터 목록은 다음 공식 메뉴얼에서 확인할 수 있다.  
    http://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments
- `@Controller`의 사용 가능한 응답 값 목록은 다음 공식 메뉴얼에서 확인할 수 있다.  
    http://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-return-types

#### HTTP 요청 파라미터 - 쿼리 파라미터, HTML Form
- 클라이언트에서 서버로 요청 데이터를 전달하는 방법
  - `GET`, `POST`, `HTTP message body`

#### HTTP 요청 파라미터 - @RequestParam
- `@RequestParam` : 파라미터 이름으로 바인딩
- `@ResponseBody` : View 조회를 무시하고 HTTP message body에 직접 해당 내용 입력
- HTTP 파라미터 이름이 변수 이름과 같으면 `@RequestParam(name = "??")` 생략 가능
- HTTP 파라미터가 `String`, `int`, `Integer` 등의 단순 타입이면 `@RequestParam`도 생략 가능
- 팀이 스프링에 아직 익숙하지 않는다면 `@RequestParam`을 쓰고 아니라면 뺴는것을 고민해볼만 하다
- `defaultValue`로 설정을 했을 경우 파라미터가 빈 문자("")일 경우에도 defaultValue로 설정됨

#### HTTP 요청 파라미터 - @ModelAttribute
- `@ModelAttribute`를 사용함으로써 지금까지 적어온 `@RequestParam` 같이 갖고 오는 파라미터를 다 없앨 수 있다
- `@ModelAttribute`도 생략 가능(argument resolver로 지정해둔 타입 외)


#### HTTP 요청 메시지 - 단순 텍스트
- `HTTP message body`에 데이터를 직접 담아서 요청
  - HTTP API에서 주로 사용, JSON, XML, TEXT
  - 데이터 형식은 주로 JSON 사용
  - POST, PUT, PATCH
- HttpEntity
  - 메시지 바디 정보를 직접 조회
  - 요청 파라미터를 조회하는 기능과 관계 없음
  - 응답에도 사용 가능
  - 헤더 정보 포함 가능
  - view 조회 하지 않음
- `@RequestBody`
  - HTTP 메시지 바디를 조회할 수 있ek
- HTTP Message Converter
- 요청 파라미터 vs HTTP 메시지 바디
  - 요청 파라미터를 조회하는 기능 : `@RequestParam`, `@ModelAttribute`
  - HTTP 메시지 바디를 직접 조회하는 기능 : `@RequestBody`
