---
aliases: []
sticker: lucide//share-2
---
- 상황: Spring이 `MemberRepository` 타입의 빈을 주입하려고 했는데, 같은 타입의 빈이 **2개**가 발견되어 어떤 것을 선택해야 할지 모름
- 원인: 전에 @Component 어노테이션 붙여서도 스프링 빈 등록할 수 있다는 예제 따라하고 안지워서 발생
- **같은 빈을 수동 등록(`@Bean`)과 자동 등록(`@Component`) 둘 다 하면서 충돌 발생** → 둘 중 하나만 써야한다