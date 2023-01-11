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
https://www.redhat.com/ko/topics/automation/learning-ansible-tutorial

https://wikidocs.net/book/6350

https://kim-dragon.tistory.com/13
https://kim-dragon.tistory.com/53

https://11001.tistory.com/97
https://11001.tistory.com/98

https://www.lesstif.com/ansible/ansible-22052877.html

https://blog.softpine.dev/tag/ansible

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
	- 호스트는  그룹에 포함시켜서 관리할 수 있음, 그룹은 하위 그룹을 포함할 수 있으며 한 호스트는 여러 그룹에 포함될 수 있음

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
		* Non-Idempotent Bash 예시
		echo “127.0.0.1 localhost”>> /etc/hosts
		→ /etc/hosts에 항상 내용을 추가함.

		* Idempotent Bash 예시
		If (! grep -q 127.0.0.1 /etc/hosts); then echo“127.0.0.1 localhost”>> /etc/hosts; fi
		→ /etc/hosts에 내용이 있으면 추가하지 않음.

		* ansible 의 모듈중 하나인 lineinfile 을 사용하면 파일에 특정 항목이 없으면 추가하고 다른 값이 있으면 변경
		- name: Ensure SELinux is set to enforcing mode
		    lineinfile:
		      path: /etc/selinux/config
		      regexp: '^SELINUX='
		      line: SELINUX=enforcing

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

	# 설치 할 앤서블 버전을 설정 가능 
	$ pip install ansible==2.10.7

2) Ubuntu
   sudo apt install ansible

3) CentOS
   um install -y ansible 

4) OS X
   brew install ansible

# Ansible 설치 확인
  
  * Inventory 파일 생성
	ansible 이 관리하는 서버의 정보를 담은 파일을 Inventory 파일이라고 하며 인벤토리는 세 가지 방법으로 지정
	적용 우선 순위는 3,2,1 순 여러개 지정 되어 있으면 우선 순위 높은 것 하나만 적용 됨 
	1번은 root 권한이 필요하며 유연성이 떨어지므로 잘 사용하지 않음 

	1) /etc/ansible/hosts(기본 설정)
	   mkdir /etc/ansible/
	   echo "127.0.0.1" > /etc/ansible/hosts

	2) ANSIBLE_INVENTORY 환경 변수에 설정
	   echo "127.0.0.1" > ~/.ansible/inventory.ini
	   export ANSIBLE_INVENTORY=~/.ansible/inventory.ini

	3) ansible 실행시 -i 옵션과 파일 경로 지정
	   ansible all -m ping  -k -i ~/my_ansible_hosts


	Inventory는 확장자 명이 따로 없음
	파일 이름은 자유롭게 지정 가능, 추후 관리를 위해 Inventory파일은 따로 Inventory라고 명시해 주시는 것이 좋음 
	yaml 파일로도 작성 가능 

    https://docs.ansible.com/ansible/2.9/user_guide/intro_inventory.html#id17

	* 일반적인 설정 방법 
    ----------------------------------
    //이렇게 하면 Ad-hoc이나 Playbook에서 해당 호스트 네임으로 명령어 실행가능
    mail.example.com	

	//호스트 네임과 IP주소로 설정 가능
	[webservers]
	192.168.0.5
	192.168.0.6

	[dbservers]
	one.example.com
	two.example.com
	three.example.com

    //기본 ssh포트를 사용하지 않는다면 이런식으로도 설정 가능
	[nossh]
	nossh.example.com:5050	

    // for문 처리를 통해 01 ~ 50 번까지의 이름을 묶을 수 있음 
	[webservers2]
	www[01:50].example.com	

    //알파벳 또한 가능
	[databases2]
	db-[a:f].example.com	

	//호스트 변수
	[atlanta]
	host1 http_port=80 maxRequestsPerChild=808	

	//해당 호스트 네임에 이런식으로 정의
	host2 http_port=303 maxRequestsPerChild=909 

	//호스트들을 atlanta라는 묶음으로 사용
	//그룹 변수
	[atlanta]
	host1
	host2

    //그룹 자체를 기준으로 변수를 부여
	[atlanta:vars]	
	ntp_server=ntp.atlanta.example.com
	proxy=proxy.atlanta.example.com

	//그룹 그룹 묶기
	[atlanta]
	host1
	host2

	[raleigh]
	host2
	host3

    // :children 키워드를 이용해 그룹끼리 묶을 수 있음
	[southeast:children]
	atlanta
	raleigh

	* yaml 형식 설정
	----------------------------------
    all:
	  hosts:
	    webserver-host1:
	    webserver-host2:
	    dbserver-host[1:10]:
	    10.1.3.2:

	  children:
	    webservers:
	      hosts:
	        webserver-host1:
	          host_var: "local_var"
	        webserver-host2:

	    dbservers:
	      hosts:
	        dbserver-host[1:10]:
	      vars:
	        db_id: "admin"
	        db_passwd: "passw@rd"

	  vars:
	    global_var: "server_name"

 
	* 설정 example 
	----------------------------------
	[all:vars]
	ansible_connection=ssh
	ansible_port=22
	ansible_ssh_user=ec2-user
	ansible_ssh_private_key_file=~/aws-key/keyfile.pem
	#ansible_python_interpreter=/usr/bin/python

	[all]
	localhost       ansible_host=127.0.0.1          ip=127.0.0.1
	web1            ansible_host=192.168.0.51       ip=192.168.0.51
	web2            ansible_host=192.168.0.52       ip=192.168.0.52
	web3            ansible_host=192.168.0.53       ip=192.168.0.53
	web4            ansible_host=192.168.0.54       ip=192.168.0.54
	web5            ansible_host=192.168.0.55       ip=192.168.0.55
	db1             ansible_host=192.168.0.201      ip=192.168.0.201
	db2             ansible_host=192.168.0.202      ip=192.168.0.202
	api1            ansible_host=192.168.0.61       ip=192.168.0.61
	api2            ansible_host=192.168.0.62       ip=192.168.0.62
	redis1          ansible_host=192.168.0.211      ip=192.168.0.211
	redis2          ansible_host=192.168.0.212      ip=192.168.0.212
	redis3          ansible_host=192.168.0.213      ip=192.168.0.213
	was1            ansible_host=192.168.0.81       ip=192.168.0.81
	was2            ansible_host=192.168.0.82       ip=192.168.0.82
	was3            ansible_host=192.168.0.83       ip=192.168.0.83
	was4            ansible_host=192.168.0.84       ip=192.168.0.84
	was5            ansible_host=192.168.0.85       ip=192.168.0.85

	[local]
	localhost

	[webgroups:children]
	web
	was
	api

	[dbgroups:children]
	db
	redis

	[web]
	web[1:5]

	[was]
	was[1:5]

	[db]
	db1
	db2

	[api]
	api1
	api2

	[redis]
	redis1
	redis2
	redis3

	* ssh key파일 연결 설정 example
	----------------------------------
	[dev:vars]
	ansible_ssh_user=ubuntu
	ansible_ssh_private_key_file=/root/cloud/key/ssh-key-2021-03-17.key

	[prod:vars]
	ansible_ssh_user=ubuntu
	ansible_ssh_private_key_file=/root/cloud/key/ssh-key-2021-02-23.key

	[dev]
	193.123.255.92

	[prod]
	193.123.252.22

    ----------------------------------

    # 설정 내용 확인 
    ansible-inventory -i inventory.ini --graph
    ansible-inventory -i inventory.ini --list


  * Config 파일 생성
    ansible 동작을 제어하는 설정 파일은 /etc/ansible/ansible.cfg  이며  ANSIBLE_CONFIG 환경 변수를 통해 설정

    export ANSIBLE_CONFIG=~/.ansible/ansible.cfg


    구성 파일 설정
	Ansible 구성 파일은 [section] 대괄호로 묶여진 여러 섹션(분류) 구성 
	각 섹션에는 키 = 값 으로 설정된 설정이 포함

	[defaults]
	inventory = ./inventory.ini
	remote_user = ubuntu
	ask_pass = false
	#log_path = /tmp/ansible.log

	[privilege_escalation]
	become = true
	become_method = sudo
	become_user = root
	become_ask_pass = false

	[ssh_connection]
	ssh_args = -C -o ControlMaster=auto -o ControlPersist=120s
	control_path_dir = ~/.ansible/cp
	pipelining = True

	[persistent_connection]
	connect_timeout = 30
	command_timeout = 30


	[default] 섹션
	    inventory: 인벤토리 파일의 위치 (기본: /etc/ansible/ansible.cfg)
	    		   인벤토리 파일은 콤마를 이용하여 여러 개를 지정할 수 도 있음 
	    remote_user: SSH 인증하기 위한 사용자 (기본: 현재 사용자)
	    ask_pass: SSH 인증하기 위한 패스워드 요청/입력 여부 (기본: false)
	 
	[privilege_escalation] 섹션
	    become: 권한 상승 여부 (기본: false)
	    become_method: 권한 상승 방법 (기본: sudo)
	    become_user: 권한 상승할 사용자 (기본: root)
	    become_ask_pass: 권한 상승 방법의 패스워드 요청/입력 여부 (기본: false)

	구성 파일 설정 확인
	ansible-config view


  * 설정 내용 확인 
    ansible --version

  * 설치 확인
    ansible 의 정상 설치 여부는 ansible ad-hoc 명령어를 사용해서 확인

   ansible all -m ping -k

	localhost | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}

    127.0.0.1 | FAILED => No authentication methods available 와 같은 에러가 난다면 
    실행시에 암호 프롬프트를 띄우는 옵션인  -k, --ask-pass 옵션을 추가

