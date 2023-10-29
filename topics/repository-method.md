# 레파지토리 queryDsl 메소드

- 메소드 명명규칙
    - 메소드 명명규칙은 spring-data-jpa 명명 규칙 기반으로 작성합니다.
    - 만일 복잡한 조인으로 인해 메소드명이 길고 불분명한 경우, 동사를 제외하고 [기본 명명 규칙 - 메소드](default-namespace.md#메소드 "기본 명명 규칙 #메소드")에 따라 작성합니다.
- 메소드 접근 제한자
    - `public`
- 메소드 파라미터
    - 메소드 파라미터 정의 방식은 spring-data-jpa 에서 파라미터를 정의하는 방식과 동일하게 작성합니다.
    - 만일 파라미터가 길고 복잡해지는 경우(파라미터 인자 개수 3개 이상), DTO 클래스를 생성하여 사용합니다.
- 메소드 반환 타입
    - 메소드 반환 타입은 spring-data-jpa 반환 타입 형식과 동일하게 작성합니다.
    - 만일 반환 타입이 복잡해지는 경우(엔티티 조인이 2개 이상), DTO 클래스를 생성하여 사용합니다.

```java
...
public class SampleRepositoryForDsl {
	...

	public Optional<SampleEntity> findSampleEntityByName(String name) throws Exception {
		...
		return ...;
    }

	public Page<SampleEntity> findSampleEntityAll(Pageable pageable) throws Exception {
		...
		return ...;
	}

	public Page<FindSampleDetailList.ResultDto> findSampleDetailList(Pageable pageable, 
                                                                     FindSampleDetailList.ParamDto paramDto
                                                                     ) throws Exception {
		...
		return ...;
	}
	...
}
```