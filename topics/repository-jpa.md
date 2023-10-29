# 공통 레파지토리

JPA 엔티티 기준 공통적으로 필요한 메소드를 관리합니다.

- 특징
    - JPQL을 spring-data-jpa 기반으로 작성합니다.
    - 모든 프로그램에서 사용할 수 있는 레파지토리만의 공통 기능을 제공할 수 있습니다.
    - JPA 엔티티와 1:1 관계를 갖습니다.
        - e.g. SampleEntity - SampleEntityRepository

> 만약 spring-data-jpa로 작성하기 어려운 쿼리가 필요하여 QueryDSL 기반으로 메소드를 제공해야할 시, 공통 개발자에게 JpaRepository Support Class 생성 요청 바랍니다.
{style="note"}