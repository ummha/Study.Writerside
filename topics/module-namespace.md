# 모듈 명명 규칙

- 모듈명의 구분자는 붙임표(=대시, =dash, ='-')를 사용합니다. (규약)
- 모듈 디렉토리 구분자는 쌍점(=콜론, =colon, =':')을 사용합니다. (문법)
- 모듈명은 모두 소문자를 사용합니다. (규약)

애플리케이션 모듈 명명 규칙 :

- {애플리케이션명}-application
    - e.g. kw-application
- {애플리케이션명}-application-boot-{실행 분류}
    - e.g. kw-boot
    - e.g. kw-boot-test

공통 모듈 명명 규칙 :

- {회사명}-common-{공통 분류}
    - e.g. irk-common
    - e.g. irk-common-legacy

설정 모듈 명명 규칙 :

- {회사명}-config:config-{설정 분류}
    - e.g. irk-config:config-data-source
    - e.g. irk-config:config-swagger

스키마 모듈 명명 규칙:

- schema:schema-{스키마명}
    - e.g. schema:schema-common
    - e.g. schema:schema-dividend
    - e.g. schema:schema-kw