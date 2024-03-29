# HeXA Bootcamp 2024 Backend Track 과제 #2 (WIP)

![Hexa Bootcamp Logo](docs/assets/hexa-bootcamp-logo.png)

백엔드 트랙의 두 번째 과제를 완수하고 오신 분들 수고하셨습니다!
이 과제는 백엔드 트랙의 두 번째 과제로, 백엔드 및 데브옵스 개발자로서 기초적인 서버 인프라 구축 능력을 키우는 것을 목표로 합니다.
아래의 과제 설명을 자세히 읽어 두 번째 과제도 성공적으로 과제를 완수하시기를 바라겠습니다.

구분 | 내용
--- | ---
과제명 | URL 단축 서비스 EC2 인스턴스 배포
제출 기한 | 2024년 4월 5일 오후 11시 59분
과제 설명 | [과제 설명](#과제-설명)
과제 진행 | [과제 진행](#과제-진행)
과제 평가 기준 | [과제 평가 기준](#과제-평가-기준)
ETC | [ETC](#ETC)


## 과제 설명

본 과제에서 여러분은 [지난 과제](https://github.com/01Joseph-Hwang10/url-shortener-template)에서 개발한 URL Shortener를 AWS의 EC2 인스턴스에 배포하고, API가 인스턴스의 public 도메인으로 접근 가능하도록 구성하게 됩니다.

AWS EC2는 클라우드 컴퓨팅 서비스인 AWS의 가상 서버 (Virtual Machine) 서비스로, 사용자가 원하는 운영 체제와 네트워크 설정을 선택하여 원하는 가상 컴퓨팅 머신을 생성하고 사용할 수 있습니다. EC2 인스턴스를 사용하면 사용자는 물리적인 서버를 구입하거나 유지보수할 필요 없이, 필요한 만큼의 컴퓨팅 리소스를 사용할 수 있습니다.

여러분은 수업에서 배운 내용을 참고하여, EC2 인스턴스를 생성하고, 해당 인스턴스에 URL Shortener를 배포하고, 인스턴스의 public 도메인으로 API에 접근할 수 있도록 구성해야 합니다.

본 과제는 지난 과제에서 사용한 리포지토리를 그대로 사용하여 진행하게 됩니다. `Dockerfile`, `nginx.conf`, `docker-compose.yml` 등의 파일 변경사항들은 전부 해당 리포지토리에 커밋하시면 됩니다.

## Pre-requisites

본 과제를 진행하기 위해서는 다음의 사항이 필요합니다.

- Linux 기반 OS (WSL, macOS, Linux 등)
- AWS 계정 생성
  - 카드 등록 필요. (아래 NOTE 참조)
- Docker
- Nginx

> [!NOTE]\
> 본 과제들은 AWS Free Tier 범위 내에서 진행되며, AWS Free Tier 크레딧을 다 쓰신 분이라 하더라고 전체 과제 진행에 $10 이상의 요금이 발생하지 않기 때문에, 안심하고 진행하셔도 됩니다.

## 과제 진행

본 과제는 다음과 같은 순서로 진행됩니다.

### 1. 배포 작업용 브랜치 생성

지난 과제에서 사용한 리포지토리의 `develop` 브랜치에서 새로운 브랜치 `deploy`를 생성합니다. 이 브랜치에 배포 작업 관련 코드를 커밋하게 됩니다.

### 2. 배포를 위한 기존 프로젝트 코드 수정

지난 과제에서 작성한 URL Shortener 앱을 프로덕션에 배포하기 위해, 프로젝트 코드를 수정해야 할 수 있습니다.

예를 들어, 지난 과제에서는 로컬 환경에서 동작하는 것을 가정하고, `localhost`로 API에 접근하는 코드를 작성했을 수 있습니다. 이를 EC2 인스턴스의 public 도메인으로 접근하도록 수정해야 합니다.

### 3. Dockerfile 작성

URL Shortener를 배포하기 위한 Dockerfile을 작성합니다. 
이 Dockerfile은 지난 과제에서 작성한 애플리케이션을 실행해야 합니다.

### 4. Docker 이미지 빌드 및 실행 테스트

Dockerfile을 사용하여 Docker 이미지를 빌드하고, 
해당 이미지를 사용하여 컨테이너를 실행하는 전체 과정을 로컬에서 먼저 테스트합니다.

### 5. EC2 인스턴스 생성

AWS Management Console에 로그인하여 EC2 인스턴스를 생성합니다. 인스턴스 생성 시 아래의 사항을 유의하여 생성합니다.

- 인스턴스 유형: t2.micro (Free Tier)
- 운영 체제: Debian 12
- 보안 그룹: **HTTP, HTTPS 트래픽을 허용하는 보안 그룹 생성**

#### Materials

- [AWS EC2 인스턴스 생성 방법](./docs/how-to-create-ec2-instance.md)
- [AWS EC2 인스턴스 SSH 연결 방법](./docs/how-to-connect-to-ec2-instance.md)

### 6. 인스턴스 set-up 스크립트 작성 및 실행

URL Shortener를 배포하기 위한 인스턴스 set-up 스크립트를 작성합니다. 
이 스크립트는 다음과 같은 작업을 수행해야 할 것입니다.

- `apt update` 및 `apt upgrade` 명령어를 통한 패키지 업데이트
- Docker 설치
- Nginx 설치
- ...

> [!NOTE]\
> Set-up script는 SSH 등을 통해 직접 인스턴스에 접속해서 테스트하는 것이 훨씬 편할 것이기 때문에, 
> github 리포지토리에는 최종적으로 작성한 스크립트를 커밋하시면 됩니다.

### 7. URL Shortener 배포

EC2 인스턴스 내부에 프로젝트 파일을 업로드하고, [1번](#1-dockerfile-작성)에서 작성한 Dockerfile을 사용하여 URL Shortener를 EC2 인스턴스에 배포합니다.

### 8. Nginx 설정 파일 작성 및 실행

Nginx 설정 파일을 작성하여 URL Shortener에 대한 리버스 프록시를 설정합니다. 이 설정 파일은 인스턴스의 public 도메인으로 API에 접근할 수 있도록 설정해야 합니다.

> [!NOTE]\
> Nginx 설정 파일 또한 직접 인스턴스에 접속해서 테스트하는 것이 훨씬 편할 것이기 때문에, 
> github 리포지토리에는 최종적으로 작성한 스크립트를 커밋하시면 됩니다.

### 9. 외부에서 API 접근 테스트

Postman, cURL 등을 사용하여 외부에서 인스턴스의 public 도메인으로 API에 접근하여 URL Shortener가 정상적으로 동작하는지 테스트합니다.

### 10. 과제 제출

위의 모든 과정을 완료하신 후, 최종적으로 `deploy` 브랜치에 변경사항을 커밋하고, `deploy` -> `develop` 브랜치로 Pull Request를 생성하여 과제를 제출합니다. PR에는 URL Shortener를 배포하기 위해 어떤 작업을 했는지에 대한 설명을 간략히 작성해주시고, URL Shortener를 배포한 EC2 인스턴스의 public http url을 함께 첨부해주시기 바랍니다.

> [!NOTE]\
> EC2 인스턴스의 public URL은 해당 EC2 인스턴스의 상세 정보 페이지에서 확인할 수 있습니다.

![ec2-dashboard](./docs/assets/ec2-dashboard.png)
![ec2-instance-status](./docs/assets/ec2-instance-status.png)

본 과제 제출 시에 필수로 추가되어야 하는 파일은 다음과 같습니다.

- 배포 시에 사용한 `Dockerfile`
- 배포 시에 사용한 Nginx 설정 파일
- 배포 시에 사용한 set-up 스크립트 (bash 스크립트 등)

## 과제 평가 기준


본 과제는 다음과 같은 기준으로 평가됩니다.

- URL Shortener가 EC2 인스턴스의 public http url로 외부에서 접근 가능한지 여부
- URL Shortener의 기능들이 정상적으로 동작하는지 여부


## ETC

- EC2 인스턴스 접속을 위해 생성한 키페어 파일은 유출되지 않도록 각별히 주의하시기 바랍니다.
- 환경변수, 시크릿 키 등의 민감한 정보 또한 유출되지 않도록 각별히 신경써주시기 바랍니다.
- 과제 수행 중 궁금한 사항이 있을 경우, 본 저장소의 Issues 탭에 새로운 이슈를 생성하여, 궁금한 사항을 질문해주시기 바랍니다.