########################################################
### Ansible Usage

# ansible ad-hoc 명령어 사용법
  ansible adhoc 명령은 playbook 을 작성하지 않고 command-line 에서 직접 앤서블 모듈을 호출해서 실행하는 방식
  호스트 패턴을 입력하고 -m 옵션 뒤에 사용할 모듈을 지정하며 [  ] 안에 있는 내용은 옵션
  -m 옵션으로 모듈을 지정하지 않을 경우 ansible 은 임의의 명령을 실행할 수 있는 모듈인 command 모듈을 사용

  ansible host-pattern -m module [-a 'module options'] [-i inventory] [u -username]

  * ping module
    서버에서 python 모듈 실행 여부를 확인할 수 있는 ping 모듈 (TCP ping이 아님)
    ansible all -m ping

  * setup module
    ansible fact 를 수집하는 setup 모듈
    인벤토리에 있는 서버의 모든 fact 를 수집해서 출력
    ansible all -m setup

    출력되는 내용이 많을 경우 필터 옵션 사용 가능 
    ansible all -m setup -a "filter=ansible_dist*"

  * shell 실행 
    ansible ... -m shell -a "<셸 명령줄>" ...
    ansible all -m shell -a "uname -a"


### inventory ###
인벤토리는 앤서블을 이용하여 작업을 진행할 서버의 정보와 작업에 사용할 변수 정보를 저장
ini 파일과 yaml 파일로 설정할 수 있음 

all 에는 모든 호스트의 정보를 추가
children에는 그룹별 호스트 정보를 추가
    그룹은 작업 단위별로 설정하는 것이 좋음
    db 서버 모음, 웹 서버 모음과 같은 형태
dbserver-host[1:10]과 같은 형태로 1~10 번까지의 호스트 설정 가능
10.1.3.2와 같은 형태로 IP를 직접적으로 설정하는 것도 가능


 # ini 파일
 	mail.example.com

	[webservers]
	foo.example.com 
	bar.example.com

	[dbservers]
	one.example.com
	two.example.com
	three.example.com
 
