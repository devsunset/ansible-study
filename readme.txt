-------------------------------------------------

			# ANSIBLE-WORK #

-------------------------------------------------

########################################################
### Ansible

https://www.ansible.com/
https://github.com/ansible/ansible
https://docs.ansible.com/ansible/2.9/index.html

########################################################
### Reference

https://www.redhat.com/ko/technologies/management/ansible/what-is-ansible

https://wikidocs.net/book/6350

https://kim-dragon.tistory.com/13
https://kim-dragon.tistory.com/53

https://11001.tistory.com/97
https://11001.tistory.com/98

https://www.lesstif.com/ansible/ansible-22052877.html

https://booiljung.github.io/technical_articles/ansible/ansible_simple_guide.html

########################################################
### Ansible Guide

# 위의 Reference 사이트 내용을 정리 한 내용임 자세한 설명 내용은 해당 사이트 참조 

# Ansible
여러 개의 서버를 효율적으로 관리할 수 있게 해주는 환경 구성 자동화 도구
2012년에 마이클 데한 이라는 개발자가 개발한 오픈소스 소프트웨어 2015에 RedHat이 인수 
Infrastructure as a code (환경의 배포와 구성을 규격화된 코드로 정의해 사용하는 것을 의미)

	파이썬으로 개발 되었음
	앤서블은 Python Code를 호출하여 실행하기 때문에 Python이 필수적으로 필요
	원격의 컴퓨터를 구성 및 관리
	원격 컴퓨터에 에이전트를 설치할 필요 없음(ssh를 통해 원격 컴퓨터 제어)
	원격 컴퓨터에 앤서블 명령을 내리는 컴퓨터는 앤서블 코어가 설치 되어야 함 
	다수의 컴퓨터에 동시에 동일한 환경을 배포 가능 

# 앤서블 3요소 + 2요소 
* 인벤토리(inventory): 어디서 수행할 것인지 기술
	- 인벤토리는 앤서블에 의해 제어될 대상을 정의
	- 일반적으로 hosts.ini 파일에 정의해 사용 ,서버들의 SSH접근 iP, 포트, 리눅스 사용자 와 같은 접속 정보 정의

* 플레이북(Playbook): 무엇을 수행할 것인지 기술
	- 플레이북은 인벤토리 파일에서 정의한 대상들이 무엇을 수행할 것인지 정의하는 역할을하며, yaml 포맷으로 설정

* 모듈(Module) : 어떻게 수행할 것인지 기술
	- 모듈은 플레이북에서 task가 어떻게 수행될지를 정의하는 요소
	- 실제 작업을 처리하는 단위로 이 Module이라는 개념을 사용

- API 	Ansible python API 를 사용하여 노드를 제어하고, Ansible 을 확장 하여 다양한 Python 이벤트에 응답하고,
        다양한 플러그인을 작성할 수 있으며, 플러그를 연결가능

- Plugin Ansible의 핵심 기능 을 보강하는 코드 조각
		 Ansible 은 플러그인 아키텍처를 사용하여 풍부하고 유연하며 확장 가능한 기능 세트를 활성화

# 앤서블의 멱등성 
		여러번 적용해도 결과는 바뀌지 않는다.
		바뀌는 것이 없으면 당연히 배포도어도 바뀌지 않는다.
		바뀌는 부분이 있으면 그 부분만 반영된다.

		- shell, command, file module은 멱등성 제공하지 않는 모듈임 아래와 같이 멱등성 기반 으로 작성 필요
		Non-Idempotent Bash 예시
		echo“127.0.0.1 localhost”>> /etc/hosts
		→ /etc/hosts에 항상 내용을 추가함.

		Idempotent Bash 예시
		If (! grep -q 127.0.0.1 /etc/hosts); then echo“127.0.0.1 localhost”>> /etc/hosts; fi
		→ /etc/hosts에 내용이 있으면 추가하지 않음.

# 특징
	1) Provisioning : 요구에 맞게 시스템 자원을 할당/배치 해두어 즉시 사용할 수 있는 상태로 준비
	    Package/SW 설치
	    구성/설정 변경
	    File 전송, 배포

	2) Configuration : 구성,적용
	    보안 적용, 패치
	    Service 시작과 종료, 각종 Service와 Demon 관리
	    상태 파악과 확인
	    Batch 처리
	    Update 실행 

	3) Orchestration : Provisiong, Configuration 을 다 얹어서 한번에 처리 
	    서버, 네트워크, 서비스, 로드 밸런스, 방화벽 설정 및 배포, 작업 등을 자동화
	    Server / Router / Switch / FW / Load Balancer / Storage / Database / Cloud etc…

	4) Monitoring

	5) Reporting

	6) Gathering 


