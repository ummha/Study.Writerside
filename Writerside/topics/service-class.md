# 서비스 클래스

- 클래스 위치
    - 프로그램 패키지 하위에 위치합니다.
    - e.g. com.irk.kw.sample.SampleService.java
- 클래스 명명 규칙
    - class {프로그램명-PascalCase}Service
- 클래스 접근 제한자
    - `default`
- 클래스 어노테이션
    - `@Service`
        - 클래스가 서비스 구현 클래스임을 명시하며 빈으로 등록합니다.
    - `@RequiredArgsConstructor`
        - 빈 주입은 생성자 주입 방식을 사용합니다.

```java
@Service
@RequiredArgsConstructor
class SampleService {
	private final SampleRepositoryForDsl sampleRepositoryForDsl;
	...
}
```