# yaml 파일
	all:
	  hosts:
	    mail.example.com:
	  children:
	    webservers:
	      hosts:
	        foo.example.com:
	        bar.example.com:
	    dbservers:
	      hosts:
	        one.example.com:
	        two.example.com:
	        three.example.com:

# 파라미터 전달
  -i 옵션으로 인벤토리 파일을 지정 할 수 있음 
  ansible-playbook -i inventory.yaml

# cfg 파일 설정
  cfg 파일에 inventory 옵션에 파일 경로를 지정, 인벤토리 파일은 콤마를 이용하여 여러 개를 지정
  [default]
  inventory = inventory.yaml,common.yaml


# 변수 타입
    문자열
    숫자
    불린(boolean)
    작업리스트
    딕셔너리(dict)

# 변수 예제
vars:
  string_var: "A"
  number_var: 1
  boolean_var: "yes"
  list_var:
    - A
    - B
    - C
  dict_var:
      key_a: "val_a"
      key_b: "val_b"
      key_c: "val_c"

# 전역 변수
  다음 값은 전역변수 입니다. 모든 서버에서 사용할 수 있는 변수값입니다.
  vars:
    global_var: "server_name"

# 그룹 변수
  그룹에서 사용할 수 있는 변수값입니다. dbservers 그룹에서 다음의 변수들을 사용할 수 있습니다.
    dbservers:
      hosts:
        dbserver-host[1:10]:
      vars:
        db_id: "admin"
        db_passwd: "passw@rd"

# 호스트 변수
  단일 호스트에서 사용할 수 있는 변수도 설정할 수 있습니다. webserver-host에서 host_var 변수를 사용할 수 있습니다. 
  children:
    webservers:
      hosts:
        webserver-host1:
          host_var: "local_var"


