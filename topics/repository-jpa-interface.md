# JpaRepository 인터페이스

- 인터페이스 위치
    - 영속성 패키지 하위의 repository 패키지 하위에 위치합니다.
    - e.g. com.irk.kw.persistence.repository.SampleEntityRepository.java
- 인터페이스 명명 규칙
    - interface {JPA엔티티명}Repository
- 인터페이스 접근 제한자
    - `public`
- 인터페이스 상속
    - `JpaRepository<T, ID>`
        - Spring data jpa 에서 제공하는 인터페이스를 상속 받습니다.
        - T = 엔티티 타입
        - ID = 엔티티 식별자(고유 아이디/필드) 타입
    - 필요 시, Support 인터페이스를 상속 받을 수 있습니다.

```java
public interface SampleEntityRepository extends JpaRepository<SampleEntity, Long> {
	...
}
```