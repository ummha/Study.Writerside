# 컨트롤러 메소드

- 메소드 명명규칙
    - URI의 마지막 기능명을 메소드명으로 작성합니다.
        - e.g. uri = `/sample/getSampleList`
        - e.g. method = `… getSampleList(…)`
    - [기본 명명 규칙 - 메소드](default-namespace.md#method-name "기본 명명 규칙 #메소드")
- 메소드 접근 제한자
    - `public`
- 메소드 어노테이션
    - URI 매핑
        - `@PostMapping`
            - 기본적으로 모든 Http method는 POST 방식를 사용합니다.
            - PUT, DELET, PATCH 등 사용 안함
        - `@GetMapping`
            - SSR 구현 시 페이지 랜더링할 때 사용합니다.
    - `@Operation`
        - Swagger 문서 자동 완성 어노테이션을 작성합니다.
        - 속성
            - `summary` = API 간략 설명
            - `description` = API 상세 설명
            - 그 외 속성은 작성하지 않습니다.
- 메소드 파라미터
    - `@RequestBody`
        - 요청 바디의 정보를 매핑한 객체임을 명시합니다.
    - `@Valid`
        - 요청 스키마 클래스의 필드에 정의된 조건에 따라 유효성 검증을 진행합니다.
    - 파라미터명은 “request”로 작성합니다.
- 메소드 반환 타입
    - 컨트롤러 메소드 반환 타입은 4가지 중에서 하나를 사용합니다.
    - `DataResponse<T>`
        - 단건 데이터를 반환하고자 하는 경우 사용합니다.
    - `ListResponse<T>`
        - 다건 데이터를 반환하고자 하는 경우 사용합니다.
    - `PagingResponse<T>`
        - 페이징 처리된 다건 데이터를 반환하고자 하는 경우 사용합니다.
    - `ResultResponse`
        - 응답 코드만 반환하고자 하는 경우 사용합니다.
    - 반환 타입 정의 시 컴파일 오류가 발생한다면 응답 스키마 클래스가 `ResponseSchema` 인터페이스를 상속 받았는지 확인하고 없다면 상속을 받습니다.
- 응답 데이터 반환 시
    - `HttpResponse` 인터페이스의 `builder()` 메소드를 사용하여 응답 데이터를 반환합니다.
    - HttpResponse.builder().data(data).build() → DataResponse
    - HttpResponse.builder().list(list).build() → ListResponse
    - HttpResponse.builder().list(list).page(page).build() → PagingResponse
    - HttpResponse.builder().build(result) → ResultResponse

```java
...
class SampleController {
	...
    
	@PostMapping("/sample/getSample")
    @Operation(summary = "샘플 상세 조회 API",	
               description = "샘플 관리 화면에서 샘플 상세 정보를 조회하기 위한 API")
    DataResponse<GetSample.Response> getSample(@Valid
                                               @RequestBody 
                                               GetSample.Request request
                                               ) throws Exception {
        return HttpResponse.builder()
                           .data(service.getSample(request))
                           .build();
    }
	...
}
```