### module ###
모듈은 단일 명령어 이자 수행할 작업

  모듈에서 사용 할 수 있는 공통 파라미터 
	ignore_errors: yes
	    오류 무시
	become: yes
	    root 권한으로 실행
	when:
	    조건문 분기
	tags
	    실행 태그 추가
	environment
	    실행 환경 변수 설정

  ex) hiveserver2 실행 정지 명령 
	- name: hiveserver2 stop  
	  shell: systemctl stop hiveserver2  
	  when: hive_installed.stat.exists  
	  become: yes  
	  tags:  
    - stop
	  environment:
	    http_proxy: "http://proxy_host:proxy_port"

  # file 
    디렉토리, 파일과 관련된 처리를 할 때 사용하는 모듈 복사, 삭제, 권한 변경 등을 처리

    * 권한 설정
      mode는 파일의 권한, 모드를 설정 recurse를 이용하여 하위의 모든 디렉토리 권한을 함께 변경
      ex)
	  	- name: "copy and Mode 755"
		  file:
		    src: "/home/file-a"
		    dest: "/home/file-b"
		    mode: 0755
		    owner: "deploy"
		    group: "deploy"
		  become: yes

    * 링크 생성
      state: link를 이용하여 링크를 생성
      ex)
      	- name: "link file-a"
		  file:
		    src: "/home/file-a"
		    dest: "/home/link-file-a"
		    state: link
		    owner: "deploy"
		    group: "deploy"
		  become: yes

    * 파일 삭제
      state: absent를 이용하여 파일을 삭제
      ex)
      	- name: "delete file-a"
		  file:
		    path: "/home/file-a"
		    state: absent

	* 디렉토리 생성
	  state: directory를 이용하여 디렉토리를 생성
	  ex)
	 	 - name: "make directory dirA"
		  file:
		    path: "/home/dirA"
		    state: directory

	* 0 Byte 파일
	  state: touch를 이용하여 파일을 생성
	  ex)
	  	- name: "touch file"
		  file:
		    path: "/home/file-c"
		    state: touch

  # unarchive 
	압축을 해제
	ex)
	- name: unarchive java file 
	  unarchive:
	    src: "OpenJDK8U-jdk_x64_linux_hotspot_8u262b10.tar.gz"
	    dest: "/usr/lib"
	    remote_src: True
	  become: True
  
  # copy 
    copy는 앤서블을 실행하는 서버(제어 노드)의 파일을 원격 서버(매니지드 노드)로 복사

    * 로컬 복사
      src를 dest 위치로 복사 src는 앤서블이 실행되는 서버의 파일 아래와 같은 경우 scp명령어로 복사하는 것과 동일 처리 
      ex)
     	 - name: "copy file"
		  copy:
		    src: "file-a"
		    dest: "/home/file-a"

	* 원격 복사
	  remote_src가 yes이면 원격 서버의 파일을 복사 become: yes로 설정하면 root 권한으로 파일을 복사
	  ex)
	  	- name: "copy file"
		  copy:
		    src: "/home/file-a"
		    dest: "/home/file-b"
		  remote_src: yes
		  become: yes

	* with_items를 이용한 리스트 복사
	  ex)
	  	- name: copy airflow configs  
		  copy:  
		    src: "{{ item }}"  
		   dest: "/opt/airflow/{{ item }}"  
		  with_items:  
		    - airflow.cfg

  # template 
    template은 파이썬의 jinja2 템플릿을 이용하여 파일을 변환하면서 복사
    jinja2 템플릿은 인벤토리에서 전달 된 변수와 서버의 정보를 이용하여 설정 파일 등을 호스트에 맞게 변환
    https://jinja.palletsprojects.com/en/3.0.x/templates/#escaping
    ex)
    - name: copy airflow configs  
	  template:  
	    src: "{{ item }}"  
	    dest: "/opt/airflow/{{ item }}"  
	    with_items:  
	    - airflow.cfg

	* for 문 처리
	  {% %} 안에서 루프를 이용하여 값을 변환
	  ex)
	  all:
		  hosts:
		  children:
		    tomcat:
		      sample-url-1:
		      sample-url-2:

		  vars:
		    host_ip:
		      - "192.168.0.1"
		      - "192.168.0.2"

	  	{% set port = '1234' %}
		{% set server_ip = [] %}

		# vars 정보를 이용
		{% for ip in host_ip  %}
		{{ server_ip.append( ip+":"+port ) }}
		{% endfor %}

		# 그룹 정보를 이용
		{% for ip in groups['tomcat']  %}
		{{ server_ip.append( ip+":"+port ) }}
		{% endfor %}

  # get_url 
    get_url은 파일을 다운로드 wget 명령과 동일
    프록시 설정을 이용해야 하는 경우 environment 를 이용하여 설정
    ex)
    - name: download hive file  
	    get_url:  
	      url: "http://file-url/hive.tgz"
	      dest: /home/user/hive.tgz
	      mode: '0660'
	    environment:  
	      https_proxy: http://proxy_url:proxy_port

  # stat 
    stat은 파일의 상태를 확인 register를 이용해서 파일 파이즈, 존재 여부, 권한 등의 상태를 확인하고 다음 처리에 이용

    * 사이즈 확인
      ex)file.txt 의 사이즈를 확인하고 0이면 파일을 다운로드 
      	- stat:  
		    path: "/home/user/file.txt"  
		  register: st  

		- name: download file.txt
		    get_url:  
		      url: "http://file.txt"  
		    dest: /home/user
		      mode: '0660'  
		    when: "st.stat.size = 0 | int"

  # shell 
    쉘 명령어를 실행

    * 단일 명령어 실행
      ex)
      	# mysql 명령어를 이용하여 쿼리를 실행 
		- name: execute drop_db query  
		    shell: |  
		      mysql -u root < /tmp/drop_db.sql  
		    become: yes

		# airflow-start.sh 스크립트를 실행 
		# when: 을 이용하여 조건문을 처리 
		- name: airflow start  
		    shell: /opt/airflow/bin/airflow-start.sh all  
		    when: airflow_installed.stat.exists

	 * 명령어 실행 결과 반환
	   register를 이용하여 명령어 실행 결과를 저장
	   ex)
	    # OS 이름을 저장합니다. 
		- name: Check os Name  
		    shell: cat /etc/os-release | egrep ^NAME | awk -F "=" '{ print $2 }' | sed s/\"//g  
		    register: os_name

     * 멀티라인 명령어
       |를 이용하여 여러 명령어를 한번에 실행
       ex)
      	# systemctl 명령어를 실행 
		- name: "service systemctl"  
		    shell: |  
		      systemctl daemon-reload  
		      systemctl enable mysql  
		    become: yes

  # apt 
    우분투의 apt 설치 명령어를 실행

    * 설치
      ex)
      	# 마리아 db 서버 설치 
		- name: setup db  
		    apt:  
		      name: mariadb-server  
		    become: yes

    * 제거
      state: absent 옵션을 이용하여 제거
      ex)
      	# 마리아 db 서버 제거 
		- name: delete db  
		    apt:  
		      name: mariadb-server  
		      state: absent
		    become: yes

	* 업데이트 및 설치
	  update_cache: yes 를 이용하여 apt를 업데이트 하면서 패키지를 설치
	  ex)
	  	# 마리아 db 서버 설치 
		- name: setup db  
		  apt:  
		    name: mariadb-server  
		    update_cache: yes
		  become: yes

  # systemd
    systemctl 명령어를 이용하여 서비스를 실행
    ex)
    # 마리아 db 재시작 
	- name: restart mariadb  
	  systemd:  
	    state: restarted  
	    name: mariadb  
	  become: yes

  # debug 
    디버그용 문자열을 출력
    ex) java -version 명령어 실행결과를 java_result에 저장하고, when 상태에 따라 debug로 출력
    - name: Install Java
	  hosts: NEW_VM
	  tasks:
	  - name: Check if java is installed
	    command: java -version
	    register: java_result
	    ignore_errors: True

	  - debug:
	      msg: "Failed - Java is not installed"
	    when: java_result is failed

	  - debug:
	      msg: "Success - Java is installed  {{ java_result.stdout }}"
	    when:  java_result is success

  # user
	사용자 추가
	ansible ... -m user -"name=<사용자 이름>" -k

	마지막 추가된 사용자 확인
	ansible ... -a "tail -n 1 /etc/passwd" -k

	사용자 제거
	ansible ... -m user - "name=<사용자 이름> state=absent" -k


  # package
	패키지 설치
	ansible ... -m <yum> -a "name=httpd state=present" -k
	ansible ... -m <apt> -a "pkg=httpd update_cache=yes state=present" -k

	패키지 제거
	ansible ... -m <yum> -a "name=httpd state=absent" -k
	ansible ... -m <apt> -a "name=httpd autoremove=yes state=absent" -k

  # service
    서비스 시작, 중지, 재시작.
	ansible ... -m service -a "name=<서비스 이름> state=<상태>"

	서비스 상태
	    started: 시작
	    stop: 중지
	    restarted: 재시작


### task ###
태스크는 모듈의 모음 연속된 동작으로 진행할 작업을 정의 
  ex) hue 4.10.0 버전의 압축 파일을 다운로드 하고, 압축을 해제한 후, 링크를 거는 태스크
	- name: download hue file
	  get_url:
	    url: "https://cdn.gethue.com/downloads/hue-4.10.0.tgz"
	    dest: /tmp
	    mode: '0660'

	- name: unarchive hue file
	  unarchive:
	    src: "/tmp/hue-4.10.0.tgz"
	    dest: "/opt"
	    remote_src: True
	    owner: "hue"
	    group: "hue"
	  become: True

	- name: "create symlink /opt/hue"
	  file:
	    src: "/opt/hue-4.10.0"
	    dest: /opt/hue
	    state: link
	    owner: "hue"
	    group: "hue"
	  become: yes

  # 태스크 불러오기 
    import_tasks, include_tasks를 이용해서 다른 파일의 태스크를 불러올 수 있음 
    ex)
    - name: configure NameNode HA
	  when: "groups['hadoop_namenodes'] | length > 1"
	  import_tasks: ./configure_hadoop_ha.yaml

