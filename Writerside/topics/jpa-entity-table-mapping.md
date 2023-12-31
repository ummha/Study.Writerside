# 엔티티 클래스와 테이블 매핑

엔티티 클래스는 테이블 매핑에 필요한 어노테이션 뿐만아니라 롬복 어노테이션도 활용합니다.

- `@Getter` (필수)
    - 엔티티 클래스 필드의 값을 조회할 수 있는 Getter 메소드 자동 생성 어노테이션을 작성합니다.
- `@Setter` (필수)
    - 엔티티 클래스 필드의 값을 수정할 수 있는 Setter 메소드 자동 생성 어노테이션을 작성합니다.
- `@ToString` (필수)
    - 엔티티 클래스의 toString 메소드를 자동 생성하는 어노테이션을 작성합니다.
    - 주의사항
        - 엔티티 객체 접근 시, 불필요한 N+1 조회 쿼리가 발생하는 경우 toString 메소드를 먼저 의심해봅니다.
        - toString 메소드가 자동 생성될 때 연관된 객체도 기본적으로 toString 메소드 안에 포함되므로 주의가 필요합니다.
        - 따라서, 만일 연관된 객체가 존재할때 해당 연관관계 매핑 필드에 `@ToString.Exclude` 어노테이션을 작성해줍니다.
- `@NoArgsConstructor` (필수)
- `@AllArgsContructor` (필수)
- `@Entity` (필수)
    - 해당 클래스가 테이블과 매핑할 엔티티 클래스임을 정의합니다.
    - 엔티티명은 클래스명으로 기입하기 위해 어노테이션 속성으로 따로 정의하지 않습니다.
    - 속성
        - `name` = 엔티티명을 설정합니다. (설정하지 않을 시 기본적으로 클래스명으로 설정됩니다.)
- `@Table` (필수)
    - 어떤 테이블을 엔티티와 매핑할 것인지 설정합니다.
    - 속성
        - `name` = 매핑할 테이블명을 설정합니다.
- `@Comment` (필수)
    - 테이블의 주석을 작성합니다.
- `@EntityListeners` (선택)
    - 엔티티 리스너들을 등록합니다.
    - 주로 엔티티의 상태를 감시하기 위해서 사용됩니다.
    - `AuditingListener.class`
        - 모델링하고자 하는 엔티티에 대해서 등록자, 등록일을 기록하고자 하는 경우 등록합니다.
            - 추가 작업 - 상속 : `implements Auditing.Create`
        - 모델링하고자 하는 엔티티에 대해서 수정자, 수정일을 기록하고자 하는 경우 등록합니다.
            - 추가 작업 - 상속 : `implements Auditing.Update`