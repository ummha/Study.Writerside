# 레파지토리 파라미터 DTO 클래스

- 클래스 위치
    - 레파지토리 DTO 인터페이스 내부에 위치합니다.
- 클래스 명명 규칙
    - class ParamDto
- 클래스 접근 제한자
    - 접근 제한자는 `public static` 이지만 인터페이스 내부에 클래스가 위치하므로 모두 생략합니다.
- 클래스 어노테이션
    - `@Data`
        - DTO 객체 특성을 모두 설정합니다.
    - `@NoArgsConstructor`
    - `@AllArgsConstructor`
        - 모든 필드를 초기화하는 생성자를 자동 생성합니다.
- 필드 명명 규칙
    - camelCase
- 필드 접근 제한자
    - `private`

```java
public interface FindSampleDetailList {
	
	@Data
	@AllArgsConstructor
	class ParamDto	{
		private String name;
		...
	}
}
```