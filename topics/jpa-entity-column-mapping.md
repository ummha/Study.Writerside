# 엔티티 클래스 필드와 테이블 컬럼 매핑

[**⬆️ 개요 보기 ⬆️**](https://www.notion.so/KW2-0-626ff44cedbb4795b5672ec12f4b77ba?pvs=21)

필드는 `@Column`과 `@Comment`를 반드시 포함하여 작성합니다. 그리고 모든 필드는 `private` 접근 제한자를 가집니다.

```java
@Size(max = 20)
@Column(name = "SAMPLE_CODE", columnDefinition = "VARCHAR2(20)")
@Comment("샘플 코드")
private String sampleCode;

@NotNull
@Size(max = 100)
@Column(name = "EMAIL", columnDefinition = "NVARCHAR2(100)", unique = true)
@Comment("이메일")
private String email;

@Embedded
private CreateAudit createAudit;

@ColumnDefault("1")
@Convert(converter = BooleanConverter.class)
@Column(name = "IS_USED", columnDefinition = "NUMBER(1,0)")
@Comment("사용 여부")
private boolean isUsed = true;
```

- 필드 타입
    - LocalDateTime
        - 컬럼명이 ‘DATE’로 끝나는 경우
    - LocalDate
        - 컬럼명이 ‘DAY’로 끝나는 경우
    - primitive type
        - Null을 허용하지 않는 경우
    - reference type / wrapper type(Long, Integer, …)
        - Null을 허용하는 경우
- `@Column` (필수)
    - 매핑할 컬럼 정보를 입력하여 DDL 자동 생성에 반영될 수 있도록 작성합니다.
    - 속성
        - `name` = 컬럼명을 작성합니다.
        - `columnDefinition` = 컬럼 타입을 작성합니다. (타입 길이와 같이 작성)
            - 시퀀스로 제약 조건과 이를 외래키로 같는 컬럼의 데이터 타입은 `NUMBER(19,0)`으로 통일합니다. (오라클 밴더의 기본 설정이 NUMBER(19,0)으로 되어있음)
            - 날짜 타입인 경우 모두 `DATE`로 작성합니다.
        - `unique` = UNIQUE 제약조건이 필요한 경우 작성합니다.
        - `nullable` = NULL 여부, 이번 프로젝트에서는 작성하지 않습니다.
- `@Comment` (필수)
    - 컬럼의 설명을 작성합니다. 따라서, 필드 별로 javadoc 주석이 필요없습니다.
- `@ColumnDefault` (선택)
    - 컬럼의 기본 값이 필요할 때 작성합니다.
    - 보통 NUMBER(1) 타입의 1과 0의 값 또는 CHECK 제약조건이 걸린 경우 사용됩니다.
    - 속성
        - `value` = 기본 값
    - 주의 사항
        - 해당 어노테이션은 객체에 반영되지 않습니다. 따라서 객체의 기본값은 초기화 선언으로 해결해야합니다.
- `@Embedded` (선택)
    - 필드가 값 타입인 경우 작성합니다.
    - 주로 `CreateAudit` 또는 `UpdateAudit` 타입의 필드를 작성할 때 사용됩니다.
- `@Size` (선택)
    - 데이터 타입 사이즈에 맞게 설정합니다.
    - 스프링 빈 유효성 패키지 어노테이션으로 SQL 쿼리가 보내지기 전에 유효성 검증을 할 수 있습니다.
    - 속성
        - `max` = 최대값 (보통 max만 사용)
        - `min` = 최소값
- `@NotNull`
    - NOT NULL 조건이 있는 경우 설정합니다.
    - [스프링 빈 유효성 패키지 어노테이션](https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#validator-defineconstraints-spec)으로 SQL쿼리가 보내지기 전에 유효성 검증을 할 수 있습니다.
    - 하이버네이트에서 해당 어노테이션을 지원하기 때문에 DDL 자동 생성 시 NOT NULL 조건이 추가 됩니다.
- `@Convert`
    - 컨버터(변환 객체)를 등록할 때 사용합니다.
    - 주로 boolean 타입이나 열거형 타입을 사용할 때 사용됩니다.
    - 속성
        - `converter` = 컨버터 클래스를 설정합니다.