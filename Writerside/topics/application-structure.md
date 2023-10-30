# KW 애플리케이션 모듈 내부 구조

큐더스웍스2.0 프로젝트에서는 애플리케이션 구조가 예전과 많이 다르진 않지만 Map 사용을 최소화 하면서 발생되는 이슈와 개발 관점과 사용 범위에 따라 구조가 조금씩 달라졌습니다.

## 애플리케이션 모듈 내부 패키지 구조

```
com.irk.kw (base-package)
|
├ persistence
|  ├ repository
|  |  ├ dto
|  |  |  ㄴ ...
|  |  ㄴ ...
|  ├ ...
├ ...
|
|
├ {프로그램:대분류}
|  ├ {기능:중분류}
|  |  ├ repository
|  |  |  ├ dto
|  |  |  |  ㄴ ...
|  |  |  ㄴ ...
|  |  ├ schema
|  |  |  ㄴ ...
|  |  ㄴ ...
|  |
|
├ ...
|
```

## 프로그램 패키지 구조 상세 (e.g. 사용자 관리)

(예시일뿐, 실제 개발할때 명칭은 다를 수 있습니다.)

- com.irk.kw
    - persistence.repository
        - dto
            - FindUserDetail
            - …
        - UserInfoRepository
    - user - 프로그램:사용자 관리 패키지
        - profile - 프로필 관리 패키지
            - repository - 레파지토리 패키지
                - dto
                    - FindProfileDetail
                    - …
                - ProfileRepository
            - schema - HTTP 통신 API 스키마 패키지
                - GetProfile
                - UpdateProfile
                - …
            - ProfileController
            - ProfileService
        - employee - 담당자(직원) 관리 기능
            - repository - 레파지토리 패키지
                - dto
                    - FindEmployeeDetail
                    - …
                - EmployeeRepository
            - schema - HTTP 통신 API 스키마 패키지
                - GetEmployee
                - GetEmployeeList
                - …
            - EmployeeController
            - EmployeeService