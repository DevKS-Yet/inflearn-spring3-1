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