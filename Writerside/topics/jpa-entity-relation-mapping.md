# 엔티티 연관관계 매핑

연관관계 매핑할때 모든 참조 객체에 @ToString.Exclude 어노테이션을 작성합니다. 또한 Fetch(로딩) 방식은 모두 LAZY(지연) 방식으로 통일합니다. 기본적으로 LAZY 방식을 사용하다가 추가적으로 필요할 때 EAGER(즉시) 방식을 사용하도록 합니다.

또한 모든 연관관계는 양방향 연관관계를 전제로 매핑합니다.

## OneToMany

```java
...
@Entity
public class SampleManagementGroup {
	...

	@Id
	@Column(name = "ID", ...)
	...
	private Long id;

	...

	@ToString.Exclude
	@OneToMany(mappedBy = "sampleManagementGroup", fetch = FetchType.LAZY)
	private Set<SampleManagement> sampleManagementSet = new LinkedHashSet<>();
}
```

- `@OneToMany` (필수)
    - 일대다 연관관계 매핑할 때 사용합니다.
    - 주로 반대쪽(Many) 테이블에 외래키가 존재하는 경우 사용됩니다.
    - 속성
        - `mappedBy` = 반대쪽(Many)의 엔티티에서 자신(One) 엔티티 타입을 참조하고 있는 필드명을 작성합니다.
        - `fetch` = Fetch 방식을 설정합니다.
- 컬렉션 타입
    - 컬렉션 타입은 Set 또는 List를 사용합니다.
    - Set, LinkedHashSet
        - 주로 중복이 없어야 할 때 사용합니다. 일반적으로 Set을 사용합니다.
    - List, ArrayList
        - 주로 중복을 허용할 때 사용합니다. HISTROY 분류의 엔티티는 List를 사용합니다.

## ManyToOne

```java
...
@Entity
public class SampleManagement {
	...

	@Column(name = "GROUP_ID", ...)
	...
	private Long groupId;
	
	...

	@ToString.Exclude
	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "GROUP_ID", 
							referencedColumnName = "ID", 
							insertable = false, 
							updatable = false)
	private SampleManagementGroup sampleManagementGroup;
}
```

- `@ManyToOne` (필수)
    - 다대일 연관관계 매핑할 때 사용합니다.
    - 주로 엔티티에 외래키 컬럼과 매핑한 필드가 존재하는 경우 사용됩니다.
    - 속성
        - `fetch` = Fetch 방식을 설정합니다.
        - `optional` = 엔티티가 항상 존재해야하는지의 여부를 설정합니다. (기본: true)
            - false로 설정시 엔티티는 항상 존재해야합니다.
            - 따라서 필요에 따라 설정이 필요합니다.
            - EAGER 방식일 때 false = 내부 조인, true = 외부 조인
- `@JoinColumn` (필수)
    - 조인할 컬럼을 설정할 때 사용합니다.
    - 속성
        - `name` = 조인될 컬럼명을 설정합니다. (외래키)
        - `referencedColumnName` = 조인할 반대쪽(Many) 컬럼명을 설정합니다.
        - `insertable` = 엔티티 생성 시 해당 컬럼도 같이 저장할 것인지 설정합니다.
            - false로 설정하면 데이터베이스에 저장되지 않습니다.
            - 즉, 읽기 전용으로 사용할 때 사용합니다.
        - `updatable` = 엔티티 수정 시 해당 컬럼도 같이 저장할 것인지 설정합니다.
            - false로 설정하면 데이터베이스에 수정하지 않습니다.
            - 즉, 읽기 전용으로 사용힐 떼 사용합니다.

기본적으로 연관관계를 매핑할 때 지연 로딩과 읽기 전용으로 연관관계를 설정하도록 했습니다. 실제로 엔티티를 저장하거나 제어할때 JPA에서 많은 작업이 이루어지면서 우리가 예측하지 못한 오류가 발생합니다. 따라서 초기에만 가이드대로 구성하고 후에 개선이 필요하면 설정 옵션들을 수정할 수 있도록 합니다.