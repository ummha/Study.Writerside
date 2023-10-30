# Controller

클라이언트의 요청과 응답을 처리하는 클래스이며 세션(HttpSession)과 요청(HttpServletRequest)을 제어합니다.

- 특징
    - 컨트롤러 클래스는 하나의 서비스 구현체만 주입 받아서 사용합니다.
    - 컨트롤러 클래스는 다른 컨트롤러 클래스 또는 레파지토리 클래스를 주입 받지 않습니다.
    - 컨트롤러 클래스에서만 HttpServletRequest, HttpSession을 제어합니다.