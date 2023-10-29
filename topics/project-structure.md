# 프로젝트 구조

큐더스웍스2.0 개발 프로젝트는 **싱글 프로젝트 멀티 모듈** 기반으로 구성하여 개발합니다.

전반적인 프로젝트를 이해하기 위해서 각 구조별로 특징과 구분 방식에 대한 이해가 필요합니다.

```
kw2_be
├ buildSrc
├ gradle
├ {companyName}-common          [module]
├ {companyName}-config
| ├ config-{configName}         [module]
| ├ config-...                  [module]
├ {applicationName}-application [module]
├ {applicationName}-boot        [module]
├ {applicationName}-boot-local  [module]
├ persistence
| ├ orm-core                    [module]
| ├ orm-data                    [module]
| ├ schema-{schemaName}         [module]
| ├ schema-...                  [module]
|
├ ...
ㄴ settings.gradle
```

## buildSrc

`buildSrc` 디렉토리는 **그래들 컨벤션 스크립트 파일들을 관리**하기 위해 생성된 구조입니다.

`buildSrc` 디렉토리는 그래들 빌드 시 그래들에서 기본적으로 바라보는 **그래들의 특별한 디렉토리**입니다. 따라서, 개발자 입맛에 맞게 해당 디렉토리명을 바꾸지 않아야 합니다.

모듈 별로 중복되는 디펜던시 설정을 최소화 하고 모듈 성향에 따라 설정을 다르게 하기 위해서 그래들 빌드 스크립트를 여러개로 분리할 필요가 있습니다. 따라서, 분리된 스크립트를 한 곳에서 관리하기
위해 `buildSrc` 디렉토리 하위에 그래들 스크립트 파일들을 생성합니다. 이러한 그래들 파일들을 내부에서는 빌드 컨벤션(규약) 그래들 파일로 칭하겠습니다.

- 컨벤션 파일 명명 규칙
    - {분류}.java-conventions.gradle
    - e.g. base.java-conventions.gradle
    - e.g. querydsl.java-conventions.gradle

모듈 별로 빌드 컨벤션을 사용하기 위해서는 사용하고자 하는 컨벤션 파일의 확장자명을 제외하고 모듈의 별도 빌드 스크립트 파일에 아래와 같이 작성합니다.

- build.gradle

```groovy
plugins {
	id 'base.java-conventions'
}
```
{collapsible="true" collapsed-title="build.gradle"}

## gradle

`gradle` 디렉토리는 gradle wrapper 설정을 관리하는 디렉토리 입니다. 따라서, VCS(버전 관리 시스템) 사용할 때 `gradle` 디렉토리가 제외되지 않도록 주의합니다. 해당 디렉토리로 인해서 **모든 참여 개발자가 별도의 설정 작업 없이 같은 그래들 버전 환경에서 개발할 수 있도록 합니다.**

## \{companyName\}-common

큐더스웍스2.0 프로젝트의 모듈들 중에서 **공통 모듈**로 구분됩니다.

공통 모듈에서는 모든 모듈에서 사용할 수 있는 공통 데이터 구조 클래스, 공통 어노테이션, 공통 코드 열거형(Enum) 클래스, 공통 예외 클래스, 공통 시스템 프로세스 처리 클래스, 공통 유틸리티 클래스 등 다양한
공통 클래스들을 포함합니다.

## \{companyName\}-config:config-\{configName\}

큐더스웍스2.0 프로젝트의 모듈들 중에서 **설정 모듈로** 구분합니다.

설정 모듈은 대표적으로 데이터베이스 커넥션 설정 모듈을 포함하고 있습니다. 다른 모듈에서 중복적으로 설정해야하는 설정들을 한 곳에서 관리할 수 있습니다.

## \{applicationName\}-application

큐더스웍스2.0 프로젝트의 모듈들 중에서 **애플리케이션 모듈**로 구분합니다.

애플리케이션 모듈은 기능 담당 개발자들이 주로 작업할 공간으로서 큐더스웍스 서비스와 REST API 클래스들을 포함하고 있습니다. 해당 모듈에서 단독으로 애플리케이션을 실행할 수 없는 특징이 있습니다.

## \{applicationName\}-boot

큐더스웍스2.0 프로젝트 모듈들 중에서 **애플리케이션 메인 실행 모듈**로 구분합니다.

애플리케이션 메인 실행 모듈은 애플리케이션 최종 빌드 및 배포 기준이 되는 모듈이며 시큐리티와 같은 애플리케이션 전체 시스템 프로세스가 동작되도록 구성하는 모듈입니다.

## \{applicationName\}-boot-test

큐더스웍스2.0 프로젝트 모듈들 중에서 **애플리케이션 테스트 실행 모듈**로 구분합니다.

애플리케이션 테스트 실행 모듈은 순수하게 REST API를 테스트하기 위한 실행 모듈입니다.

애플리케이션 실행 모듈들을 별도로 분리하게 되면서 애플리케이션 기능 담당 개발자들이 보다 빠르게 REST API를 테스트하고 다른 시스템 프로세스로 부터 방해받지 않도록 합니다. 전체 시스템 프로세스를 포함하여
테스트해야 할 시, 애플리케이션 실행 모듈로 테스트를 진행하면 되기 때문에 테스트 코드를 작성하지 않는 지금 상황에서 개발자가 보다 유연하게 테스트 할 수 있도록 합니다.

## persistence:orm-core

큐더스웍스2.0 프로젝트 모듈들 중에서 **ORM 코어 모듈**로 구분합니다.

ORM 코어 엔티티에서 사용될 감시 리스너, 데이터/타입 컨버터, 엔티티 영속 리스너 등이 관리되는 모듈입니다.

## persistence:orm-data

큐더스웍스2.0 프로젝트 모듈들 중에서 **ORM 데이터 모듈**로 구분합니다.

ORM 데이터 모듈은 JPA 엔티티의 공통 값 타입들이 관리되는 모듈입니다.

## persistence:schema-\{schemaName\}

큐더스웍스2.0 프로젝트 모듈들 중에서 **스키마 모듈**로 구분합니다.

여기서 스키마는 데이터베이스의 스키마를 의미합니다.

스키마 모듈은 오라클 스키마 기준으로 나뉘며 각 오라클 스키마 별로 테이블과 매핑된 JPA 엔티티가 관리됩니다.

## settings.gradle

settings.gradle 파일은 **프로젝트명을 설정**하고 **프로젝트에 포함할 모듈들을 설정**하는 파일입니다. 따라서, 새로운 모듈 추가 시 이 파일에 추가한 모듈이 프로젝트에 등록되어 있는지 꼭 확인해줘야
합니다. 등록 되어있지 않다면 그래들 관리 목록에서 제외되기 때문에 아래와 같이 추가한 모듈을 등록해야합니다.

```groovy
rootProject.name = 'kw2_be' // 프로젝트명 설정 구문

include 'irk-common'  // 모듈 등록 설정 구문
```
{collapsible="true" collapsed-title="settings.gradle"}