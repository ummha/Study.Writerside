# Service

기능 요구사항에 따라 비즈니스 로직을 처리하여 서비스를 구현합니다.

- 특징
    - 서비스 클래스는 같은 경로의 repository 패키지 내의 QueryDsl 용 레파지토리 클래스를 주입 받아 사용하거나 persistence 패키지 내의 JpaRepository용 레파지토리 인터페이스를 주입 받아서 사용합니다.
    - 서비스 클래스는 다른 서비스 클래스 또는 컨트롤러 클래스를 주입 받지 않습니다.
    - 서비스 클래스에서는 HttpServletRequest, HttpSession을 제어하지 않습니다.