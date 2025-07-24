---
tags:
  - error
sticker: lucide//check-square
---
# 문제
```java
public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {  
  this.policyMap = policyMap;  
  this.policies = policies;  
  
  System.out.println("policyMap = " + policyMap);  
  System.out.println("policies = " + policies);  
}
```
- 출력했을 때 전부 빈 값으로 나옴

# 원인
1. 빈 등록 안됨: @Component로 빈 자동 등록
2. 잘못된 빈 이름 호출: 클래스명의 첫 글자를 소문자로 바꿔서 빈 이름을 만든다