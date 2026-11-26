# LostHub Backend

2026학년도 2학기 소프트웨어시스템설계 팀 프로젝트로 개발하는 교내 분실물 서비스 LostHub의 백엔드 서버입니다.

## 현재 상태

저장소 초기 설정 단계입니다. 애플리케이션 서버, 데이터베이스, WebSocket, 이미지 저장소 및 Railway 실행 기능은 아직 구현되지 않았습니다.

## 기술 기준

- Java 17
- Gradle Wrapper 및 Groovy DSL
- Spring과 Spring Boot를 사용하지 않는 순수 Java 서버 구조
- JUnit 5
- 영속 데이터 저장을 위한 MySQL
- 게시글 이미지 저장을 위한 Railway Bucket
- 배포를 위한 Railway Railpack

Docker, Dockerfile 및 Docker Compose는 사용하지 않습니다.

## 검증

Windows에서 전체 백엔드 검증 작업을 실행합니다.

```powershell
.\gradlew.bat verifyLostHubBe
```

Linux 또는 Railway에서는 다음 명령을 사용합니다.

```bash
./gradlew verifyLostHubBe
```

이 작업은 테스트를 실행하고 Javadoc을 생성한 뒤 런타임 의존성을 포함한 Fat JAR를 빌드합니다.

각 작업은 별도로 실행할 수도 있습니다.

```powershell
.\gradlew.bat test
.\gradlew.bat javadoc
.\gradlew.bat fatJar
```

생성된 Javadoc은 `build/docs/javadoc`에 저장됩니다.

## Railway 배포

백엔드에 실제 서버 진입점과 `GET /health` endpoint가 구현된 후 Railway 배포를 활성화합니다.

예정된 배포 설정은 다음과 같습니다.

- 빌더: Railpack
- JDK 환경변수: `RAILPACK_JDK_VERSION=17`
- Wait for CI: 활성화
- Health Check 경로: `/health`
- 실행 명령: `java -jar build/libs/losthub-be-all.jar`

서버는 `0.0.0.0`에서 요청을 수신하고 Railway의 `PORT` 환경변수에서 포트 번호를 읽어야 합니다.

## 문서 작성 규칙

`README.md`와 과제 API Manual에 사용하는 Javadoc 설명은 한글로 작성합니다. 그 밖의 저장소 Markdown 파일은 영어로 작성합니다. 외부 HTTP 및 WebSocket API 문서는 이 저장소에서 관리하지 않습니다.
