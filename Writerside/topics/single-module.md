# 단독 모듈

## 전체

- frontend
- src
    - main
        - [java](#java)
        - [resources](#resources)
    - test

## java {id="java"}

- bellins
    - 프로젝트명
        - application
            - common
            - config
            - support
                - auth
                - login
                    - ctrl
                    - dto
                        - converter
                    - entity
                    - exception
                    - repo
                    - svc
                - signup
            - web
                - 도메인(업무)명
                    - ctrl
                    - dto
                        - converter
        - common
            - dto
            - util
        - domain
            - 도메인(업무)명
                - dto
                - entity
                - exception
                - mapper
                - repo
                - svc
            - common
        - infra
            - email
            - push
            - sms
                - dto
                - entity
                - mapper
                - repo
                - svc

{type="medium"}
application
: 어플리케이션 설정  
어플리케이션의 기본 또는 공통 기능(로그인, 가입, 권한 등)  
각 도메인 업무의 웹 어뎁터(Controller, Request/Response DTO)

application.common
: 어플리케이션 공통 기능  
aop  
auth  
event  
exception  
filter  
interceptor

application.config
: 어플리케이션 설정

application.support
: 로그인, 가입, 권한 등 메인 Domain 이외의 기본적인 domain  
controller 에서 entity 까지 각 도메인의 모든 계층의 코드를 포함

application.web
: domain 패키지의 도메인에 의존하는 web 컨트롤러

common
: 공통 DTO  
공통 exception, exception handler  
공통 util 클래스

domain
: 도메인(업무) 별 주요 로직  
service, repository(mapper), entity, dto 등

infra
: 도메인(업무) 별 주요 로직  
SMS, 이메일, 푸시 등

`application`(특히 `web` 패키지) 는 `domain` 패키지에 의존하지만, 반대의 경우는 금지  
`application.support` 패키지는 `application`,`common` 패키지 이외의 의존성을 갖지 않음  
`application` 의 `request`, `response` dto 는 가능한 `record` 로 생성  
`domain` 패키지는 외부 라이브러리와 `common` 패키지 이외의 의존성을 가지지 않음  
`converter` : `MapStruct` 를 이용한 `Mapper`

## resources {id="resources"}

- config
- static
- templates

{type="narrow"}
config
: 설정 파일, mybatis Mapper 파일 등

static
: web resources(css, font, image, js 등)  
SPA 인 경우 빌드된 프론트엔드 배포 파일

templates
: view 파일 (.mustache, .jte 등)