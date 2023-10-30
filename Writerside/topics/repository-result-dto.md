# 레파지토리 결과 DTO 클래스

- 클래스 위치
    - 레파지토리 DTO 인터페이스 내부에 위치합니다.
- 클래스 명명 규칙
    - class ResultDto
- 클래스 접근 제한자
    - 접근 제한자는 `public static` 이지만 인터페이스 내부에 클래스가 위치하므로 모두 생략합니다.
- 클래스 어노테이션
    - `@Data`
        - DTO 객체 특성을 모두 설정합니다.
- 생성자
    - 생성자는 모든 필드를 초기화하는 생성자를 작성합니다.
    - `@QueryProjection`
        - 생성자에 QueryDSL용 어노테이션을 작성합니다.
        - 결과를 반환할때 사용되는 생성자를 의미하며 해당 DTO 또한 Q 클래스로 빌드됩니다.
        - DTO로 결과를 매핑하는 방식은 여러가지가 있습니다. 그 중 컴파일 시점에 타입 세이프하게 개발하기 위해 QueryProjection을 사용합니다.
            - 그 외 방법
                - 프로퍼티 접근 - Setter : Projections.Bean()
                - 필드 직접 접근 : Projections.fields()
                - 생성자 사용 : Projections.constructor
                - Q클래스 사용 : @QueryProjection (택)
- 필드 명명 규칙
    - camelCase
- 필드 접근 제한자
    - `private`

```java
public interface FindSampleDetailList {
	
	@Data
	class ResultDto	{
		private String name;
		private String name2;
		...
		
		@QueryProjection
		public ResultDto(String name, String name2, ...) {
			this.name = name;
			this.name2 = name2;
			...
		}
	}
}
```

- 사용 예시

```java
...
public class SampleRepositoryForDsl {
	...

	public Page<FindSampleDetailList.ResultDto> findSampleDetailList(...) throws Exception {
		QSampleEntity qSample = QSampleEntity.sampleEntity;
		QSampleEntity2 qSample2 = QSampleEntity2.sampleEntity2;

		queryFactory.select(new QFindSampleDetilList_ResultDto(qSample.name,
                                                               qSample2.name,
                                                               ...
                                                               ))...

		return ...;
	}
	...
}
```

> **DTO QClass 컴파일 오류 발생 시 해결방법**
> 
> DTO QClass를 작성하고 사용시 Import 구문에 오류가 발생하거나 클래스 정보가 없어서 접근 할 수 없는 상황이 발생할 수 있습니다. QClass를 사용하기 위해서는 QueryDSL 태스크를 실행하여 직접 DTO클래스를 QClass로 컴파일 시켜야 사용할 수 있습니다.
> 
> \[인텔리제이\] 우측 패널에 그래들 메뉴를 클릭하여 노출되는 그래들 목록 중에서 최종 실행 모듈(SpringApplication 실행 클래스가 위치한 모듈)의 그래들에 접근합니다. 최종 실행 모듈의 Tasks 하위 목록의 other에 들어가있는 태스크 목록 중 compileQuerydsl 태스크를 실행합니다. (KW2 기준 : kw-boot-(local) > Tasks > other > compileQuerydsl )
> 
> 실행 후 작업하고자 하는 레파지토리 소스 코드 내에서 컴파일된 QClass 접근을 시도해 봅니다.
> 
> 그럼에도 불구하고 오류가 발생 시, 공통 개발자에게 오류 확인 요청을 합니다.
{style="warning"}

  

  

  

  