# Ansible Architecture
하나의 제어 노드(Server)에서 여러 관리 노드(Client) 를 관리하는 구조
Managed Node와의 통신은 SSH (Secure Shell)을 이용 

    - 제어 노드 (Control Node)
    Ansible이 설치된 모든 머신
    Python이 설치된 모든 컴퓨터를 제어 머신으로 사용 가능하며, 여러 개의 제어노드 가질 수 있음
    명령어나 Playbook 실행 가능
    /usr/bin/ansible 또는 /usr/bin/ansible-playbook를 호출하여 실행

	    * 요구사항
		Linux/Unix Host 	(Ubuntu, CentOS,SUSE, Redhat, Debian, macOS, BSD, AIX, Oracle Solaris, HPUX등)
		Python2 (2.7이상) or Python 3 (3.5이상)
		SSH (22포트)
		서버 방화벽 또는 ACL Rule 허용

		Window 	최소 PowerShell 4.0이상 & .NetFramwork 4.0 이상
		(3.0버전은 winRM memory hotfix 적용 필요)
		WinRM (5986 포트)
		서버 방화벽 또는 ACL Rule 허용
		Windows 2008 R2 SP1 이상
		WinRM 활성화

    - 관리 노드 (Managed Node)
    Ansible로 관리하는 장치들

    	* 요구사항
	    Linux 		Python2 (2.6이상) or Python 3 (3.5이상)

		Unix-like	Python 2.4 이상 Python 2.5이전 버전 사용하는 경우 python-simplejson 필요
					SSH 인 통신 방법이 필요. 기본적으로 SFTP를 사용합니다.
					사용할 수없는 경우 ansible.cfg 에서 SCP로 전환 가능.

		Window		1.7 버전부터 Ansible로 관리하는 것은 가능, Window에 Ansible을 설치해서 다른 OS 관리하는 것은 불가능

# Design Goals
    Minimal In Nature
    관리 시스템은 환경에 대한 추가 종속성을 부과해서는 안됨 

    Consistent
    Ansible을 사용하면 일관된 환경을 만들 수 있어야함 

    Secure
    Ansible은 에이전트를 노드에 배포하지 않습니다. 오직 OpenSSH와 파이썬만 관리 노드에 필요
    → (Agentless)

    Highly Reliable
    Ansible 플레이 북은 멱등성을 가지므로 관리되는 시스템에서 예기치 않은 부작용을 방지 할 수 있음
    (멱등성에 준하게 작업 작성 필요)

    Minimal learning required
    플레이 북은 YAML 및 Jinja 템플릿을 기반으로 한 쉽고 설명적인 언어를 사용


# Ansible 도입 기대효과
	□ 안전성 향상
	    휴먼 에러 방지
	    인력 의존성 탈피
	    모든 변경 이력 관리 가능
	    작업 계획과 운영 환경의 차이 감소

	□ 작업 효율 향상
	    대상 서버 수와 상관없이 구축가능, 병렬 실행
	    장시간 작업, 야간 작업에 대한 인력의존성 탈피
	    신속한 릴리즈 작업

	□ 다른 툴과 통합하여 자동화와 효율성 향상
	    버전 관리툴(git…)에 의한 순서/설정 관리
	    자동 테스트 툴에 의한 환경 테스트 (serverspec 등)
	    각종 CI(Continuos Integration)툴과의 자동 연계 (Jenkins 등)
	    모니터링 도구와 연계된 장애 대응 자동화 (zabbix, nagios 등)
	    Slack 등과 연계해 채팅 베이스에서의 운용 작업 실행

# Ansible 설치
Ansible 은 agent 가 없으므로 설치가 매우 간단하며 ansible 스크립트를 실행할 
제어 노드(Control Node) 를 제외하고는 python 설치도 필요 없음 
제어 노드는 Linux 나 Unix 만 지원하며 python 3(>= 3.5) 또는 python 2(>= 2.7) 이상 버전이 설치 필요

1) pip로 설치Link to pip로 설치
   - 의존성있는 패키지까지 자동으로 설치해주므로 개인적으로 권장하는 설치 방법으로 root 권한으로 설치
   pip install ansible

2) Ubuntu
   sudo apt install ansible

3) OS X
   brew install ansible

########################################################
### Ansible 사용 예시 

사전 준비
    Ansible설치, 1대의 Ansible 실행서버, 3대의 대상서버

