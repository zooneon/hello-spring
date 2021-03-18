# 🌱 hello-spring
스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 👈 [강의 보러 가기](https://www.youtube.com/watch?v=-oeeqfRVrzI&list=PLumVmq_uRGHgBrimIp2-7MCnoPUskVMnd)

# 간단 정리
[Gradle](#gradle)

[정적 컨텐츠](#정적-컨텐츠)

[템플릿 엔진](#템플릿-엔진)

[API 방식](#api-방식)

[Annotation](#annotation)

[일반적인 웹 애플리케이션 계층 구조](#일반적인-웹-애플리케이션-계층-구조)

[테스트 케이스 작성](#테스트-케이스-작성)

## Gradle

Groovy를 이용한 빌드 자동화 시스템

- Maven과 같은 구조화된 build 프레임워크
- Artifacts(Files, Java source and class files 등등..) 빌드
- Dependencies 관리(자동 업데이트)

## 정적 컨텐츠

스프링부트에서 정적 컨텐츠 기능 지원

- 요청을 보내면 먼저 내장 톰캣 서버를 통해 관련 컨트롤러 찾음
- 컨트롤러가 없으면 resources: static/fileName.html 찾아서 반환

## 템플릿 엔진

템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성하여 결과 문서를 출력하는 소프트웨어

### Thymeleaf

- 서버사이드 자바 템플릿 엔진

  →  서버에서 DB 혹은 API에서 가져온 데이터를 미리 정의된 템플릿에 넣어 html을 그리고 클라이언트에 전달

- JSP와 달리 servlet 코드로 변환되지 않음
- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버 `viewResolver`가 화면을 찾아서 처리

    ```java
    @Controller
    public class HelloController {

        @GetMapping("hello")
        public String hello(Model model) {
            model.addAttribute("data", "spring!");
            return "hello";
        }
    }
    ```

  → resources: templates/hello.html 찾아서 처리

## API 방식

### @ResponseBody 문자 변환

```java
@Controller
public class HelloController {

		// @ResponseBody 문자 반환
    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
}
```

→ HTTP의 body에 문자 내용을 직접 반환

### @ResponseBody 객체 변환

```java
@Controller
public class HelloController {

    // 데이터를 내려줄 객체 생성
    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

		// @ResponseBody 객체 반환
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }
}
```

→ 객체를 반환하면 객체가 JSON으로 변환됨

### @ResponseBody 사용 원리

`@ResponseBody`를 사용

- HTTP의 body에 내용을 직접 반환
- `viewResolver` 대신에 `HttpMessageConverter`가 동작

  `StringHttpMessageConverter` : 기본 문자 처리

  `MappingJackson2HttpMessageConverter` : 기본 객체 처리

- byte 처리 등 여러 `HttpMessageConverter`가 기본으로 등록되어 있음

## Annotation

사전적 의미 : 주석

자바 annotation은 코드 사이에 주석처럼 쓰이며 특별한 의미, 기능을 수행하도록 함

프로그램에게 추가적인 정보를 주는 메타데이터

### 용도

- 컴파일러에게 코드 작성 문법 에러를 체크하도록 문법 제공
- 프로그램 빌드시 코드를 자동으로 생성할 수 있도록 정보 제공
- 실행시 특정 기능을 실행하도록 정보 제공

### 종류

**@SpringBootApplication**

- @Configuration, @EnableAutoConfiguration, @ComponentScan을 하나의 annotation으로 합친 것

**@Controller**

- Spring의 Controller를 의미, Spring MVC에서 Controller클래스에 쓰임

**@RequestMapping**

- URL요청을 어떤 method가 처리할지 mapping
- 요청 받는 형식을 정의하지 않는다면 자동으로 GET으로 설정

**@RequestParam**

- request의 파라미터에서 가져오는 것, method의 파라미터에 사용

**@GetMapping**

- @RequestMapping(Method=RequestMethod.GET)과 같다.

**@Repository**

- DB에 접근하는 method를 가지고 있는 클래스에 쓰임

**@Service**

- 비즈니스 로직을 수행하는 클래스라는 것을 나타냄

[참고](https://velog.io/@gillog/Spring-Annotation-정리#requestparam)
## 일반적인 웹 애플리케이션 계층 구조

![image](https://user-images.githubusercontent.com/59433441/111621126-f24bf400-882a-11eb-8ee0-93750bc8aa02.png)

- 컨트롤러 : 웹 MVC의 컨트롤러 역할
- 서비스 : 핵심 비즈니스 로직 구현
- 레포지토리 : DB에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 : 비즈니스 도메인 객체(회원, 주문, 쿠폰 등등..)

## 테스트 케이스 작성

`src/test/Java` 폴더에 패키지를 만든다.

클래스 이름 관례는 테스트할 클래스에 Test를 붙인다.(ExampleClassTest)

테스트할 메서드에 `@Test` 어노테이션을 붙인다.

모든 테스트는 메서드 순서 **상관없이** 다 따로 동작한다.(순서에 의존관계가 있으면 좋은 테스트가 아님)

### 테스트 라이브러리

- JUnit : 테스트 프레임워크
- Mockito : JUnit 위에서 동작하며 Mocking과 Verification을 도와주는 프레임워크
- AssertJ : 테스트 코드를 더 편리하게 작성하도록 도와주는 라이브러리
- spring-test : 스프링 통합 테스트 지원

### Assertions.assertThat

- `Assertions.assertThat`을 통해 기대한 값이 실제 값과 같은지 알 수 있음

```java
class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @Test
    public void save() {
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
				// 같으면 초록색, 다르면 빨간색
        assertThat(member).isEqualTo(result);
    }
}
```

### @AfterEach

- 메서드 실행이 끝날 때마다 작동하는 콜백 메세지

```java
class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clear();
    }
}
```

→ 각 테스트가 종료될 때마다 메모리 DB에 저장된 데이터 삭제

### given, when, then

- 뭔가가 주어졌을 때, 무엇을 실행해서 어떤 결과가 나와야하는지 알아보기 편함

```java
class MemberServiceTest {

    MemberService memberService;

    @Test
    void join() {
        // given
        Member member = new Member();
        member.setName("hello");

        // when
        Long saveId = memberService.join(member);

        // then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }
}
```
