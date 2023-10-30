# 요청/응답 스키마 인터페이스

요청 스키마 인터페이스는 각 컨트롤러 메소드 별로 작성하며 내부에 요청과 응답 스키마 클래스를 작성합니다.

- 인터페이스 위치
    - 프로그램 패키지 내부에 schema 패키지 하위에 위치합니다.
    - e.g. com.irk.kw.sample.schema.GetSample.java
- 인터페이스 명명 규칙
    - interface {메소드명-PascalCase}
- 인터페이스 접근 제한자
    - `public`

```java
public interface GetSample {
	...
}
```