작업리스트  
    1) Anslibe inventory 설정
    2) Ansible palybook 작성
    3) Ansible palybook 실행


 1) Ansible inventory 설정
 	## ansible hosts 파일 설정
	vi /etc/ansible/hosts
 	-----------------------------------------------------
	# 대상서버에 사용할 ssh key와 user 설정
	[dev:vars]
	ansible_ssh_private_key_file=/etc/ansible/DEV.pem
	ansible_user=ec2-user

	[prod:vars]
	ansible_ssh_private_key_file=/etc/ansible/PROD.pem
	ansible_user=ec2-user

	[stg:vars]
	ansible_ssh_private_key_file=/etc/ansible/STG.pem
	ansible_user=ec2-user

	# 대상서버 IP
	[dev]
	10.144.0.1

	[prod]
	10.144.0.2

	[stg]
	10.144.0.3
	-----------------------------------------------------

	## key checking false 설정 (key checking 을 false로 설정해야 인증 확인을 물어보지 않음)
	vi /etc/ansible/ansible.cfg
	-----------------------------------------------------
	[defaults] 
	host_key_checking = False
	-----------------------------------------------------

2) playbook.yaml 작성
   vi playbook.yaml
   -----------------------------------------------------
- name: set up
  hosts: prod
  become_user: root
  become: yes
  tasks:

   - name: add user
     user:
      name: security_temp
      password: $6$keA0jbN9oNtWT0UE$neES8QAPl2X64ZIEt6VHBxvCxsOuUXL/PXhCV1bTTGkdCED6NZO6A4mx6xAcAnYPI1ESn4yi9PSIjB25XvU.O.
   - name: modify visudo
     lineinfile:
      path: /etc/sudoers
      insertafter: '^root*'
      state: present
      line: "security_temp\tALL=(ALL)\tALL"
      validate: 'visudo -cf %s'
   - name: modify sshd_config
     replace:
      path: /etc/ssh/sshd_config
      regexp: 'PasswordAuthentication no'
      replace: 'PasswordAuthentication yes'
   - name: restart sshd
     service:
      name: sshd
      state: restarted

- name: set up
  hosts: dev
  become_user: root
  become: yes
  tasks:

   - name: add user
     user:
      name: security_temp
      password: $6$keA0jbN9oNtWT0UE$neES8QAPl2X64ZIEt6VHBxvCxsOuUXL/PXhCV1bTTGkdCED6NZO6A4mx6xAcAnYPI1ESn4yi9PSIjB25XvU.O.
   - name: modify visudo
     lineinfile:
      path: /etc/sudoers
      insertafter: '^root*'
      state: present
      line: "security_temp\tALL=(ALL)\tALL"
      validate: 'visudo -cf %s'
   - name: modify sshd_config
     replace:
      path: /etc/ssh/sshd_config
      regexp: 'PasswordAuthentication no'
      replace: 'PasswordAuthentication yes'
   - name: restart sshd
     service:
      name: sshd
      state: restarted

- name: set up
  hosts: stg
  become_user: root
  become: yes
  tasks:

   - name: add user
     user:
      name: security_temp
      password: $6$keA0jbN9oNtWT0UE$neES8QAPl2X64ZIEt6VHBxvCxsOuUXL/PXhCV1bTTGkdCED6NZO6A4mx6xAcAnYPI1ESn4yi9PSIjB25XvU.O.
   - name: modify visudo
     lineinfile:
      path: /etc/sudoers
      insertafter: '^root*'
      state: present
      line: "security_temp\tALL=(ALL)\tALL"
      validate: 'visudo -cf %s'
   - name: modify sshd_config
     replace:
      path: /etc/ssh/sshd_config
      regexp: 'PasswordAuthentication no'
      replace: 'PasswordAuthentication yes'
   - name: restart sshd
     service:
      name: sshd
      state: restarted
   -----------------------------------------------------

   # User의 Password는 아래와 같이 암호화를 해야함. (python3.6 version으로 암호화)
   -----------------------------------------------------
   	## python 3.6
	python3 -m pip install passlib

	## 아래 암호화 함수 실행 하여 암호화할 값 입력하면됨
	python3 -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.using(rounds=5000).hash(getpass.getpass()))"
   -----------------------------------------------------


3) Playbook 실행
  ansible-playbook playbook.yml


# RedHat계열 linux초기 설정 Playbook 예시
   -----------------------------------------------------
 tasks:
    # Asia 시간 세팅
    - name: Set OS Time
      timezone:
        name: Asia/Seoul

    # 시간 동기화
    - name: Sync OS Time
      shell: systemctl restart rsyslog
      shell: systemctl restart chronyd


    # Python3 설치
    - name: Install Python3
      yum:
        state: installed
        name: 
          - python3

    # awscli / boto3
    - name: Install Python lib
      pip:
        executable: pip3
        name:
          - awscli
          - boto3

    # 기타 패키지 설치
    - name: Install software requirements
      yum:
        state: installed
        name: 
          - telnet
   -----------------------------------------------------


