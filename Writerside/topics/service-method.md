# 서비스 메소드

- 메소드 명명규칙
    - 메소드 명명규칙은 컨트롤러 메소드와 동일합니다.
    - [기본 명명 규칙 - 메소드](default-namespace.md#method-name "기본 명명 규칙 #메소드")
- 메소드 접근 제한자
    - `public`
- 메소드 어노테이션
    - `@Transactional`
        - 트랜잭션을 제어할때 사용합니다.
        - 속성
            - `rollbackFor` = 어떤 예외가 발생했을 때 롤백을 시킬건지 작성합니다.
                - Spring 프레임워크에서는 기본적으로 RuntimeException이 발생할 때 롤백이 처리됩니다.
                - 따라서, 공통 예외 클래스로 감지하기 위해서는 `Exception.class` 로 설정합니다.
            - 그 외 속성은 서비스 메소드 성향에 따라 추가 작성합니다.
- 메소드 파라미터
    - 파라미터 타입은 해당 서비스 메소드를 호출하는 컨트롤러 메소드의 요청 스키마 클래스 타입으로 작성합니다.
    - 파라미터명은 “request”로 작성합니다.
    - 만일 세션 정보와 같이 추가적인 입력 데이터가 필요하다면 파라미터를 추가 작성합니다.
- 메소드 반환 타입
    - 메소드 반환 타입은 해당 서비스 메소드를 호출하는 컨트롤러 메소드의 응답 스키마 클래스 타입을 작성합니다.
    - `PagingResult<T>`
        - 만일 페이징 정보와 함께 컬렉션 객체를 반환하고자 하는 경우 사용합니다.
        - 제네릭 타입은 응답 스키마 클래스를 작성합니다.

```java
...
class SampleService {
	...
	
    public GetSample.Response getSample(GetSample.Request request,
                                        LoginUser loginUser
										) throws Exception {
		...
		return ...;
    }

	public PagingResult<GetSampleList.Response> getSampleList(GetSampleList.Request request) throws Exception {
		...
		return new PagingResult(...);
	}

	...
}
```