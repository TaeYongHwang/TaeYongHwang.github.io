# Jenkins 설치 및 구동

- Linux 환경
- Docker
- Jenkins

Admin 계정명: hexlant

암호: hexlant

이름: hexlant

계정: bm1478@hexlant.com

1. 설치
```shell script
docker pull jenkins/jenkins:lts (일반 jenkins 이미지는 plugin 설치 안되는 에러)
docker run -d -p 8080:8080 -v /home/deploy/jenkins:/var/jenkins_home --name jenkins -u root jenkins/jenkins:lts (-v 옵션 host 디렉토리:container 디렉토리 연결)
(stop 상태에서는) docker start jenkins
(터미널 접속 시) docker exec -it jenkins /bin/bash
``` 
젠킨스 서버 8080로 실행 후 추천 Plugin 설치 옵션으로 설치

추가로

Github plugin: Jenkins와 Github 통합

Global Slack Notifier Plugin: Slack 연동(Job 알림 설정)

Publish Over SSH: ssh로 빌드 파일 보내기

Embeddable Build Status Plugin: Github 레포지토리에 빌드 상태바 생성

Managed Scripts: Node.js 기반의 서버를 배포하기 위한 스크립트

설치.

2. Jenkins 홈페이지 설정
- Jenkins 관리 > Configure Global Security
    - Security Realm

        Jenkins' own user database 를 체크. 사용자의 가입 허용은 가입이 필요한 경우만 체크

    - Authorization

        Matrix-based security를 선택하여 유저와 그룹의 허가할 퍼미션을 선택합니다. (현재는 유저 추가 후 지정)

    - CSRF Protection 설정 체크를 풀어줘야 합니다. 이렇게 해야 외부에서 Job에 대한 트리거링이 가능
    
- Jenkins 관리 > 시스템 설정
    - Github 탭
        Github 에서 생성한 Person Access Tokens 등록
        
        토큰 복사 후 Manage hooks 체크
        
        Credentials Add > Jenkins
        
        Kind: Secret text, Secret: Personal access token, ID: Github ID
    
    - Publish Over SSH
        Remote 서버에 ssh 통해서 배포 하기 위함.
        
        배포를 위해 scp를 사용하여 Jenkins에 있는 프로젝트 코드를 NAVER Cloud 서버에 복사해야 한다.
        1. Jenkins 서버 (컨테이너 안)에 ssh 키를 생성한다. ```ssh-keygen -t rsa```
        2. Jenkins(Docker)에서는 NAVER Cloud 서버를 호스트로, NAVER Cloud 서버에서는 Jenkins 서버(Docker)의 공개 키를 허가받은 키로 등록
        3. Jenkins 서버에서 NAVER Cloud 서버를 known_hosts로 등록 ```ssh-keyscan -H (NAVER CLOUD 서버)ip >> ~/.ssh/known_hosts```
        4. Jenkins에서 생성한 키 중에 공개 키(id_rsa.pub)를 NAVER Cloud 서버에서 생성한 authorized_keys에 입력한다. ```vi ~/.ssh/authorized_keys```
        
        ```shell script
        chmod 700 ~/.ssh
        chmod 600 ~/.ssh/id_rsa
        chmod 644 ~/.ssh/id_rsa.pub  
        chmod 644 ~/.ssh/authorized_keys
        chmod 644 ~/.ssh/known_hosts
        ```
        
        Path to Key 에 Jenkins 서버 개인키 Path
        
        SSH Server 에 배포하고자 하는 SSH 서버 (현재는 Docker를 통해 내부에서 젠킨스를 사용하기 때문에 내부망 IP)
        
            Name(작명), Hostname(IP 주소), Username(접속하고자 하는 계정 이름), Remote Directory (배포 경로 설정 안되어 있을 경우 이곳에 배포) 설정
            
    - Slack

        Slack App 에서 Jenkins 설치 후 토큰 Jenkins > 시스템 설정 > Slack 에서 토큰 등록

3. Github 홈페이지 설정.
- Github 계정 토큰 설정

계정 Settings > Developer settings > Personal access tokens > Generate new token > repo, admin:repo_hook 체크 후 생성
 
- Github Hook 사용 경우 (push 시 자동 빌드)

    - GitHub plugin은 jenkins가 GitHub의 hook을 받을 수 있도록 수동 모드와 자동모드를 제공. 
    - 수동 모드: 사용자가 Git Repository setting로 가서 Hooks & services을 직접 등록해 사용하는 것이고
    - 자동 모드: 사용자의 GitHub OAuth 토큰을 이용해 Jenkins가 자동으로 Hooks & services를 등록한다.

- Poll SCM 사용 경우 - X

4. 아이템 생성 후 저장소 연결하기 (SSH 이용) 또는 아이템 구성 들어가기.

- Github 홈페이지 
    
    Github Private Repository Setting: SSH Key 세팅 (Docker Jenkins Server에 접속해 ```ssh-keygen``` ```cat /.ssh/id_rsa.pub```)

    Option) Webhook 설정: Repo > Settings > Webhook > ```http://JENKIN'S SERVER/github-webhook/``` url로 지정 해주고 Active.

- Jenkins

    소스 코드 관리 탭에서 Github Credentials - Private Key, ID 로 저장. (Github 저장소에 Publick Key 있어야함.)

    이 때 저장소 주소는 ssh 주소로 해주기.

    - 빌드 유발 탭
        - Github hook trigger for GITScm polling 선택. (Webhook 사용 경우)
        - Poll SCM: Git 이 바라보는 브랜치에서 커밋같은 변화 감지하면 자동 빌드 후 배포.
          
          15분 간격으로 빌드 작업을 수행: H/15 * * * *
          
          모든 시간의 첫 30분 동안에 10분 간격으로 빌드를 수행: H(0-29)/10 * * * *
          
          주말을 제외한 주중에 9시부터 16시 사이에 2시간에 한번씩 빌드를 수행: H 9-16/2 * * 1-5
          
          12월 달은 제외하고 매달 1일과 15일에 한번씩 빌드를 수행: H H 1,15 1-11 * 
          
    - Build: Execute managed script 사용: 현재 스크립트 - Restart API
    
        - 파일 관리 (Jenkins 설정 > Managed Files > Add a new Config)
        ```shell script
        ssh root@10.41.171.111 <<EOF
         cd ~/explorer-api
         git fetch
         git pull origin dev
         npm install
         apidoc -i routes/ -o apidoc/
         pm2 restart api
         exit
        EOF
        ```
      
        빌드 후 조치는 Set Github commit status 라던가 Slack Notifications 라던가 자유롭게.
        
        