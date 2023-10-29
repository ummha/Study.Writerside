# SessionHandler

SessionHandler 인터페이스는 HttpSession을 쉽게 제어하기 위해 만들어진 공통 인터페이스 입니다.

SessionHandler 인터페이스를 통해서 HttpSession을 가져오거나 세션에 객체를 저장하고 저장된 객체를 꺼내올 수 있도록 메소드를 제공합니다.

- 특징
    - SessionHandler 인터페이스에서 HttpSession에 접근하고자 할 때 SessionHandler 인터페이스에서 제공하는 세션 생성 메소드를 사용하지 않았다면 SessionNotFoundException 예외가 발생합니다.

# 세션 생성 및 조회하기

세션을 생성할 때는 SessionHandler 인터페이스의 `createSession()` 메소드를 사용합니다. 해당 메소드는 SessionHandler 인터페이스 자신을 반환하기 때문에 `getSession()` 메소드를 통해 HttpSession을 반환 받을 수 있습니다. 만일 기존의 생성된 세션을 무시하고 새로운 세션을 생성하고자 하는경우 `createSession(true)` 를 호출합니다.

```java
@RestController
@RequiredArgsConstructor
class SampleController {
	private final SessionHandler sessionHandler;

  ...
	void getSessionTest() {
		HttpSession session = sessionHandler.createSession().getSession();
		String sessionId1 = session.getId();
		
		session = sessionHandler.createSession().getSession();
		String sessionId2 = session.getId();

		session = sessionHandler.createSession(true).getSession();
		String sessionId3 = session.getId();

		System.out.println(sessionId1.equals(sessionId2)); // true
		System.out.println(sessionId1.equals(sessionId3)); // false
	}

}
```

## 세션 객체 만들기

큐더스웍스2.0 부터는 세션에 데이터를 저장하기 위해서는 세션 객체 모델링이 필요합니다.

세션 객체를 모델링할 때 아래와 같이 Record 타입의 SessionModel 인터페이스를 상속 받아서 모델링합니다.

```java
public record SampleSessionData(
	String data1,
	String data2,
	...
) implements SessionModel {
}
```

## 세션 모델 객체 저장 및 조회하기

세션 모델 객체를 저장하기 위해서는 SessionHandler 인터페이스의 `setSessionModel(SessionModel sessionModel)`을 사용하여 저장할 세션 모델 객체를 메소드 파라미터 인자로 전달합니다. 반대로, 저장한 세션 모델 객체를 조회할 때는 `getSessionModel(Class tClazz)` 를 사용합니다. 조회하고자 하는 세션 모델 객체가 없을 시, null을 반환합니다.

```java
@RestController
@RequiredArgsConstructor
class SampleController {
	private final SessionHandler sessionHandler;

  ...
	void setSessionModelTest() {
		sessionHandler.setSessionModel(new SampleSessionData("", "", ...));
	}

	void getSessionModelTest() {
		SampleSessionData sessionData = sessionHandler.getSessionModel(SampleSessionData.class);
	}

}
```