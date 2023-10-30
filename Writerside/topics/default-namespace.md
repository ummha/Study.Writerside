# 기본 명명 규칙

## 패키지명

- 패키지명은 모두 lower_snake_case 형태로 작성합니다.

## 클래스명

- 클래스명은 모두 PascalCase 형태로 작성합니다.

## 메소드명 {id="method-name"}

- 메소드명은 모두 camelCase 형태로 작성합니다.
- 메소드명 형식
    - 단건
        - `동사 + 명사`
            - e.g. getUserDetail
        - `동사 + 명사 + for + 대상`
            - 대상을 위해 명사를 동사한다.
            - e.g. getUserDetailForProfile
        - `동사 + 명사 of 대상`
            - 대상의 부분(명사)을 동사한다.
            - e.g. getUserOfCompany
        - `동사 + 명사 + 전치사 + 대상`
            - e.g. getUserWithCompany
    - 다건
        - `동사 + 명사 + List`
            - e.g. getUserList
        - `동사 + 명사 + List + for + 대상`
            - 대상을 위해 명사를 동사한다.
            - e.g. getUserListForSendingMail
        - `동사 + 명사 + List of 대상`
            - 대상의 부분(명사)을 동사한다.
            - e.g. getUserListOfCompany
        - `동사 + 명사 + List + 전치사 + 대상`
            - e.g. getUserListWithCompany
- 메소드명 동사 사용 규칙
    - `get + 명사`
        - 명사 가져오기
        - e.g. getUser
    - `get + 명사 + count`
        - 명사 개수 가져오기
        - e.g. getUserCount
    - `update + 명사`
        - 명사 수정하기
        - e.g. updateUser
    - `create + 명사`
        - 명사 생성하기
        - e.g. createUser
    - `delete + 명사`
        - 명사 삭제하기
        - e.g. deleteUser
    - `valid + 명사`
        - 명사 데이터 유효성 검사하기
        - e.g. validUser
    - `check + 명사`
        - 명사(행위)를 할수 있는지 검사
        - e.g. checkUserCreationAvailable
    - `is + 명사`
        - 명사 인지 판단
        - e.g. isAdmin
    - `has + 명사`
        - 명사 포함 여부 판단
        - e.g. hasRole
    - `set + 명사`
        - 명사(config / 기능 등) 설정
        - e.g. setUsername


### 변수명

- 변수명은 모두 camelCase 형태로 작성합니다.
    - 자료구조 타입의 변수명 규칙
        - `명사 + List`
            - List, ArrayList, LinkedList, …
            - e.g. userList
        - `명사 + Map`
            - Map, HashMap, TreeMap, …
            - e.g. paramMap
        - `명사 + Set`
            - Set, HashSet, TreeSet, …
            - e.g. userSet
- 상수 변수는 아래 두 가지 상황에 따라 UPPER_SNAKE_CASE 또는 camelCase 형태로 작성합니다.
    - UPPER_SNAKE_CASE
        - 모든 final 변수가 초기 선언 및 초기화가 이루어진 변수라면 UPPER_SNAKE_CASE로 작성합니다.

        ```java
        public UpperSnakeCaseForConstant {
        	static final double NUMBER = 0.123;
        	static final String STR_LITERAL = "hello world";
        	static final Object OBJ = new Object();
        
        	public void sample(String personName) {
        		final String NAME = "NAME";
        		final String PERSON_NAME = personName + "-" + NAME;
        		final double RANDOM = Math.random();
        	}
        }
        ```

    - camelCase
        - 선언 및 초기화가 이루어지지 않았다면 camelCase로 작성합니다.

        ```java
        public CamelCaseForConstant {
        	final double number;
        	final String str;
        	final Object obj;
        
        	public CamelCaseForConstant(double number, String str, Object obj) {
        		...
        	}
        
        	public void sample(final String personName) {
        		...
        	}
        }
        ```