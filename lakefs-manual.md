# lakeFS 사용 메뉴얼

## 목차

1. [소개](#소개)
2. [시작하기](#시작하기)
3. [lakectl 설치](#lakectl-설치)
4. [lakectl 기본 명령어](#lakectl-기본-명령어)
5. [로컬 환경에서 작업하기](#로컬-환경에서-작업하기)
6. [자주 사용하는 워크플로우](#자주-사용하는-워크플로우)
7. [문제 해결](#문제-해결)

## 소개

lakeFS는 데이터 레이크에 Git과 같은 버전 관리 기능을 제공하는 오픈소스 플랫폼입니다. 이 메뉴얼은 팀원들이 `lakectl`을 사용하여 로컬 환경에서 lakeFS를 효과적으로 활용할 수 있도록 안내합니다.

## 시작하기

### lakeFS 서버 접속하기

우리 팀의 lakeFS 서버는 다음 URL에서 접근할 수 있습니다:

```

http://localhost:8000

```

로그인 정보:

-**Access Key ID**: `AKIAIOSFODNN7EXAMPLE`

-**Secret Access Key**: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

### 웹 인터페이스 사용하기

lakeFS 웹 인터페이스에서는 다음과 같은 작업을 수행할 수 있습니다:

- 저장소(Repository) 생성 및 관리
- 브랜치(Branch) 생성 및 관리
- 커밋(Commit) 이력 확인
- 파일 업로드 및 다운로드
- 머지(Merge) 작업 수행

## lakectl 설치

`lakectl`은 lakeFS의 명령줄 인터페이스(CLI)로, 로컬 환경에서 lakeFS 서버와 상호작용할 수 있게 해줍니다.

### Linux/Mac 설치 방법

```bash

# 최신 버전 다운로드

curl-shttps://docs.lakefs.io/assets/downloads/lakectl/install.sh | bash


# 설치 확인

lakectl--version

```

### Windows 설치 방법

1. [lakeFS 릴리스 페이지](https://github.com/treeverse/lakeFS/releases)에서 최신 Windows 버전의 `lakectl`을 다운로드합니다.
2. 다운로드한 파일의 압축을 풀고 `lakectl.exe`를 PATH에 추가합니다.

### 설정하기

`lakectl`을 사용하기 전에 설정 파일을 생성해야 합니다:

```bash

# 설정 파일 생성

mkdir-p~/.lakectl

cat > ~/.lakectl/config.yaml << EOL

credentials:

  access_key_id: AKIAIOSFODNN7EXAMPLE

  secret_access_key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

server:

  endpoint_url: http://localhost:8000

EOL

```

## lakectl 기본 명령어

### 도움말 보기

```bash

lakectl--help

```

### 저장소 목록 보기

```bash

lakectlrepolist

```

### 저장소 생성하기

```bash

lakectlrepocreatelakefs://my-repos3://my-bucket

```

### 브랜치 목록 보기

```bash

lakectlbranchlistlakefs://my-repo

```

### 브랜치 생성하기

```bash

lakectlbranchcreatelakefs://my-repo/new-branch--sourcemain

```

### 파일 업로드하기

```bash

lakectlfsuploadlocal-file.csvlakefs://my-repo/main/path/to/file.csv

```

### 파일 다운로드하기

```bash

lakectlfsdownloadlakefs://my-repo/main/path/to/file.csvlocal-file.csv

```

### 변경사항 커밋하기

```bash

lakectlcommitlakefs://my-repo/main-m"파일 추가 및 수정"

```

### 브랜치 머지하기

```bash

lakectlmergelakefs://my-repo/feature-branchlakefs://my-repo/main

```

## 로컬 환경에서 작업하기

`lakectl local` 명령어를 사용하면 로컬 파일 시스템에서 직접 lakeFS 저장소와 작업할 수 있습니다.

### 로컬 작업 디렉토리 초기화하기

```bash

# 새 디렉토리 생성

mkdirmy-lakefs-project

cdmy-lakefs-project


# 로컬 작업 디렉토리 초기화

lakectllocalinitlakefs://my-repo/main

```

### 파일 상태 확인하기

```bash

lakectllocalstatus

```

### 변경사항 스테이징하기

```bash

# 특정 파일 스테이징

lakectllocaladdpath/to/file.csv


# 모든 변경사항 스테이징

lakectllocaladd--all

```

### 변경사항 커밋하기

```bash

lakectllocalcommit-m"데이터 파일 업데이트"

```

### 변경사항 푸시하기

```bash

lakectllocalpush

```

### 원격 변경사항 가져오기

```bash

lakectllocalpull

```

### 브랜치 전환하기

```bash

lakectllocalcheckoutfeature-branch

```

### 새 브랜치 생성 및 전환하기

```bash

lakectllocalcheckout-bnew-feature-branch

```

## 자주 사용하는 워크플로우

### 1. 새 기능 개발 워크플로우

```bash

# 1. 메인 브랜치에서 최신 변경사항 가져오기

lakectllocalcheckoutmain

lakectllocalpull


# 2. 새 기능 브랜치 생성

lakectllocalcheckout-bfeature-xyz


# 3. 파일 수정 작업 수행

# ... (파일 수정) ...


# 4. 변경사항 스테이징 및 커밋

lakectllocaladd--all

lakectllocalcommit-m"XYZ 기능 구현"


# 5. 변경사항 원격 저장소에 푸시

lakectllocalpush

```

### 2. 데이터 분석 워크플로우

```bash

# 1. 분석용 브랜치 생성

lakectllocalcheckout-banalysis-abc


# 2. 데이터 파일 다운로드

lakectlfsdownloadlakefs://my-repo/main/data/dataset.csv./data/dataset.csv


# 3. 분석 결과 파일 생성

# ... (분석 작업 수행) ...


# 4. 결과 파일 업로드

lakectllocaladd./results/

lakectllocalcommit-m"ABC 분석 결과 추가"

lakectllocalpush

```

### 3. 데이터 품질 검증 워크플로우

```bash

# 1. 검증용 브랜치 생성

lakectllocalcheckout-bvalidate-data


# 2. 데이터 파일 다운로드

lakectlfsdownloadlakefs://my-repo/main/data/dataset.csv./data/dataset.csv


# 3. 데이터 검증 스크립트 실행

# ... (검증 작업 수행) ...


# 4. 검증 결과 업로드

lakectllocaladd./validation-results/

lakectllocalcommit-m"데이터 검증 결과 추가"

lakectllocalpush


# 5. 검증 통과 시 메인 브랜치에 머지

lakectlmergelakefs://my-repo/validate-datalakefs://my-repo/main

```

## 문제 해결

### 인증 오류

인증 오류가 발생하는 경우 `~/.lakectl/config.yaml` 파일의 인증 정보가 올바른지 확인하세요.

### 연결 오류

lakeFS 서버에 연결할 수 없는 경우:

1. 서버가 실행 중인지 확인하세요.

2.`~/.lakectl/config.yaml` 파일의 `endpoint_url`이 올바른지 확인하세요.

3. 네트워크 연결을 확인하세요.

### 충돌 해결하기

머지 충돌이 발생한 경우:

```bash

# 충돌 상태 확인

lakectllocalstatus


# 충돌 파일 수정 후 스테이징

lakectllocaladdpath/to/conflicted/file


# 충돌 해결 완료 후 커밋

lakectllocalcommit-m"충돌 해결"


# 변경사항 푸시

lakectllocalpush

```

### 로그 확인하기

```bash

# 커밋 이력 확인

lakectlloglakefs://my-repo/main


# 특정 파일의 변경 이력 확인

lakectlfsstatlakefs://my-repo/main/path/to/file.csv--show-meta

```

---

이 메뉴얼에 대한 질문이나 추가 정보가 필요하시면 팀 관리자에게 문의하세요.