### playbook ###	  
플레이북은 호스트와 태스크를 연결 

    앤서블 애드혹은 1회성이다.
    앤서블 플레이북은 YAML로 작성한다.
    반복적으로 사용가능하다.
    플레이북 자체가 문서다.

ansible-playbook <앤서플 플레이북 파일.yml> -k

--- # 첫줄은 반드시 ---로 시작해야 한다.
- name: <이 플레이북에 대한 설명>
  hosts: <호스트 지정>
  gather_facts: <yes 또는 no> 
  become: <아래 작업들에 대해 수퍼 유저 권한을 사용하려면 yes 아니면 no>
  
  tasks:
    <이하 작업 내용들 기술>


# inventory.yaml의 호스트로 playbook.yaml을 실행
ansible-playbook -i inventory.yaml playbook.yaml

* inventory.yaml
all:  
  hosts:  
    hue-server:  

  children:  
    hue:  
      hosts:  
        hue-server:

* playbook.yaml
- hosts: "hue"

- name: download hue file
  get_url:
    url: "https://cdn.gethue.com/downloads/hue-4.10.0.tgz"
    dest: /tmp
    mode: '0660'

- name: unarchive hue file
  unarchive:
    src: "/tmp/hue-4.10.0.tgz"
    dest: "/opt"
    remote_src: True
    owner: "hue"
    group: "hue"
  become: True

- name: "create symlink /opt/hue"
  file:
    src: "/opt/hue-4.10.0"
    dest: /opt/hue
    state: link
    owner: "hue"
    group: "hue"
  become: yes        


 # Role 
   태스크(Task)를 묶어서 롤(Role)로 만들어 놓고 반복사용하는 형태로 구성
   roles 폴더 내부에 있으면 자동으로 읽어서 사용

   구성요소
    files
        정적 파일용 디렉토리
    handlers
        핸들러를 구성하는 디렉토리
    meta
        메타 정보를 가지는 파일 생성
        작성자, 라이센스 등의 정보 추가
    task
        실제로 작업할 내용
    templates
        JinJa2 템플릿으로 변환하여 처리할 작업 파일
        설정 파일 등
    vars
        변수 추가

    ex)
    - hosts: all
	  roles:  
	  # ----------- install ---------------  
	    - role: "install/java"  
	      when: java_installed.rc != 0  
	      tags:  
	        - master

 # when 
   특정 상황에서만 작업을 처리할 수 있게 하기 위해서 조건문을 이용	        
   ex)pre_tasks로 하둡 설치 여부를 확인해서 hadoop fs 명령을 실행 	
   	 pre_tasks:  
	  - name: Check hadoop Installed  
	    stat:  
	      path: /usr/bin/hadoop
	    register: hadoop_installed  

	 tasks:
	  - name: run ls
	    shell: hadoop fs -ls hdfs:/// | wc -l
	    register: hadoop_command_result
	    when: hadoop_installed.stat.exists

 # Tag 
   플레이북은 태그를 이용하여 필요한 작업만 처리하거나, 제외할 수 있음
   태그는 태스크, 플레이북, 롤 등 어디에나 추가 가능 

   * 특수 태그
    always
        항상 실행
    never
        별도로 tag 명령을 실행해야 실행
    tasks:
	- debug:
	    msg: "Always  runs"
	  tags:
	  - always


	tasks:
     - debug: msg="{{ showmevar }}"
	    tags: [ never, debug ]

	ex)
	tasks:
	- yum:
	    name:
	    - httpd
	    - memcached
	    state: present
	  tags:
	  - packages

	- template:
	    src: templates/src.j2
	    dest: /etc/foo.conf
	  tags:
	  - configuration

	roles:
	  - role: webserver
	    vars:
	      port: 5000
	    tags: [ web, foo ]


	실행 
	ansible-playbook example.yml --tags "configuration,packages"
	ansible-playbook example.yml --skip-tags "packages"

 # with_items 
   동일한 작업을 여러번 반복하고 싶을 때 with_items 를 이용 리스트나 맵형식의 값을 이용하여 같은 작업을 반복

   * 리스트 형식
   ex) fileA, fileB를 /etc/hadoop/conf 아래로 복사
	   - file:
	    src: '{{ item }}'
	    dest: '/etc/hadoop/conf/'
	  with_items:
	    - fileA
	    - fileB

   * 맵 형식
   ex) 맵 형식의 데이터를 이용하여 반복 item의 키를 이용하여 값을 처리
	   - file:
	    src: '/opt/{{ item.src }}'
	    dest: '{{ item.dest }}'
	    state: link
	  with_items:
	    - { src: 'x', dest: 'y' }
	    - { src: 'z', dest: 'k' }


## playbook usage ##

* facts를 수집하여 json으로 저장
---
- name: <이름>
  hosts: <노드>
  
tasks:
  - name: <이름>
    setup:
    register: facts
    
  - name: save facts
    local_action:
      module: copy
      content: ""
      dest: ./_facts_by_collector.txt

* facts를 출력
  - name: debug by msg
    debug:
      msg:
        - "eth0's ip "
        - "eth1's ip "

* 작업 include
---
- name: <이름>
  hosts: <노드들>
  become: yes
  taks:
    - include_taks: <서브 작업 파일>

* 조건에 의한 작업 수행:
tasks:
  - name: <이름>
    <뭔가 함>
    when: ansible_distribution == 'CentOS'
    또는
    when: ansible_distribution == 'Ubuntu'

