# 요청 스키마 클래스

- 클래스 위치
    - 컨트롤러 요청/응답 스키마 인터페이스 내부에 위치합니다.
- 클래스 명명 규칙
    - class Request
- 클래스 접근 제한자
    - 접근 제한자는 `public static` 이지만 인터페이스 내부에 클래스가 위치하므로 모두 생략합니다.
- 클래스 어노테이션
    - `@Getter`
        - 요청 스키마 클래스 필드 접근 메소드를 자동으로 작성합니다.
        - 요청 스키마 클래스에서는 Setter 메소드를 사용하지 않습니다.
    - `@NoArgsConstructor`
        - 요청 스키마 클래스의 기본 생성자 메소드를 자동으로 생성합니다.
    - `@Schema`
        - Swagger 문서 자동 완성 어노테이션을 작성합니다.
        - 속성
            - `name` = {인터페이스명}.Request (스키마 이름)
                - e.g. `name = "GetSample.Request"`
            - `description` = 스키마 설명
            - 그 외 속성은 작성하지 않습니다.
- 클래스 상속
    - 만일 페이징 처리를 위한 요청 스키마를 작성할 시 페이징 요청 클래스인 `PagingRequest` 클래스를 상속 받습니다.
    - `@EqualsAndHashCode(callSuper = false)`
        - 페이징 요청 클래스를 상속 받을 시 사용합니다.
        - 위 어노테이션 작성을 통해 동등성(equals)과 동일성(hashCode)을 비교하는 메소드를 자동 생성하고 callSuper 속성을 false로 설정하여 부모 클래스 필드 값들은 동일성 체크를 하지 않도록 합니다.
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
    - Spring Validation 어노테이션 사용
        - 유효성 검증을 위한 어노테이션을 작성합니다.

```java
public interface GetSample {
	
	@Getter
	@NoArgsConstructor
	@ToString
	@Schema(name = "GetSample.Request",
					description = "샘플 조회 요청 스키마")
	class Requset {
		@NotBlank
		@Schema(description = "이름")
		private String name;
		...
	}
}

public interface GetSampleList {
	
	@Getter
	@NoArgsConstructor
	@ToString
	@EqualsAndHashCode(callSuper = false)
	@Schema(name = "GetSampleList.Request",
					description = "샘플 리스트 조회 요청 스키마")
	class Requset extends PagingRequest {
		@NotBlank
		@Schema(description = "이름")
		private String name;
		...
	}
}
```