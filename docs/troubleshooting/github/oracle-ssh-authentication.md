# Oracle Cloud에서 GitHub SSH 인증 문제 해결

> Oracle Cloud `ros2-server`에서 `CHUNGJINWOO/guidebook`의 로컬 commit을 push할 때 발생한 HTTPS 인증 문제의 실제 해결 기록이다.

## 1. 환경

- 서버: Oracle Cloud `ros2-server`
- OS: Ubuntu
- Repository: `CHUNGJINWOO/guidebook`
- Local path: `/mnt/data/guidebook`
- 초기 `origin`: `https://github.com/CHUNGJINWOO/guidebook.git`
- Push 대상 branch: `main`
- 로컬 commit: `f28105dcf62233e2f9773e2d25b25d75d0e90252`
- Commit message: `docs: add AI development guide and workflows`

## 2. 문제 발생

HTTPS remote로 push하려 했으나 GitHub 인증 단계에서 실패했다.

```text
could not read Username for 'https://github.com': No such device or address
```

## 3. 초기 조사

확인한 내용은 다음과 같다.

- GitHub repository는 존재하며 public이었다.
- 기본 branch는 `main`이었다.
- GitHub API를 통한 repository 조회는 가능했다.
- 일반 Git HTTPS push는 인증 단계에서 실패했다.
- `credential.helper`가 설정되어 있지 않았다.
- `gh` 명령이 설치되어 있지 않았다.
- GitHub 인증 관련 환경 변수는 설정되어 있지 않았다.

따라서 repository 자체가 없거나 비공개라서 발생한 문제는 아니었다. Oracle 서버에서 일반 Git HTTPS push에 사용할 인증 구성이 없었다.

## 4. GitHub 연결 도구 확인

GitHub 연결 도구에서는 repository 조회가 가능했지만, 파일 생성 요청은 다음 오류로 실패했다.

```text
403 Resource not accessible by integration
```

이 연결 도구의 API 조회 권한은 Oracle 서버의 일반 Git push 인증을 대신하지 못했다. 이 사건에서는 연결 도구를 push 대체 수단으로 사용할 수 없었다.

## 5. SSH 방식으로 전환

Oracle 서버에서 GitHub용 ED25519 SSH key를 생성하고, 공개키를 GitHub 계정의 SSH Authentication Key로 등록했다.

사용한 개인키 경로는 `~/.ssh/id_ed25519_github_ros2`이다. 개인키 내용과 passphrase는 기록하지 않는다.

### 5.1 키 인증 확인

키를 명시하여 GitHub SSH 인증을 테스트했다.

```bash
ssh -T -i ~/.ssh/id_ed25519_github_ros2 git@github.com
```

성공 메시지가 반환되어 SSH key 자체의 GitHub 인증이 정상임을 확인했다.

```text
Hi CHUNGJINWOO! You've successfully authenticated, but GitHub does not provide shell access.
```

### 5.2 Git SSH 설정

키를 명시한 테스트는 성공했지만, 일반 `git push`에서는 해당 키가 자동으로 선택되지 않았다. `~/.ssh/config`에 GitHub용 identity를 지정했다.

```sshconfig
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_ros2
    IdentitiesOnly yes
```

설정 파일 권한을 제한했다.

```bash
chmod 600 ~/.ssh/config
```

이후 기본 SSH 설정을 사용하는 인증 테스트도 성공했다.

```bash
ssh -T git@github.com
```

## 6. Git remote 변경 및 push

`origin`을 HTTPS에서 SSH 주소로 변경했다.

```bash
git remote set-url origin git@github.com:CHUNGJINWOO/guidebook.git
```

확인 결과:

```text
origin  git@github.com:CHUNGJINWOO/guidebook.git (fetch)
origin  git@github.com:CHUNGJINWOO/guidebook.git (push)
```

기존 로컬 commit을 `main`에 push했다.

```bash
git push -u origin main
```

결과:

```text
To github.com:CHUNGJINWOO/guidebook.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

`main` branch가 GitHub repository에 생성되고 기존 로컬 commit이 정상적으로 push되었다.

## 7. 결론

이번 문제의 원인은 repository 접근 권한이 아니라 Oracle 서버의 일반 Git HTTPS 인증 정보가 구성되어 있지 않았던 것이었다. GitHub 연결 도구를 통한 repository 조회 권한과 일반 Git push 인증은 별개의 경로다.

Oracle 서버에서 GitHub SSH 인증을 구성하고 `origin`을 SSH remote로 변경한 뒤 일반 `git push`가 정상 동작했다.

## 8. 향후 AI Agent가 사용할 진단 순서

1. 현재 repository와 remote를 확인한다.

   ```bash
   git remote -v
   ```

2. remote 주소가 HTTPS인지 SSH인지 확인한다.

3. SSH remote라면 GitHub SSH 인증을 확인한다.

   ```bash
   ssh -T git@github.com
   ```

4. SSH 인증이 실패하면 다음을 확인한다.
   - `~/.ssh/config`의 GitHub 설정과 `IdentityFile`
   - 지정한 SSH key 파일의 존재 여부
   - GitHub 계정에 공개키가 등록되어 있는지

5. HTTPS remote라면 인증 구성을 확인한다.

   ```bash
   git config --global credential.helper
   gh --version
   ```

6. GitHub 연결 도구의 API 조회 가능 여부를 일반 Git push 가능 여부와 동일하게 간주하지 않는다.

7. Credential, token, private key, passphrase는 문서나 repository에 기록하지 않는다.