* 배포판 조건에 의한 작업 include
---
- name: <이름>
  hosts: <노드>
  vars:
    linux_distribution: "centos
                       else 'ubuntu' if ansible_distribution == 'Ubuntu'
                       else 'unknown' }}"
  
tasks:
  - name: <이름>
    include_tasks: ".yml"
    
* 유저 전환
수퍼 유저로 전환
  tasks:
    - name: <이름>
      become: <수퍼 유저 권한을 사용하려면 yes 아니면 no>

지정한 유저로 전환
  tasks:
    - name: <이름>
      become: <유저 권한을 사용하려면 yes 아니면 no>
      become_user: <유저 이름>
    
* 셸명령 실행
  tasks:
    - name: <이름>
      shell: ""
      with_items:
        - "<명령줄 1>"
        - "<명령줄 2>"
        - ...
      
* 텍스트 파일 멱등성 라인 추가
  tasks:
    - name: <이름>
      lineinfile:
        path: <파일경로>
        line: ""
      with_items:
        - "<라인 1>"
        - "<라인 2>"
        - ...

* 파일 내에서 검색하여 치환
  tasks:
    - name: <이름>
      replace:
        path: <파일경로>
        regexp: <검색할 정규식>
        replace: <치환할 텍스트>

* 다수의 검색어 치환
  tasks:
    - name: <이름>
      replace:
        path: ""
        regexp: ""
        replace: ""
      with_items:
        - { path: "<경로1>", regexp: "<검색 정규식1>", replace: "치환문자열1" }
        - { path: "<경로2>", regexp: "<검색 정규식2>", replace: "치환문자열2" }

* 호스트 추가
  tasks:
    - name: <이름>
      blockinfile:
        path: /etc/ansible/hosts
        block: |
          [<그룹 이름>]
          <호스트1>
          <호스트2>
          ...
          <호스트n-1>
          <호스트n>

* SSH 키 생성
  tasks:
    - name: <이름>
      become: yes
      become_user: <유저이름>
      shell: ""
      with_items:
        - "ssh-keyscan <IP주소> >> ~/.ssh/known_hosts"
        - "ssh-keyscan <IP주소> >> ~/.ssh/known_hosts"
        ...
        - "ssh-keyscan <IP주소> >> ~/.ssh/known_hosts"
        - "ssh-keyscan <IP주소> >> ~/.ssh/known_hosts"
          
* 패키지 설치
레드헷:
  tasks:
    - name: <이름>
      yum:
        name: <패키지 이름>
        status: <present 또는 latest>

레드헷 다수의 패키지 설치:
  tasks:
    - name: <이름>
      yum:
        name: " "
        status: <present 또는 latest>
        update_cache: true
      with_items:
        - "<패키지 이름 1>"
        - "<패키지 이름 2>"
        - ...
        - "<패키지 이름 n-1>"
        - "<패키지 이름 n>"

우분투:
  tasks:
    - name: <이름>
      apt:
        pkg: ""
        state: <present 또는 latest>          
        update_cache: true
      with_items:
        - "<패키지 이름 1>"
        - "<패키지 이름 2>"
        - ...
        - "<패키지 이름 n-1>"
        - "<패키지 이름 n>"

윈도우:
  tasks:
    - name: <이름>
      win_chocolatey:
        name: <패키지 이름>

공통:
  tasks:
    - name: <이름>
      action: " name=<패키지이름> state=<present 또는 latest>"

진행 상태 표시 패키지 설치:
  tasks
    - name: Install ...
      become: yes
      apt:
        name: <package-name>
      async: 1000
      poll: 0
      register: apt_sleeper

   - name: 'apt - check on async task'
     async_status:
       jid: ""
     register: job_result
     until: job_result.finished
     retries: 1000

* 패키지 제거
레드햇:
  tasks:
    - name: <이름>
      yum:
        name: <패키지 이름>
        status: absent

우분투:
  tasks:
    - name: <이름>
      apt:
        pkg: <패키지 이름>
        status: absent
        autoremove: yes

공통:
  tasks:
    - name: <이름>
      action: " name=<패키지이름> state=absent"

* Docker 우분투 gpg 키 등록
  tasks:
    - name: Add docker apt signing key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present
      
* git 저장소 클론
  tasks:
    - name: <이름>
      git:
        repo: <git 저장소 URL>
        dest: <로컬 컴퓨터의 경로>

* cmake로 빌드
  tasks:
    - name: <이름>
      cmake:
        binary_dir: <빌드 디렉토리, 필수>
        source_dir: <소스 디렉토리>
        build_type: <빌드 타입, 기본 Debug>
        target: <빌드 타겟>
        cache_vars: <캐시 변수>

* .bashrc 구성
  tasks:
    - name: <이름>
      lineinfile:
        path: /home/.../.bashrc
        line: "
      with_items:
        - "..."
        - "..."

* 앤서블 서버에서 노드로 파일 복사하기
  tasks:
    - name: <이름>
      copy:
        src: "<앤서블 서버에서 원본 파일 경로>"
        dest: "<앤서블 노드에서 대상 파일 경로>"
        owner: <파일 소유 유저 지정>
        group: <파일 그룹 지정>
        mode: <파일 권한 지정>
        
* 앤서블 노드에서 앤서블 서버로 파일 복사해오기
  tasks:
    - name: <이름>
      fetch:
        src: "<앤서블 노드에서 원본 파일 경로>"
        dest: "<앤서블 서버에서 대상 파일 경로>"

  다수의 앤서블 노드에서 파일을 복사해 오므로 <dest 경로>/<앤서블 노드 이름>/<src 경로> 폴더에 저장 
     
  경로 단순화 : flat을 true로 지정하여 앤서블 노드의 경로를 제거 
    tasks:
    - name: <이름>
      fetch:
        src: "<앤서블 노드에서 원본 파일 경로>"
        dest: "<앤서블 서버에서 대상 디렉토리>/-파일명.확장자"
        flat: true

