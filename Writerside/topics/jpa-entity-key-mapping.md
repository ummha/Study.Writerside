# 엔티티 클래스 아이디와 테이블 PK 매핑

## 기본키 매핑 - Auto Increment

기본키 작성 양식

```java
...
@Entity
public class SampleManagement {

	@Id
	@GeneratedValue(strategy = GenerationType.SEQUENCE, 
					generator = "SampleManagementSeq")
	@SequenceGenerator(name = "SampleManagementSeq"
					   sequenceName = "SAMPLE_MANAGEMENT_SEQ"
					   allocationSize = 1)
    @Column(name = "ID")
    @Comment("샘플 관리 ID")
    private Long id;
	
	...
}
```

- `@Id` (필수)
    - 해당 어노테이션이 작성된 필드가 엔티티 아이디임을 정의합니다.
- `@GeneratedValue` (필수)
    - 엔티티 아이디는 속성에 정의된 조건에 따라 값이 할당될 수 있도록 설정합니다.
    - 속성
        - `strategy` = 할당 전략을 설정합니다. (`GenerationType.SEQUNCE` 사용)
        - `generator` = SequenceGenerator 명을 설정합니다.
- `@SequenceGenerator` (필수)
    - SequenceGenerator를 생성합니다.
    - 속성
        - `name` = JPA SequenceGenerator 이름을 설정합니다.
            - 설정된 이름은 `@GeneratorValue` 에서 사용됩니다.
            - 시퀀스 명의 PascalCase 형식으로 작성합니다.
        - `sequenceName` = 데이터베이스의 시퀀스 오브젝트 이름을 작성합니다.
            - 시퀀스 명명 규칙은 테이블명 끝에 ‘_SEQ’ 를 붙여 사용합니다.
        - `allocationSize` = 증가값을 설정합니다. (기본 : 1)
            - 만일, 대용량 일괄 처리가 자주 발생하는 엔티티는 증가값을 수정할 수 있습니다.

## 기본키 매핑 - NOT Auto Increment

기본키 작성 양식

```java
...
@Entity
public class SampleManagement {

	@Id
    @Column(name = "ID")
	@Comment("샘플 관리 ID")
	private String id;
	
	...
}
```

- `@Id` (필수)
    - 해당 어노테이션이 작성된 필드가 엔티티 아이디임을 정의합니다.

## 복합키 매핑

복합키를 매핑할 때 사용할 수 있는 방법이 두 가지가 있습니다. 하나는 `@IdClass` 를 활용하는 방법이고 또 하나는 `@EmbeddedId` 를 활용하는 겁니다. 이번 프로젝트에서는 `@EmbeddedId` 방식으로만 통일하여 복합키를 매핑할 수 있도록 합니다.

복합키 작성 양식 (Embeddable)

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
@Embeddable
public class SampleManagementId implements Serializable {

    @Column(name = "SAMPLE_CODE", columnDefinition = "VARCHAR2(20)")
    @Comment("샘플 코드")
    private String sampleCode;

    @Column(name = "SAMPLE_DETAIL_CODE", columnDefinition = "VARCHAR2(50)")
    @Comment("샘플 상세 코드")
    private String sampleDetailCode;
}
```

- `@Getter` (필수)
- `@Setter` (필수)
- `@AllArgsConstructor` (필수)
- `@NoArgsConstructor` (필수)
- `@EqualsAndHashCode` (필수)
    - 영속성 컨텍스트 내에서 엔티티 아이디 값을 비교할 수 있도록 equals()와 hashCode()를 작성합니다.
- `@Embeddable` (필수)
    - 값 타입 클래스임을 정의합니다.
- `implements Serializable` (필수)
    - 복합키 클래스가 직렬화가 될 수 있도록 Serializable 인터페이스를 상속 받도록 합니다.

복합키 설정 양식

```java
...
@Entity
public class SampleManagement implements Auditing.Create, 
										 Persistable<SampleManagementId> {

	@EmbeddedId
	private SampleManagementId id;

	...

	@Embedded
	private CreateAudit createAudit;

	...

	@Override
	public boolean isNew() {
		return isNewByCreateAudit();
	}
}
```

- `@EmbeddedId` (필수)
    - 값 타입을 엔티티의 아이디로 설정합니다.
- `Persistable<T>` (필수)
    - Spring data jpa 에서 영속 방식을 재구현하기 위해 상속받습니다.
  - JpaRepository에서 복합키로 구성된 엔티티를 새로 영속(= insert) 시키고자 할 때 바로 Insert 쿼리가 만들어져서 보내지지 않고 select 쿼리를 한 번 보내고 난 후 insert 쿼리가 보내지는 것을 방지하기 위해 사용됩니다.
    - `isNew` 메소드 정의
        - 필요한 값 타입 = `CreateAudit` (엔티티 생성 감사용 값 타입 활용)
        - 엔티티의 등록일 필드를 활용하여 신규 엔티티 여부를 판단할 수 있도록 합니다.
        - 등록일 없음 = 신규, 등록일 있음 = 신규 아님
        - `Auditing.Create` 인터페이스의 `isNewByCreateAudit()` 메소드 사용
    - 만일 테이블이 복합키로 이루어져 있는데 데이터 생성일 또는 등록일 등 데이터 생성 감사용 데이터 컬럼이 없다면 공통 개발자에게 노티 부탁드립니다.


- JpaRepository 구현체 확인해보기 - SimpleJpaRepository.java {collapsible="true"}
    ```java
    @Transactional
    @Override
    public <S extends T> S save(S entity) {
    
      Assert.notNull(entity, "Entity must not be null");
    
      if (entityInformation.isNew(entity)) {
          em.persist(entity);
          return entity;
      } else {
          return em.merge(entity);
      }
    }
    ``` 
    > JpaRepository 구현체들 중 하나인 SimpleJpaRepository의 save() 메소드가 정의된 구문을 확인해 보면 엔티티가 영속이 되기 전에 isNew() 메소드로 엔티티가 신규인지 아닌지 확인하는 구문이 있습니다.
    > 
    > 기본키 전략에서는 엔티티의 아이디 값이 없으면 항상 새로운 엔티티로 판별하기 때문에 merge()가 발생되지 않는 것입니다. 하지만 복합키를 사용 시, 개발자는 데이터를 신규 추가할 때 복합키를 항상 생성하여 영속시키고자 하기 때문에 spring data jpa에서는 엔티티 아이디가 Null이지 않아 persist()가 실행되지 않고 merge()가 실행됩니다.
    >   
    > 따라서, 복합키를 사용할 때는 이 구문을 통과시키기 위해 isNew() 메소드를 오버라이딩하여 재정의가 필요합니다. 
    
