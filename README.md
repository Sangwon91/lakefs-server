# lakeFS NAS 서버

이 프로젝트는 NAS 스토리지를 사용하는 lakeFS 서버를 Docker Compose로 구성한 것입니다.

## 시작하기

### 사전 요구사항

- Docker 및 Docker Compose가 설치되어 있어야 합니다.
- NAS 스토리지가 마운트되어 있어야 합니다.

### 설치 및 실행 방법

1. 저장소를 클론합니다:
   ```bash
   git clone <repository-url>
   cd lakefs-server
   ```

2. 환경 변수 설정:
   ```bash
   cp .env.example .env
   ```

3. `.env` 파일을 편집하여 필요한 환경 변수를 설정합니다:
   ```
   LAKEFS_AUTH_ENCRYPT_SECRET_KEY=your-random-secret-string
   LAKEFS_INSTALLATION_ACCESS_KEY_ID=your-access-key-id
   LAKEFS_INSTALLATION_SECRET_ACCESS_KEY=your-secret-access-key
   ```

4. docker-compose.yml 파일에서 볼륨 마운트 경로를 확인하고 필요시 수정합니다:
   ```yaml
   volumes:
     - /nas/shared/lakefs-storage:/nas_data  # 여기에 실제 NAS 경로를 지정하세요
   ```

5. Docker Compose로 서비스를 시작합니다:
   ```bash
   docker-compose up -d
   ```

6. 서비스가 시작되면 다음 URL로 lakeFS 웹 UI에 접속할 수 있습니다:
   ```
   http://localhost:48000
   ```

7. 로그인 정보:
   - 사용자 이름: admin
   - 액세스 키 ID: .env 파일에 설정한 LAKEFS_INSTALLATION_ACCESS_KEY_ID 값
   - 시크릿 액세스 키: .env 파일에 설정한 LAKEFS_INSTALLATION_SECRET_ACCESS_KEY 값

## 환경 변수 설명

`.env` 파일에 설정할 수 있는 주요 환경 변수:

- `LAKEFS_AUTH_ENCRYPT_SECRET_KEY`: lakeFS 인증 암호화에 사용되는 비밀 키
- `LAKEFS_INSTALLATION_ACCESS_KEY_ID`: 관리자 계정의 액세스 키 ID
- `LAKEFS_INSTALLATION_SECRET_ACCESS_KEY`: 관리자 계정의 시크릿 액세스 키

## 서비스 중지

서비스를 중지하려면 다음 명령을 실행합니다:
```bash
docker-compose down
```

## 데이터 지속성

lakeFS 데이터는 NAS 스토리지에 저장되므로 컨테이너를 재시작해도 데이터가 유지됩니다.

## 문제 해결

서비스 로그를 확인하려면:
```bash
docker-compose logs -f
```

## 보안 고려사항

- `.env` 파일은 민감한 정보를 포함하고 있으므로 Git에 커밋하지 마세요.
- 프로덕션 환경에서는 강력한 비밀번호와 액세스 키를 사용하세요.
- 필요한 경우 네트워크 보안을 강화하여 lakeFS 서버에 대한 접근을 제한하세요. 