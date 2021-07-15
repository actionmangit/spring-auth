# spring-auth

## build.gradle 파일 생성

### axion-release-plugin gradle plugin 설치

build 파일에 버전을 입력하는 것이 아닌 Git 이나 SVN의 __SCM(Software Cofiguration Managerment)__ 의 철학에 맞게 SCM에서 설정한 버저닝을 프로젝트의 버전으로 사용하는 plugin 이다.

### spring-boot-devtools 종속성 설치

소스 변경시 자동 재시작과 브라우저 핫스왑 기능을 제공한다.

## @Import 란

@Configuration으로 설정한 설정 파일을 두 개 이상 사용하는 경우 상속대신 사용하는 annotation 이다.

## users bean의 password 설정

Spring security 5 이상부터 패스워드 타입을 설정해야함. 현재 데모의 패스워드는 plaintext이기 때문에 `{noop}`를 패스워드 앞쪽에 써준다.

## RegisteredClient 생성

인증서버에 접속할 클라이언트를 등록하는 bean이다. 설정방법 세부내역을 보면 다음과 같다. `clientId` 클라이언트의 고유 아이디이다. `clientSecret` 클라이언트와 서버 사이의 비밀 코드이다.
`clientAuthenticationMethod`는 클라이언트 인증 방법으로 spring security 5.4 버전에서는 BASIC, POST 방법이 있으며 5.5 버전에서는 CLIENT_SECRET_BASIC, CLIENT_SECRET_JWT, CLIENT_SECRET_POST, PRIVATE_KEY_JWT 방법이 존재한다. 자세한 내용은 아래와 같다.

Version|클라이언트 인증 방법|설명
:---:|:---:|---
5.4|BASIC|헤더에 `Authorization: Basic {(BASE64 encoding){clientId}: {clientSecret}}` 값 입력
5.4|POST|토큰 요청시 Client ID, Client Secret값을 매개변수로 POST방식으로 전달하는 법
5.5|CLIENT_SECRET_BASIC|5.4 버전의 BASIC과 동일함
5.5|CLIENT_SECRET_JWT|client secret을 직접 포함하지 않고 서명 방식으로 전달하는 방법. client assertion(반드시 참이 되는)키 값으로 jwt형식의 데이터를 암호화 하여 보냄
5.5|CLIENT_SECRET_POST|5.4 버전의 POST와 동일함
5.5|PRIVATE_KEY_JWT|CLIENT_SECRET_JWT방식에서 공개키 개인키를 통한 서명방법

`authorizationGrantType`은 인증 방식 타입 설정이다. `AUTHORIZATION_CODE`, `IMPLICIT`, `REFRESH_TOKEN`, `CLIENT_CREDENTIALS`, `PASSWORD` 방식이 있다. 자세한 내용은 아래와 같다.

인증 방식 타입|내용
:---:|---
AUTHORIZATION_CODE|인증 코드 부여
IMPLICIT|AUTHORIZATION_CODE과 비슷하며 엑세스 토큰 즉시 발급. deprecated됨. 리프레시 토큰 발급은 권장하지 않음
REFRESH_TOKEN|엑세스 토큰이 만료되었을때 새로 고침용 토큰 발급
CLIENT_CREDENTIALS|clientId와 clientSecret값으로 인증. 리프레시 토큰 발급은 권장하지 않는다. 서버간 통신시 사용
PASSWORD|id, password 방식으로 토큰 발급
