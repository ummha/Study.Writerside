# 응답 스키마 클래스

- 클래스 위치
    - 컨트롤러 요청/응답 스키마 인터페이스 내부에 위치합니다.
- 클래스 명명 규칙
    - class Response
- 클래스 접근 제한자
    - 접근 제한자는 public static final 이지만 인터페이스와 롬복 어노테이션을 작성하면서 모두 생략합니다.
- 클래스 어노테이션
    - `@Getter`
        - 응답 스키마 클래스 필드 접근 메소드를 자동으로 작성합니다.
        - 응답 스키마 클래스에서는 Setter 메소드를 사용하지 않습니다.
    - `@AllArgsConstructor`
        - 응답 스키마 클래스의 전체 필드 초기화 생성자 메소드를 자동으로 생성합니다.
        - 응답 스키마 클래스에서는 빌더 패턴을 사용하지 않습니다.
    - `@Schema`
        - Swagger 문서 자동 완성 어노테이션을 작성합니다.
        - 속성
            - `name` = {인터페이스명}.Response (스키마 이름)
                - e.g. `name = "GetSample.Response"`
            - `description` = 스키마 설명
            - 그 외 속성은 작성하지 않습니다.
- 인터페이스 상속
    - `ResponseSchema`
        - 응답 스키마 클래스는 위의 인터페이스를 꼭 상속 받아야 합니다.
        - 상속 받지 않는다면 HttpResponse를 통해 응답 객체를 빌드할 수 없습니다.
- 필드 명명 규칙
    - camelCase
- 필드 접근 제한자
    - `private`
- 필드 어노테이션
    - `@Schema`
        - Swagger 문서 자동 완성 어노테이션을 작성합니다.
        - 속성
            - `description` = 필드 설명
            - 그 외 속성은 작성하지 않습니다.

```java
public interface GetSample {
	
	@Getter
	@AllArgsConstructor
	@Schema(name = "GetSample.Response",
					description = "샘플 조회 응답 스키마")
	class Response implements ResponseSchema {
		@Schema(description = "이름")
		private String name;
		...
	}
}
```