* 파일 내용 표시
  tasks:
    - name: <이름>
      debug:
        var: item
      with_file:
        - "<앤서블 노드에서 파일 경로 1>"
        - "<앤서블 노드에서 파일 경로 2>"
        ...

* 디렉토리 만들기
리눅스:
  tasks:
    - name: <이름>
      file:
        state: directory
        path: <만들 디렉토리 경로>
        recurse: <기본값은 no이며 깊은 디렉토리를 만드러면 yes를 지정>

윈도우:
  tasks:
    - name: <이름>
      win_file:
        state: directory
        path: <만들 디렉토리 경로>
        recurse: <기본값은 no이며 깊은 디렉토리를 만드러면 yes를 지정>

* 다수의 디렉토리 만들기
 /tmp 폴더에 test-00 부터 test-99까지 100개의 하위 디렉토리를 만든다.

  tasks:
    - name: <이름>
      file:
        state: directory
        path: "/tmp/test-"
        recurse: <기본값은 no이며 깊은 디렉토리를 만드러면 yes를 지정>
      with_sequence:
        start: 0
        end: 99
        format: %02d

 /tmp 폴더에 지정한 다수의 디렉토리를 만든다.
  tasks:
    - name: <이름>
      file:
        state: directory
        path: "/tmp/test-"
        recurse: <기본값은 no이며 깊은 디렉토리를 만드러면 yes를 지정>
      with_items:
        - foo
        - bar
        - baz

* 심볼릭 링크 만들기
  tasks:
    - name: <이름>
      file:
        state: link다
        src: <원본 경로>
        path: <심볼링 링크 경로>

* 셸에서 디렉토리 만들기
  tasks:
    - name: <이름>
      shell: ""
      with_items:
        - "mkdir -p <생성할 디렉토리 이름>"
        
* 파일 삭제
  tasks:
    - name: <이름>
      file:
        state: absent
        path: <앤서블 노드에서 파일 경로>

* 디렉토리 보기
  tasks:
    - name: <이름>
      command: ls -lht <경로>
      register: <결과를 저장할 변수명>
    - name: <이름>
      debug:
        msg: ""
        
* URL을 통해 파일 가져오기
리눅스:
  tasks:
    - name: <이름>
      get_url:
        url: "<원본 URL>"
        dest: "<대상 로컬 경로>"
        mode: <파일 권한 지정>

리눅스 파일 권한:
    0<owner><group><other> 순서로 지정.
    r: 4, w: 2, x: 1
    읽고 쓰고 실행 가능: 7
    읽기만 가능: 4
    읽고 쓰기만 가능: 6
    읽고 실행만 가능: 5

윈도우:
  tasks:
    - name: <이름>
      win_get_url:
        url: "<원본 URL>"
        dest: "<대상 로컬 경로>"
        
* 윈도우 서비스 등록
    먼저 nssm이 설치되어 있어야 한다.
  tasks:https://kim-dragon.tistory.com/53
    - name: <이름>
      win_nssm:
        name: <애플리케이션 이름>
        application: <애플리케이션 실행파일 경로>
        state: present

* 타임존 변경
리눅스:
tasks:
    - name: <이름>
      timezone:
        name: Aisa/Seoul

리눅스 타임존 목록:
timedatectl list-timezones

윈도우:
tasks:
    - name: <이름>
      win_timezone:
        timezone: 'Korea Standard Time'
        
* 마운트
리눅스:
  tasks:
    - name: <이름>
      become: yes
      mount:
        path: <마운트 할 대상 경로>
        src: <마운트 할 소스 경로>
        fstype: <파일 시스템 타입>
        opts: <파일 시스템 옵션>
        state: mounted

윈도우 NFS 마운트:
  tasks:
    - name: <설명 1>
      win_feature:
        name: NFS-Client
        state: present
    - name: <설명 2>
      win_command: net use "드라이브 문자:" "NFS 경로"

* 파일 압축 풀기
윈도우:
  tasks:
    - name: <이름>
      win_unzip:
        src: <압축된 파일 소스 경로>
        dst: <풀어진 파일 저장 경로>
        delete_archive: <압축 파일을 삭제하려면 yes 아니면 no>

* 서비스 시작
리눅스:
  tasks:
    - name: <이름>
      service:
        name: <서비스 이름>
        state: started

윈도우:
  tasks:
    - name: <이름>
      win_service:
        name: <서비스 이름>
        state: started
        
* 서비스 중지
리눅스:
  tasks:
    - name: <이름>
      service:
        name: <서비스 이름>
        state: stop

윈도우:
  tasks:
    - name: <이름>
      win_service:
        name: <서비스 이름>
        state: stop

* 서비스 재시작
리눅스:
  tasks:
    - name: <이름>
      service:
        name: <서비스 이름>
        state: restarted

윈도우:
  tasks:
    - name: <이름>
      win_service:
        name: <서비스 이름>
        state: restarted
        
* 리부팅
윈도우:
  tasks:
    - name: <이름>
      win_reboot:

* 서비스 데몬 재시작
  tasks:
    - name: <이름>
      systemd:
        state: restarted
        daemon_reload: yes
        name: tftp

* 핸들러
    전 단계가 정상적으로 이루어진 경우에 동작
    변경 사항이 발생한 경우에만 동작

핸들러:
handlers:
  - name: <핸들러 이름>
    <Do something>

핸들러 실행을 요청:
- name: <이름>
  <Do something>
  notify: <핸들러 이름>
        
       
### variable ###
플레이북에 변수가 설정된 파일이나 변수 정보를 전달할 수 있음 

vars_files
    변수가 선언된 파일
vars
    변수 선언

ex)
- name: test ping
  hosts: all
  vars_files:
    - ../group_vars/common.yml
  vars:
    sample_files_path: /home/deploy/sample_files
    id: sam

