# GCP

 ### GCP 인스턴스 생성
 
 자세한 설명은 생략한다.

 ### ssh 설치
 
 나는 window 환경이니 powershell에 접속해 vscode 전까지의 과정을 밟자. (회사 들어가면 맥북을 쓰도록 하자..)
 * 참고: https://cloud.google.com/compute/docs/connect/create-ssh-keys?hl=ko

 ### ssh 키 생성

    Window10 이상일 때: 
    ssh-keygen -t rsa -f C:\Users\WINDOWS_USER\.ssh\KEY_FILENAME -C USERNAME -b 2048

    ssh-keygen -t rsa -f C:\Users\gkatn\.ssh\first-ssh-key -C ubuntu -b 2048

WINDOWS_USER: 사용자 이름. 나의 경우에는 gkatn.

KEY_FILENAME:  SSH 키 파일의 이름. 임의로 설정하면 된다. 설정한 이름으로 비공개 키 파일이 생성된다.

USERNAME: VM의 사용자 이름. 나의 경우엔 ubuntu이다. 

위 명령어를 입력하면 키에 대한 비밀번호를 입력하라는 메세지가 뜬다. 이 때 설정하지 않고 엔터 두번 누르면 생성이 된다.

### ssh 키 추가

.ssh 디렉토리로 이동하면(cd ~/.ssh), 파일이 생성된 것을 볼 수 있다. 이 때, .pub가 붙은 파일이 공개 키, 아닌 파일이 비밀 키다.

cat first-ssh-key.pub 명령어로 텍스트를 출력 후 복사한다.

compute engine 대시보드로 가서 좌측 탭의 메타데이터로 이동한다.

ssh추가 버튼을 누르고, 복사한 공개 키를 입력하고 저장하다.

### 접속

좌측 VM 인스턴스 탭을 선택하고, 인스턴스 목록에서 외부 IP에 해당하는 주소를 확인한 뒤 SSH 접속을 한다.

    ssh -i ~/.ssh/first-ssh-key(공개키의 위치) ubuntu@xxx.xxx.xxx.xxx

    ssh -i C:\Users\gkatn\.ssh\first-ssh-key ubuntu@34.47.66.83

    이 때, 외부 IP는 인스턴스를 정지,재개 할 때 마다 바뀐다.

### VSCode

extension 에서 Remote-ssh 를 설치 후, 좌측 탭에 생성된 remote explorer 에서 톱니바퀴버튼 우측에 +를 누르고, ssh 접속할 때 사용한 명령어를 입력한다. (ssh -i ~/.ssh/first-ssh-key(공개키의 위치) ubuntu@xxx.xxx.xxx.xxx)

이후 저장할 설정 파일을 지정하라고 하는데, /{홈 경로}/.ssh/config 로 설정한다.

##### 주의
window 환경에서, config 파일을 보면 경로 설정이 잘못 되어 있을 것이다. 접속에 문제가 생긴다면 config파일을 잘 살피도록 하자.

### VSCode에 파이썬 환경 구축

miniconda 베이스에 poetry로 의존성을 관리하자. (짧은 경험이지만 이전에 환경때문에 엄청 고생했는데, poetry를 진작에 적극 활용했다면 얼마나 편했을까..)

https://docs.anaconda.com/free/miniconda/
여기서 리눅스 64버전을 다운로드 하자. (터미널에서 wget 링크)

bash ./Miniconda3-latest-Linux-x86_64.sh 명령어로 설치 마무리. 라이센스 동의 문장이 나오면 엔터 누르고 q로 스킵.

이후 기본 경로 설정, conda 상시 활성화 질문이 순서대로 나오는데 이번엔 둘 다 기본세팅으로 진행하지만 알아는 두자.

conda create -n 환경이름 python=3.11
명령어로 가상환경을 만든다.

conda activate 환경이름 명령어로 활성화.

pip install poetry로 poetry 설치. 

디렉토리 생성 후, 그 디렉토리에서 poetry init 명령어로 poetry를 초기화한다.

좌측 탭의 폴더 열기로 해당 디렉토리로 접속하면 poetry가 생성한 파일이 보인다.

이후 poetry add 명령어로 호환되는 라이브러리를 설치해 이용하면 된다.