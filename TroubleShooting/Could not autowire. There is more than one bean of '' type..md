# 문제
- `SpringDataJpaMemberRepository` 생성 후, integration 테스트를 돌렸더니 fail이 떴다.
```java
@SpringBootTest  
@Transactional  
class MemberServiceIntegrationTest {  
  
  @Autowired  
  MemberService memberService;  
  @Autowired  
  MemberRepository memberRepository; // error!
	
  ...
}
```

- MemberRepository를 Autowire로 불러오는데 다음 에러 메시지가 보였다.
```java
Could not autowire. There is more than one bean of 'MemberRepository' type.

Beans:
jdbcTemplateMemberRepository   (JdbcTemplateMemberRepository. java) springDataJpaMemberRepository   (SpringDataJpaMemberRepository. java) 
```

- 이 에러 메시지는 SpringConfig에서 동일하게 관찰되었다.
```java
@Configuration  
public class SpringConfig {  
  
  private final MemberRepository memberRepository;

  public SpringConfig(MemberRepository memberRepository) {  // error!
    this.memberRepository = memberRepository;
  }

  ...
}
```
# 원인 
- `MemberRepository` 타입의 스프링 빈이 1개 이상 생성되어 autowire 할 수 없다.

```java
@Repository
public class JdbcTemplateMemberRepository implements MemberRepository { 
 ...
}
```
- `@Repository` 어노테이션을 제거한다.

# 과정
- `MemberRepository`는 `interface` → 스프링빈으로 등록되지 않는다.
	- 스프링 빈 등록 대상: `@Repository`, `@Component`, `@Service`, `@Configuration`
- 스프링 데이터 JPA는 `interface만` 정의해도 자동으로 구현체 만들어준다
	- 구현체? 인터페이스 또는 클래스를 상속(`implements`, `extends`)하여 구체적으로 만든 클래스
- 현재 `MemberRepository` 기반 구현체는 2개임
	- `jdbcTemplateMemberRepository`
	- `springDataJpaMemberRepository`
- 
# 참고