* 특수변수 
  https://docs.ansible.com/ansible/2.9/reference_appendices/special_variables.html
  앤서블에는 기본적으로 선언된 특수 변수가 존재 
  gather_facts명령으로 수집해야 하는 facts변수와 내부적으로 제공하는 magic 변수 

  all:
	  hosts:
	en:
	    hadoop:
	      hosts:
	        master-1:

	    zookeeper:
	      hosts:
	        master-2:

	  vars:
	    type: hadoop
	    version: 3.2.1

	Magic
	    groups
	        그룹 정보
	        hadoop, zookeeper
	    hostvars
	        변수 정보
	        type, version
	    inventory_hostname
	        현재 작업중인 호스트의 이름

	    hotvars 는 선언된 변수에 접근할 수 있는 명령어
	    ex)
	    - name: "EXAMINE HOSTVARS OF SPINE DEVICES"
		  hosts: "spine"
		  gather_facts: "no"
		  tasks:
		    - name: "DEBUG HOSTVARS"
		      debug:
		        var: "hostvars"

	Facts
	    ansible_facts
	    호스트의 설정 정보에 접근할 수 있는 명령어 
	    gather_facts가 실행되어야만 정보를 확인할 수 있음 
	    호스트의 OS 정보, IP 정보 등 운영체제 관련 정보를 확인
	    ex)
		- name: "check facts"
		  hosts: "all"
		  gather_facts: "yes"

		  tasks:
		    - debug:  
		        msg: "{{ ansible_facts }}"


	Connection variables
	    ansible_host
	        앤서블을 실행한 서버의 정보

 * 필터 
   변수의 기본 값, 변수 조작 등을 수행할 수 있는 함수

   # version - version은 버전을 비교할 때 사용
   ex)하둡 버전이 3.0.0 보다 낮은지를 비교
    - debug:
      msg: "{{ hadoop_version is version(\"3.0.0\", '<') }}"

   # join - 리스트 형식의 변수를 모아서 반환 파이썬의 join 함수와 동일 
   ex) {{ zk_server_ip | join(',') }}

   # length - 리스트 형식의 길이를 반환
   ex)
  	{% if list | length > 2 %}
	list 의 길이기 2 초과이면 처리 
	{% else %}
	list의 길이기 1이하이면 처리 
	{% endif %}

### 설정 ###
앤서블은 설정 파일을 이용하여 기본값을 설정
ansible.cfg 파일이 기본설정 파일

앤서블이 해당 파일을 참조 순서
1) ANSIBLE_CONFIG 환경변수로 지정된 파일
2) ./ansible.cfg
3) ~/.ansible.cfg
4) /etc/ansible/ansible.cfg

ex) ansible.cfg 
[defaults]
forks = 50 
host_key_checking = False
remote_user = deploy
remote_port = 22

forks: 한번에 처리하는 노드의 개수
host_key_checking: 앤서블에 연결하는 호스트의 키 확인 여부
remote_user: 서버에 접속하는 유저명
remote_port: 서버에 접속하는 포트


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
---   
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
...      
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

# OS가 우분투인지 CentOS 인지 확인하고, 우분투의 버전이 18.04인지, 20.04인지 확인 후 
  조건에 따른 mysql db setup 예시 
  -----------------------------------------------------
---
- name: Check os Name  
  shell: cat /etc/os-release | egrep ^NAME | awk -F "=" '{ print $2 }' | sed s/\"//g  
  register: os_name  

- name: setup db  
  apt:  
    name: mysql-server  
  become: yes  
  when: not mysql_installed.stat.exists and os_name.stdout == "Ubuntu"  

- name: "setup db"  
  yum:  
    name: mysql-server  
  become: yes  
  when: not mysql_installed.stat.exists and os_name.stdout == "CentOS Linux"  

- name: Check os Version  
  shell: cat /etc/os-release | grep VERSION_ID | awk -F "=" '{ print $2 }' | sed s/\"//g  
  register: os_version  

- name: mysql db cnf  
  template:  
    src: "my.cnf"  
  dest: "/etc/mysql/my.cnf"  
  become: yes  
  when: os_version.stdout is version("18.04", '=')  

- name: mysql db cnf  
  template:  
    src: "mysqld.cnf"  
  dest: "/etc/mysql/mysql.conf.d/mysqld.cnf"  
  become: yes  
  when: os_version.stdout is version("20.04", '=')
...    
  -----------------------------------------------------

* 패스워드 없이 수행하기 위해 앤서블 서버와 노드사이의 인증 생성
-----------------------------------------------------
---
- name: Create known_hosts between server and hosts
  host: nodes
  connection: local
  serial: 1

tasks:
  - name: ssh-keyscan for known_hosts file
    command: /usr/bin/ssh-keyscan -t ecdsa 
    register: keyscan
    
  - name: input key
    lineinfile:
      path: ~/.ssh/known_hosts
      line: ""
      create: yes
    with_item:
      - ""
      
- name: Create authority_key between server and hosts
  host: nodes
  connection: local
  gather_facts: no
  vars:
    ansible_password: <비밀번호>   
      
  - name: ssh-keygen for authorized_key file
    command: "ssh-keygen -b 2048 -t rsa -f ~/ssh/id_rsa -q -N ''"
    ignore_errors: yes
    run_once: true
    
  - name: input key for each node
    connection: ssh
    authorized_key:
      user: <유저이름>
      state: present
      key: ""
...      
-----------------------------------------------------

# ping test
---
-  name: ping test
   hosts: all
   gather_facts: false
   tasks:
   - name: ping
     ping:
...

# create directory & touch file
---
- name: touch files test
  hosts: all
  gather_facts: false
  tasks:
  - name: create directory
    file:
      path: /home/ubuntu/ansible
      state: directory
 
  - name: touch filens
    file:
      path: /home/ubuntu/ansible/ansible.txt
      state: touch
...
