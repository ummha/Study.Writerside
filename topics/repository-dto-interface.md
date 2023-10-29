# 레파지토리 DTO 인터페이스

레파지토리 DTO 인터페이스는 각 레파지토리 메소드 별로 작성하며 내부에 결과와 파라미터 클래스를 작성합니다.

레파지토리 DTO 인터페이스의 DTO 클래스들은 복잡한 조회 쿼리와 파라미터가 필요한 경우 사용합니다.

- 인터페이스 위치
    - 레파지토리 패키지 내부에 dto 패키지 하위에 위치합니다.
    - e.g. com.irk.kw.sample.repository.dto.FindSampleDetailList.java
- 인터페이스 명명 규칙
    - {메소드명-PascalCase}.java
- 인터페이스 접근 제한자
    - `public`

```java
public interface FindSampleDetailList {
	...
}
```