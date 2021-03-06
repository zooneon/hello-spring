# ๐ฑ hello-spring
์คํ๋ง ์๋ฌธ - ์ฝ๋๋ก ๋ฐฐ์ฐ๋ ์คํ๋ง ๋ถํธ, ์น MVC, DB ์ ๊ทผ ๊ธฐ์  ๐ [๊ฐ์ ๋ณด๋ฌ ๊ฐ๊ธฐ](https://www.youtube.com/watch?v=-oeeqfRVrzI&list=PLumVmq_uRGHgBrimIp2-7MCnoPUskVMnd)

# ๊ฐ๋จ ์ ๋ฆฌ
[Gradle](#gradle)

[์ ์  ์ปจํ์ธ ](#์ ์ -์ปจํ์ธ )

[ํํ๋ฆฟ ์์ง](#ํํ๋ฆฟ-์์ง)

[API ๋ฐฉ์](#api-๋ฐฉ์)

[Annotation](#annotation)

[์ผ๋ฐ์ ์ธ ์น ์ ํ๋ฆฌ์ผ์ด์ ๊ณ์ธต ๊ตฌ์กฐ](#์ผ๋ฐ์ ์ธ-์น-์ ํ๋ฆฌ์ผ์ด์-๊ณ์ธต-๊ตฌ์กฐ)

[ํ์คํธ ์ผ์ด์ค ์์ฑ](#ํ์คํธ-์ผ์ด์ค-์์ฑ)

[์คํ๋ง ๋น๊ณผ ์์กด๊ด๊ณ](#์คํ๋ง-๋น๊ณผ-์์กด๊ด๊ณ)

[์คํ๋ง ํตํฉ ํ์คํธ](#์คํ๋ง-ํตํฉ-ํ์คํธ)

[JPA](#jpa)

## Gradle

Groovy๋ฅผ ์ด์ฉํ ๋น๋ ์๋ํ ์์คํ

- Maven๊ณผ ๊ฐ์ ๊ตฌ์กฐํ๋ build ํ๋ ์์ํฌ
- Artifacts(Files, Java source and class files ๋ฑ๋ฑ..) ๋น๋
- Dependencies ๊ด๋ฆฌ(์๋ ์๋ฐ์ดํธ)

## ์ ์  ์ปจํ์ธ 

์คํ๋ง๋ถํธ์์ ์ ์  ์ปจํ์ธ  ๊ธฐ๋ฅ ์ง์

- ์์ฒญ์ ๋ณด๋ด๋ฉด ๋จผ์  ๋ด์ฅ ํฐ์บฃ ์๋ฒ๋ฅผ ํตํด ๊ด๋ จ ์ปจํธ๋กค๋ฌ ์ฐพ์
- ์ปจํธ๋กค๋ฌ๊ฐ ์์ผ๋ฉด resources: static/fileName.html ์ฐพ์์ ๋ฐํ

## ํํ๋ฆฟ ์์ง

ํํ๋ฆฟ ์์๊ณผ ํน์  ๋ฐ์ดํฐ ๋ชจ๋ธ์ ๋ฐ๋ฅธ ์๋ ฅ ์๋ฃ๋ฅผ ํฉ์ฑํ์ฌ ๊ฒฐ๊ณผ ๋ฌธ์๋ฅผ ์ถ๋ ฅํ๋ ์ํํธ์จ์ด

### Thymeleaf

- ์๋ฒ์ฌ์ด๋ ์๋ฐ ํํ๋ฆฟ ์์ง

  โ  ์๋ฒ์์ DB ํน์ API์์ ๊ฐ์ ธ์จ ๋ฐ์ดํฐ๋ฅผ ๋ฏธ๋ฆฌ ์ ์๋ ํํ๋ฆฟ์ ๋ฃ์ด html์ ๊ทธ๋ฆฌ๊ณ  ํด๋ผ์ด์ธํธ์ ์ ๋ฌ

- JSP์ ๋ฌ๋ฆฌ servlet ์ฝ๋๋ก ๋ณํ๋์ง ์์
- ์ปจํธ๋กค๋ฌ์์ ๋ฆฌํด ๊ฐ์ผ๋ก ๋ฌธ์๋ฅผ ๋ฐํํ๋ฉด ๋ทฐ ๋ฆฌ์กธ๋ฒ `viewResolver`๊ฐ ํ๋ฉด์ ์ฐพ์์ ์ฒ๋ฆฌ

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

  โ resources: templates/hello.html ์ฐพ์์ ์ฒ๋ฆฌ

## API ๋ฐฉ์

### @ResponseBody ๋ฌธ์ ๋ณํ

```java
@Controller
public class HelloController {

    // @ResponseBody ๋ฌธ์ ๋ฐํ
    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
}
```

โ HTTP์ body์ ๋ฌธ์ ๋ด์ฉ์ ์ง์  ๋ฐํ

### @ResponseBody ๊ฐ์ฒด ๋ณํ

```java
@Controller
public class HelloController {

    // ๋ฐ์ดํฐ๋ฅผ ๋ด๋ ค์ค ๊ฐ์ฒด ์์ฑ
    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

    // @ResponseBody ๊ฐ์ฒด ๋ฐํ
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }
}
```

โ ๊ฐ์ฒด๋ฅผ ๋ฐํํ๋ฉด ๊ฐ์ฒด๊ฐ JSON์ผ๋ก ๋ณํ๋จ

### @ResponseBody ์ฌ์ฉ ์๋ฆฌ

`@ResponseBody`๋ฅผ ์ฌ์ฉ

- HTTP์ body์ ๋ด์ฉ์ ์ง์  ๋ฐํ
- `viewResolver` ๋์ ์ `HttpMessageConverter`๊ฐ ๋์

  `StringHttpMessageConverter` : ๊ธฐ๋ณธ ๋ฌธ์ ์ฒ๋ฆฌ

  `MappingJackson2HttpMessageConverter` : ๊ธฐ๋ณธ ๊ฐ์ฒด ์ฒ๋ฆฌ

- byte ์ฒ๋ฆฌ ๋ฑ ์ฌ๋ฌ `HttpMessageConverter`๊ฐ ๊ธฐ๋ณธ์ผ๋ก ๋ฑ๋ก๋์ด ์์

## Annotation

์ฌ์ ์  ์๋ฏธ : ์ฃผ์

์๋ฐ annotation์ ์ฝ๋ ์ฌ์ด์ ์ฃผ์์ฒ๋ผ ์ฐ์ด๋ฉฐ ํน๋ณํ ์๋ฏธ, ๊ธฐ๋ฅ์ ์ํํ๋๋ก ํจ

ํ๋ก๊ทธ๋จ์๊ฒ ์ถ๊ฐ์ ์ธ ์ ๋ณด๋ฅผ ์ฃผ๋ ๋ฉํ๋ฐ์ดํฐ

### ์ฉ๋

- ์ปดํ์ผ๋ฌ์๊ฒ ์ฝ๋ ์์ฑ ๋ฌธ๋ฒ ์๋ฌ๋ฅผ ์ฒดํฌํ๋๋ก ๋ฌธ๋ฒ ์ ๊ณต
- ํ๋ก๊ทธ๋จ ๋น๋์ ์ฝ๋๋ฅผ ์๋์ผ๋ก ์์ฑํ  ์ ์๋๋ก ์ ๋ณด ์ ๊ณต
- ์คํ์ ํน์  ๊ธฐ๋ฅ์ ์คํํ๋๋ก ์ ๋ณด ์ ๊ณต

### ์ข๋ฅ

**@SpringBootApplication**

- @Configuration, @EnableAutoConfiguration, @ComponentScan์ ํ๋์ annotation์ผ๋ก ํฉ์น ๊ฒ

**@Controller**

- Spring์ Controller๋ฅผ ์๋ฏธ, Spring MVC์์ Controllerํด๋์ค์ ์ฐ์

**@RequestMapping**

- URL์์ฒญ์ ์ด๋ค method๊ฐ ์ฒ๋ฆฌํ ์ง mapping
- ์์ฒญ ๋ฐ๋ ํ์์ ์ ์ํ์ง ์๋๋ค๋ฉด ์๋์ผ๋ก GET์ผ๋ก ์ค์ 

**@RequestParam**

- request์ ํ๋ผ๋ฏธํฐ์์ ๊ฐ์ ธ์ค๋ ๊ฒ, method์ ํ๋ผ๋ฏธํฐ์ ์ฌ์ฉ

**@GetMapping**

- @RequestMapping(Method=RequestMethod.GET)๊ณผ ๊ฐ๋ค.

**@Repository**

- DB์ ์ ๊ทผํ๋ method๋ฅผ ๊ฐ์ง๊ณ  ์๋ ํด๋์ค์ ์ฐ์

**@Service**

- ๋น์ฆ๋์ค ๋ก์ง์ ์ํํ๋ ํด๋์ค๋ผ๋ ๊ฒ์ ๋ํ๋

[์ฐธ๊ณ ](https://velog.io/@gillog/Spring-Annotation-์ ๋ฆฌ#requestparam)
## ์ผ๋ฐ์ ์ธ ์น ์ ํ๋ฆฌ์ผ์ด์ ๊ณ์ธต ๊ตฌ์กฐ

![image](https://user-images.githubusercontent.com/59433441/111621126-f24bf400-882a-11eb-8ee0-93750bc8aa02.png)

- ์ปจํธ๋กค๋ฌ : ์น MVC์ ์ปจํธ๋กค๋ฌ ์ญํ 
- ์๋น์ค : ํต์ฌ ๋น์ฆ๋์ค ๋ก์ง ๊ตฌํ
- ๋ ํฌ์งํ ๋ฆฌ : DB์ ์ ๊ทผ, ๋๋ฉ์ธ ๊ฐ์ฒด๋ฅผ DB์ ์ ์ฅํ๊ณ  ๊ด๋ฆฌ
- ๋๋ฉ์ธ : ๋น์ฆ๋์ค ๋๋ฉ์ธ ๊ฐ์ฒด(ํ์, ์ฃผ๋ฌธ, ์ฟ ํฐ ๋ฑ๋ฑ..)

## ํ์คํธ ์ผ์ด์ค ์์ฑ

`src/test/Java` ํด๋์ ํจํค์ง๋ฅผ ๋ง๋ ๋ค.

ํด๋์ค ์ด๋ฆ ๊ด๋ก๋ ํ์คํธํ  ํด๋์ค์ Test๋ฅผ ๋ถ์ธ๋ค.(ExampleClassTest)

ํ์คํธํ  ๋ฉ์๋์ `@Test` ์ด๋ธํ์ด์์ ๋ถ์ธ๋ค.

๋ชจ๋  ํ์คํธ๋ ๋ฉ์๋ ์์ **์๊ด์์ด** ๋ค ๋ฐ๋ก ๋์ํ๋ค.(์์์ ์์กด๊ด๊ณ๊ฐ ์์ผ๋ฉด ์ข์ ํ์คํธ๊ฐ ์๋)

### ํ์คํธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ

- JUnit : ํ์คํธ ํ๋ ์์ํฌ
- Mockito : JUnit ์์์ ๋์ํ๋ฉฐ Mocking๊ณผ Verification์ ๋์์ฃผ๋ ํ๋ ์์ํฌ
- AssertJ : ํ์คํธ ์ฝ๋๋ฅผ ๋ ํธ๋ฆฌํ๊ฒ ์์ฑํ๋๋ก ๋์์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ
- spring-test : ์คํ๋ง ํตํฉ ํ์คํธ ์ง์

### Assertions.assertThat

- `Assertions.assertThat`์ ํตํด ๊ธฐ๋ํ ๊ฐ์ด ์ค์  ๊ฐ๊ณผ ๊ฐ์์ง ์ ์ ์์

```java
class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @Test
    public void save() {
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        // ๊ฐ์ผ๋ฉด ์ด๋ก์, ๋ค๋ฅด๋ฉด ๋นจ๊ฐ์
        assertThat(member).isEqualTo(result);
    }
}
```

### @AfterEach

- ๋ฉ์๋ ์คํ์ด ๋๋  ๋๋ง๋ค ์๋ํ๋ ์ฝ๋ฐฑ ๋ฉ์ธ์ง

```java
class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clear();
    }
}
```

โ ๊ฐ ํ์คํธ๊ฐ ์ข๋ฃ๋  ๋๋ง๋ค ๋ฉ๋ชจ๋ฆฌ DB์ ์ ์ฅ๋ ๋ฐ์ดํฐ ์ญ์ 

### given, when, then

- ๋ญ๊ฐ๊ฐ ์ฃผ์ด์ก์ ๋, ๋ฌด์์ ์คํํด์ ์ด๋ค ๊ฒฐ๊ณผ๊ฐ ๋์์ผํ๋์ง ์์๋ณด๊ธฐ ํธํจ

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

## ์คํ๋ง ๋น๊ณผ ์์กด๊ด๊ณ

๊ฐ์ฒด ์์กด๊ด๊ณ๋ฅผ ์ธ๋ถ์์ ๋ฃ์ด์ฃผ๋ ๊ฒ์ **DI**(Dependency Injection), **์์กด์ฑ ์ฃผ์**์ด๋ผ๊ณ  ํ๋ค.

DI์๋ ํ๋ ์ฃผ์, setter ์ฃผ์, ์์ฑ์ ์ฃผ์ 3๊ฐ์ง ๋ฐฉ๋ฒ์ด ์๋๋ฐ ์์กด๊ด๊ณ๊ฐ ์คํ์ค์ ๋์ ์ผ๋ก ๋ณํ๋ ๊ฒฝ์ฐ๋ ๊ฑฐ์ ์์ผ๋ฏ๋ก ์์ฑ์ ์ฃผ์์ ๊ถ์ฅํ๋ค.

`@Autowired` ๋ฅผ ํตํ DI๋ ์คํ๋ง์ด ๊ด๋ฆฌํ๋ ๊ฐ์ฒด์์๋ง ๋์ํ๋ค.(์คํ๋ง ๋น์ผ๋ก ๋ฑ๋กํ์ง ์๊ณ  ์ง์  ์์ฑํ ๊ฐ์ฒด์์๋ ๋์ํ์ง ์์)

์์ฑ์์ `@Autowired` ๋ฅผ ์ฌ์ฉํ๋ฉด ๊ฐ์ฒด ์์ฑ ์์ ์ ์คํ๋ง ์ปจํ์ด๋์์ ํด๋น ์คํ๋ง ๋น์ ์ฐพ์ ์ฃผ์ํ๋ค.

์คํ๋ง ์ปจํ์ด๋์ ์คํ๋ง ๋น์ ๋ฑ๋กํ  ๋, ๊ธฐ๋ณธ์ผ๋ก **์ฑ๊ธํค์ผ๋ก** ๋ฑ๋กํ๋ค.(ํ๋๋ง ๋ฑ๋กํด์ ๊ณต์ )

### ์ปดํฌ๋ํธ ์ค์บ๊ณผ ์๋ ์์กด๊ด๊ณ ์ค์ 

- `@Component` ์ด๋ธํ์ด์์ด ์์ผ๋ฉด ์คํ๋ง ๋น์ผ๋ก ์๋ ๋ฑ๋ก
- `@Component` ์ ํฌํจํ๋ ์ด๋ธํ์ด์๋ค๋ ์คํ๋ง ๋น์ผ๋ก ์๋ ๋ฑ๋ก
  - `@Controller`
  - `@Service`
  - `@Repository`
- ์ ํํ๋ ์ปจํธ๋กค๋ฌ, ์๋น์ค, ๋ ํฌ์งํ ๋ฆฌ ๊ฐ์ ์ฝ๋์ ์ฌ์ฉ

```java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
	       this.memberService = memberService;
    }
}
```

### ์๋ฐ ์ฝ๋๋ก ์ง์  ์คํ๋ง ๋น ๋ฑ๋กํ๊ธฐ

- ์ง์  Configuration ํ์ผ์ ๋ง๋ค์ด ์ฌ์ฉ
- `@Bean` ์ด๋ธํ์ด์์ ์ฌ์ฉํ์ฌ ์คํ๋ง ๋น์ผ๋ก ๋ฑ๋ก
- ์ ํํ ๋์ง ์๊ฑฐ๋, ์ํฉ์ ๋ฐ๋ผ ๊ตฌํ ํด๋์ค๋ฅผ ๋ณ๊ฒฝํด์ผ ํ๋ ๊ฒฝ์ฐ ์ฌ์ฉ

```java
@Configuration
public class SpringConfig {
    
    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }
    
    @Bean
    public MemberRepository memberRepository() {
      return new MemoryMemberRepository();
    }
}
```

## ์คํ๋ง ํตํฉ ํ์คํธ

```java
@SpringBootTest
@Transactional
class MemberServiceIntegrationTest {

    @Autowired MemberService memberService;
    @Autowired MemberRepository memberRepository;

    @Test
    void join() {
        ...
    }
		
    ...
}
```

### @SpringBootTest

์คํ๋ง ์ปจํ์ด๋์ ํ์คํธ๋ฅผ ํจ๊ป ์คํํ๋ค.

### @Transactional

ํ์คํธ ์ผ์ด์ค์ ์ด ์ด๋ธํ์ด์์ด ์์ผ๋ฉด ํ์คํธ ์์ ์ ์ ํธ๋์ญ์์ ์์ํ๊ณ , ํ์คํธ ์๋ฃ ํ์ ํญ์ ๋กค๋ฐฑํ๋ค.

์ด๋ ๊ฒ ํ๋ฉด DB์ ๋ฐ์ดํฐ๊ฐ ๋จ์ง ์์ผ๋ฏ๋ก ๋ค์ ํ์คํธ์ ์ํฅ์ ์ฃผ์ง ์์ ๊ณ์์ ์ธ ํ์คํธ๊ฐ ๊ฐ๋ฅํ๋ค.

โ ํ์ง๋ง ํ์คํธ์ `@Commit` ์ด๋ธํ์ด์์ด ์์ผ๋ฉด ๋กค๋ฐฑํ์ง ์๊ณ  ํ์คํธ ๋ฐ์ดํฐ๊ฐ DB์ ์ ์ฅ๋๋ค.

```java
@Test
@Commit
void join() {
    ...
}
```

**์คํ๋ง ์ปจํ์ด๋๋ฅผ ์ด์ฉํ ํตํฉ ํ์คํธ๋ณด๋ค ์๋ฐ๋ก ์ด๋ฃจ์ด์ง ๋จ์ ํ์คํธ๊ฐ ์ข์ ํ๋ฅ ์ด ๋๋ค!**

## JPA
JPA๋ ๊ธฐ์กด์ ๋ฐ๋ณต ์ฝ๋๋ ๋ฌผ๋ก ์ด๊ณ  ๊ธฐ๋ณธ์ ์ธ SQL๋ JPA๊ฐ ์ง์  ๋ง๋ค์ด์ ์คํํด์ค๋ค.

JPA๋ฅผ ์ฌ์ฉํ๋ฉด SQL๊ณผ ๋ฐ์ดํฐ ์ค์ฌ์ ์ค๊ณ์์ ๊ฐ์ฒด ์ค์ฌ์ ์ค๊ณ๋ก ํจ๋ฌ๋ค์์ ์ ๋ฌํ  ์ ์๋ค.

JPA๋ฅผ ์ฌ์ฉํ๋ฉด ๊ฐ๋ฐ ์์ฐ์ฑ์ ํฌ๊ฒ ๋์ผ ์ ์๋ค.

```java
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none
```

**spring.jpa.show-sql**

- JPA๊ฐ ์์ฑํ๋ SQL์ ์ถ๋ ฅํ๋ค.

**spring.jpa.hibernate.ddl-auto**

- JPA๋ ํ์ด๋ธ์ ์๋์ผ๋ก ์์ฑํ๋ ๊ธฐ๋ฅ์ ์ ๊ณตํ๋๋ฐ, `none`์ ์ฌ์ฉํ๋ฉด ํด๋น ๊ธฐ๋ฅ์ ๋๋ค.
- `create`์ ์ฌ์ฉํ๋ฉด entity ์ ๋ณด๋ฅผ ๋ฐํ์ผ๋ก ํ์ด๋ธ๋ ์ง์  ์์ฑํด์ค๋ค.

### @Transactional

```java
import org.springframework.transaction.annotation.Transactional

@Transactional
```

- ์คํ๋ง์ ํด๋น ํด๋์ค์ ๋ฉ์๋๋ฅผ ์คํํ  ๋ ํธ๋์ญ์์ ์์ํ๊ณ , ๋ฉ์๋๊ฐ ์ ์ ์ข๋ฃ๋๋ฉด ํธ๋์ญ์์ ์ปค๋ฐํ๋ค.
- ๋ฐํ์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ฉด ๋กค๋ฐฑํ๋ค.
- JPA๋ฅผ ํตํ ๋ชจ๋  ๋ฐ์ดํฐ ๋ณ๊ฒฝ์ ํธ๋์ญ์ ์์์ ์คํํด์ผ ํ๋ค.