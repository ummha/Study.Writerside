# 컨트롤러 클래스

- 클래스 위치
    - 프로그램 패키지 하위에 위치합니다.
    - e.g. com.irk.kw.sample.SampleController.java
- 클래스 명명 규칙
    - class {프로그램명-PascalCase}Controller
- 클래스 접근 제한자
    - `default`
- 클래스 어노테이션
    - `@RestController`
        - REST API 컨트롤러임을 명시합니다.
        - text/html 응답 시 `@Controller` 사용합니다.
    - `@RequiredArgsConstructor`
        - 빈 주입은 생성자 주입 방식을 사용합니다.
    - `@Tag`
        - Swagger 문서 자동 완성 어노테이션을 작성합니다.
        - 속성
            - `name` = 프로그램명 (API 그룹 명칭)
            - 그 외 속성은 작성하지 않습니다.

```java
@Tag(name = "샘플")
@RestController
@RequiredArgsConstructor
class SampleController {
	private finSampleService sampleService;
	...
}
```