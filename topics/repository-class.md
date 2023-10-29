# 레파지토리 queryDsl 클래스

- 클래스 위치
    - 프로그램 패키지 하위의 repository 패키지 하위에 위치합니다.
    - e.g. com.irk.kw.sample.repository.SampleRepositoryForDsl.java
- 클래스 명명 규칙
    - class {프로그램명-PascalCase}RepositoryForDsl
- 클래스 접근 제한자
    - `public`
- 클래스 어노테이션
    - `@Repository`
        - 클래스가 데이터베이스 데이터를 제어하는 레파지토리 클래스임을 명시하며 빈으로 등록합니다.
    - `@RequiredArgsConstructor`
        - 빈 주입은 생성자 주입 방식을 사용합니다.
- 클래스 필드
    - `JPAQueryFactory queryFactory`
        - JPQL을 동적 생성하기 위한 팩토리 빈을 주입 받아 사용합니다.

```java
@Repository
@RequiredArgsConstructor
public class SampleRepositoryForDsl {
	private final JPAQueryFactory queryFactory;
	...
}
```