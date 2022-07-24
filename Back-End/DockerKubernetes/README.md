# **Docker&Kubernetes** 🔄

### 참고자료

- 컨테이너 인프라 환경 구축을 위한 쿠버네티스/도커

### 학습 프로젝트 저장소

- [링크]()

## 목차
>- [새로운 인프라 환경이 온다](#새로운-인프라-환경이-온다)
>>- [컨테이너 인프라 환경이란](#컨테이너-인프라-환경이란)
>>- [컨테이너 인프라 환경을 지원하는 도구](#컨테이너-인프라-환경을-지원하는-도구)
>>- [새로운 인프라 환경의 시작](#새로운-인프라-환경의-시작)
>- [테스트 환경 구성하기](#테스트-환경-구성하기)
>>- [테스트 환경을 자동으로 구성하는 도구](#테스트-환경을-자동으로-구성하는-도구)
>>- [베이그런트로 랩 환경 구축하기](#베이그런트로-랩-환경-구축하기)
>>- [터미널 프로그램으로 가상 머신 접속하기](#터미널-프로그램으로-가상-머신-접속하기)


<br>

---

## 새로운 인프라 환경이 온다

>### 컨테이너 인프라 환경이란
>- 개요
>>- 컨테이너(container)는 하나의 운영체제 커널에서 다른 프로세스에 영향을 받지 않고 독립적으로 실행되는 프로세스 상태를 의미함
>>- 가상화 상태에서 동작하는 프로세스보다 가볍고 빠르게 동작함
>- 모놀리식 아키텍처
>>- 모놀리식 아키텍처(monolithic architecture)는 하나의 큰 목적이 있는 서비스 또는 애플리케이션에 여러 기능이 통합돼 있는 구조를 의미함
>>- 소프트웨어가 하나의 결합된 코드로 구성되기 때문에 초기 단계에서 설계하기 용이하며 개발지 좀 더 단순하고 코드 관리가 간편함
>>- 하지만 서비스를 운영하는 과정에서 수정이 많을 경우 연관된 다른 서비스에 영향을 미칠 가능성이 커지며, 서비스가 점점 성장해 기능이 추가될수록 처음에는 단순했던 서비스 간의 관계가 매우 복잡해질 수 있음
>- 마이크로서비스 아키텍처
>>- 마이크로서비스 아키텍처(MSA, Microservices Architecture)는 개별 기능을 하는 작은 서비스를 각각 개발해 연결하여 시스템 전체가 하나의 목적을 지향하는 구조임
>>- 개발된 서비스를 재사용하기 쉽고, 향후 서비스가 변경됐을 때 다른 서비스에 영향을 미칠 가능성이 줄어들며 사용량의 변화에 따라 특정 서비스만 확장할 수 있으므로 사용자의 요구사항에 따라 가용성을 즉각적으로 확보해야 하는 IaaS 환경에 적합함
>>- 하지만 모놀리식 아키텍처보다 복잡도가 높으며 각 서비스가 서로 유기적으로 통신하는 구조로 설계되기 때문에 네트워크를 통한 호출 횟수가 증가해 성능에 영향을 줄 수 있음

<br>

[목차로 이동](#목차)

>### 컨테이너 인프라 환경을 지원하는 도구
>- 개요
>>- 컨테이너 인프라 환경은 크게 컨테이너, 컨테이너 관리, 개발 환경 구성 및 배포 자동화, 모니터링으로 구성됨
>- 도커
>>- 도커(Docker)는 컨테이너 환경에서 도긻적으로 애플리케이션을 실행할 수 있도록 컨테이너를 만들고 관리하는 것을 도와주는 컨테이너 도구임
>>- 도커로 애플리케이션을 실행하면 운영체제 환경에 관계없이 독립적인 환경에서 일관된 결과를 보장함
>- 쿠버네티스
>>- 쿠버네티스(Kubernetes)는 다수의 컨테이너를 관리하는 데 사용함
>>- 컨테이너의 자동 배포와 배포된 컨테이너에 대한 동작 보증, 부하에 따른 동적 확장 등의 기능을 제공함
>>- 처음에는 다수의 컨테이너만 관리하는 도구였지만, 지금은 컨테이너 인프라에 필요한 기능을 통합하고 관리하는 솔루션으로 발전했음
>- 젠킨스
>>- 젠킨스(Jenkins)는 지속적 통합(CI, Continuous Integration)과 지속적 배포(CD, Continuous Deployment)를 지원함
>>- 지속적 통합과 지속적 배포는 개발한 프로그램의 빌드, 테스트, 패키지화, 배포 단계를 모두 자동화해 개발 단계를 표준화함
>- 프로메테우스와 그라파나
>>- 프로메테우스(Prometheus)와 그라파나(Grafana)는 모니터링을 위한 도구임
>>- 프로메테우스는 상태 데이터를 수집하고, 그라파나는 수집한 데이터를 관리자가 보기 좋게 시각화함
>>- 컨테이너 인프라 환경에서는 많은 종류의 소규모 기능이 각각 나누어 개발되기 때문에 중앙 모니터링이 필요함
>>- 프로메테우스와 그라파나는 컨테이너로 패키징돼 동작하며 최소한의 자원으로 쿠버네티스 클러스터의 상태를 시각적으로 표현함
>>- 데이터를 시각화하는 도구는 그라파나와 키바나(Kibana)가 시장을 양분한 상태이지만, 키바나는 프로메테우스와 연결 구성이 복잡하여 프로메테우스를 사용할 때는 간결하게 구성할 수 있는 그라파나가 더 선호됨

<br>

[목차로 이동](#목차)

---

## 테스트 환경 구성하기

>### 테스트 환경을 자동으로 구성하는 도구
>- 개요
>>- 코드로 인프라를 생성할 수 있게 지원하는 소프트웨어는 많지만 교육용 및 소규모 환경에선 베이그런트(Vagrant)가 가장 배우기 쉽고 사용 방법도 간단함
>>- 베이그런트는 가상화 소프트웨어인 버추얼 박스와도 호환성이 매우 좋음
>- 버추얼 박스 설치하기
>>- 버추얼 박스는 현존하는 대부분의 운영체제를 게스트 운영체제로 사용할 수 있으며 확장팩을 제외하면 아무런 제한 없이 소프트웨어의 모든 기능을 무료로 이용할 수 있음
>>- 다른 가상화 소프트웨어보다 기능이 강력하고 안정적임
>>- https://www.virtualbox.org/wiki/Download_Old_Builds_6_1 에서 6.1.12 버전으로 다운로드하고 기본 세팅으로 설치함
>- 베이그런트 설치하기
>>- 베이그런트는 프로비저닝(provisioning)해줌
>>>- 프로비저닝이란 사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요할 때 시스템을 사용할 수 있는 상태로 미리 준비해두는 것
>>- https://www.vagrantup.com/downloads 에서 다운로드하고 기본 세팅으로 설치함
>- 베이그런트 구성하고 테스트하기
>>- 테스트 환경을 구성하기 전에 설치된 도구가 정상적으로 작동하는지 확인하기 위해 프로비저닝을 위한 코드를 작성하고, 이를 베이그런트에서 불러온 후 버추얼박스에 운영체제를 설치함
>>>1. cmd를 실행하고 베이그런트 설치 디렉토리로 이동후 vagrant init 명령을 실행하여 프로비저닝에 필요한 기본 코드를 생성함
>>>2. c:/HashiCorp 폴더의 Vagrantfile을 열어서 config.vm.box = "base" 라는 내용이 있는지 확인
>>>3. cmd에서 vagrant up 실행 후 설치하려는 이미지가 base로 명시되어 있으나 베이그런트가 해당 이미지를 찾지 못해 발생하는 에러 확인
>>>4. 가상 머신의 이미지를 선택하고 필요에 맞게 이미지를 수정하는 과정은 매우 복잡하고 어려우므로 저자가 목적에 맞게 제작해 둔 가상 이미지를 사용함
>>>>- 가상이미지는 베이그런트 클라우드(https://app.vagrantup.com/boxes/search)에 접속해 내려받는데, sysnet4admin/CentOS-k8s를 검색하여 확인해봄
>>>5. Vagrantfile의 config.vm.box ="sysnet4admin/CentOS-k8s" 로 수정함
>>>6. vagrant up 실행하여 해당 가상 머신 이미지를 내려받는지 확인함
>>>7. 버추얼박스를 실행하여 가상 머신이 제대로 생성됐는지 확인함
>>>8. cmd에서 vagrant ssh 실행하여 설치된 CentOS에 접속함
>>>9. uptime과 cat /etc/redhat-release 를 입력하여 설치가 정상적으로 이루어졌는지 확인함
>>>10. 설치 테스트가 완료되었다면 vagrant destroy -f 를 실행하여 가상 머신을 삭제함

<br>

[목차로 이동](#목차)

>### 베이그런트로 테스트 환경 구축하기
>- 가상 머신에 필요한 설정 자동으로 구성하기
>>1. 원하는 구성을 자동으로 생성할 수 있도록 Vagrantfile을 새로 작성, 베이그런트 코드는 루비로 작성됨
>>```ruby
>># -*- mode: ruby -*-
>># vi: set ft=ruby :
>>Vagrant.configure("2") do |config|
>>  config.vm.define "m-k8s" do |cfg|
>>  config.vm.boot_timeout = 1800
>>    cfg.vm.box = "sysnet4admin/CentOS-k8s"
>>    cfg.vm.provider "virtualbox" do |vb|
>>      vb.name = "m-k8s(github_SysNet4Admin)"
>>      vb.cpus = 1
>>      vb.memory = 1024
>>      vb.customize ["modifyvm", :id, "--groups", "/k8s-SM(github_SysNet4Admin)"]
>>    end
>>    cfg.vm.host_name = "m-k8s"
>>    cfg.vm.network "private_network", ip: "192.168.1.10"
>>    cfg.vm.network "forwarded_port", guest: 22, host: 60010, auto_correct: true, id: "ssh"
>>    cfg.vm.synced_folder "../data", "/vagrant", disabled: true
>>  end
>>end
>>```
>>2. vagrant up 실행 후 vagrant ssh 로 접속
>>3. ip addr show eth1 실행하여 ip가 192.168.1.10 으로 설정됐는지 확인
>>4. exit 로 CentOS 접속 종료
>- 가상 머신에 추가 패키지 설치하기
>>1. Vagrantfile 에 셸 프로비전을 추가
>>```ruby
>>...
>>cfg.vm.synced_folder "../data", "/vagrant", disabled: true
>>cfg.vm.provision "shell", path: "install_pkg.sh" #add provisioning script
>>```
>>2. Vagrantfile 이 위치한 디렉토리에 추가 패키지를 설치하기 위한 스크립트인 install_pkg.sh 를 작성함
>>```sh
>>#!/usr/bin/env bash
>># install packages
>>yum install epel-release -y
>>yum install vim-enhanced -y
>>```
>>3. vagrant provision 명령으로 추가한 프로비전 구문을 실행함
>>4. vagrant ssh 명령으로 CentOS에 접속함
>>5. yum repolist 명령으로 추가한 EPEL 저장소가 구성됐는지 확인함
>>6. vi .bashrc 명령으로 문법 하이라이트가 적용됐는지 확인함
>>7. 모든 내용을 확인했다면 exit 후 vagrant destroy -f 명령으로 가상 머신을 삭제함
>- 가상 머신 추가로 구성하기
>>- 베이그런트로 운영 체제를 자동으로 설치하고 구성하면 편리하지만 단순히 운영 체제 1개를 구성하려고 사용하는 것은 아님
>>- 기존에 설치한 가상 머신 외에 가상 머신 3대를 추가로 설치하고 기존의 가상 머신과 네트워크 통신이 원활하게 작동하는지 확인함
>>- Vagrantfile
>>```ruby
>># -*- mode: ruby -*-
>># vi: set ft=ruby :
>>Vagrant.configure("2") do |config|
>>  config.vm.define "m-k8s" do |cfg|
>>  config.vm.boot_timeout = 1800
>>    cfg.vm.box = "sysnet4admin/CentOS-k8s"
>>    cfg.vm.provider "virtualbox" do |vb|
>>      vb.name = "m-k8s(github_SysNet4Admin)"
>>      vb.cpus = 2
>>      vb.memory = 2048
>>      vb.customize ["modifyvm", :id, "--groups", "/k8s-SM(github_SysNet4Admin)"]
>>    end
>>    cfg.vm.host_name = "m-k8s"
>>    cfg.vm.network "private_network", ip: "192.168.1.10"
>>    cfg.vm.network "forwarded_port", guest: 22, host: 60010, auto_correct: true, id: "ssh"
>>    cfg.vm.synced_folder "../data", "/vagrant", disabled: true
>>    cfg.vm.provision "shell", path: "install_pkg.sh" #add provisioning script
>>    cfg.vm.provision "file", source: "ping_2_nds.sh", destination: "ping_2_nds.sh"
>>    cfg.vm.provision "shell", path: "config.sh"
>>  end
>>
>>  #=============#
>>  # Added Nodes #
>>  #=============#
>>  (1..3).each do |i|
>>    config.vm.define "w#{i}-k8s" do |cfg|
>>    config.vm.boot_timeout = 1800
>>      cfg.vm.box = "sysnet4admin/CentOS-k8s"
>>      cfg.vm.provider "virtualbox" do |vb|
>>        vb.name ="w#{i}-k8s(github_SysNet4Admin)"
>>        vb.cpus = 1
>>        vb.memory = 1024
>>        vb.customize ["modifyvm", :id, "--groups", "/k8s-SM(github_SysNet4Admin)"]
>>      end
>>      cfg.vm.host_name = "w#{i}-k8s"
>>      cfg.vm.network "private_network", ip: "192.168.1.10#{i}"
>>      cfg.vm.network "forwarded_port", guest: 22, host: "6010#{i}", auto_correct: true, id: "ssh"
>>      cfg.vm.synced_folder "../data", "/vagrant", disabled: true
>>      cfg.vm.provision "shell", path: "install_pkg.sh"
>>    end
>>  end
>>end
>>```
>>- ping_2_nds.sh
>>```sh
>># ping 3 times per nodes
>>ping 192.168.1.101 -c 3
>>ping 192.168.1.102 -c 3
>>ping 192.168.1.103 -c 3
>>```
>>- config.sh
>>```sh
>>#!/usr/bin/env bash
>># modify permission
>>chmod 744 ./ping_2_nds.sh
>>```
>>1. vagrant up 명령으로 총 4대의 CentOS를 설치하고 구성함
>>2. vagrant ssh 명령으로 설치된 CentOS에 접속 시도시 설치된 가상 머신이 여러 대이기 때문에 접속할 가상 머신의 이름을 입력해야 한다는 메시지가 출력되므로 vagrant ssh m-k8s 명령을 실행함
>>3. Vagrantfile 에 작성된 스크립트에 따라 업로드된 ./ping_2_nds.sh 파일을 실행하여 3대의 CentOS와 통신하는 데 문제가 없는지 확인함
>>4. 실행에 이상이 없다면 exit 명령으로 가상 머신 접속을 종료

<br>

[목차로 이동](#목차)

>### 터미널 프로그램으로 가상 머신 접속하기
>- 푸티 설치하기
>>- 푸티(PuTTY란 터미널 접속 프로그램 중에서 가장 많이 사용되는 프로그램임
>>- 가볍고 무료이며 다양한 플러그인을 통해 여러 대의 가상 머신에 접속할 수 있으며 접속 정보를 저장하고 바로 불러와 실행할 수 있는 기능이 있음
>>- https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html 에서 Alternative binary files 항목에서 운영 체제 및 버전에 맞는 실행 파일을 다운로드함
>- 슈퍼푸티 설치하기
>>- 푸티를 단독으로 사용하면 창을 여러 개 띄워야 하므로 명령을 내리기가 매우 번거롭다는 문제가 있는데 슈퍼푸티를 사용하여 해결이 가능함
>>- https://github.com/jimradford/superputty/releases/tag/1.4.0.9 에서 SuperPuttySetup-1.4.0.9.msi 를 다운로드 받고 실행하여 기본옵션으로 설치함
>>- 설치완료 후 실행하여 putty.exe Location (Required) 옆의 Browse 버튼을 클릭하여 푸티의 위치를 지정함
>- 슈퍼푸티로 다수의 가상 머신 접속하기
>>1. 슈퍼푸티 화면 오른쪽에 위치한 Sessions 창의 PuTTY Sessions 에서 마우스 오른쪽 버튼을 클릭하고 New Folder를 클릭함
>>2. 접속 정보 입력 창에서 k8s를 입력하고 OK 클릭함
>>3. 새로 추가된 k8s 디렉토리에서 마우스 오른쪽 버튼을 클릭하고 New 클릭
>>4. 가상 머신의 정보를 입력하는 창에 Session Name에 m-k8s, Host Name에 127.0.0.1, TCP Port에 60010, Login Username에 root, Extra PuTTY Arguments에 -pw vagrant 입력 후 Save 하면 Arguments 가 평문으로 저장되어 보안상 위험하다는 창이 뜨는데 확인 클릭함
>>5. m-k8s 에서 마우스 오른쪽 버튼을 클릭하여 Copy As로 복사하고 Session Name에 w1-k8s, TCP Port에 60101 입력함
>>6. m-k8s 에서 마우스 오른쪽 버튼을 클릭하여 Copy As로 복사하고 Session Name에 w2-k8s, TCP Port에 60102 입력함
>>7. m-k8s 에서 마우스 오른쪽 버튼을 클릭하여 Copy As로 복사하고 Session Name에 w3-k8s, TCP Port에 60103 입력함
>>8. 평문으로 접속하기 위해 슈퍼푸티의 보안 설정을 변경, Tools > Options 의 GUI 탭에서 Allow plain text passwords on putty command line 항목을 체크하고 OK
>>9. k8s 디렉토리에서 마우스 오른쪽 버튼을 누르고 Connect All을 선택하여 모든 가상 머신에 한번에 접속함
>>10. 슈퍼푸티가 푸티를 호출하면서 경고창이 뜨는데 실행을 누르고 추가로 발생하는 보안경고에서도 예를 클릭함
>>11. 가상 머신에 접속되는지 확인하고 창을 분리배치하여 작동을 한눈에 확인할 수 있음
>>12. 상단의 commands 창에서 hostname을 입력하여 4개의 창에서 모두 실행되는지 확인함
>>13. 이상이 없다면 vagrant destroy -f 로 가상 머신을 모두 삭제함

<br>

[목차로 이동](#목차)

---

## 컨테이너를 다루는 표준 아키텍처, 쿠버네티스

>### 쿠버네티스 이해하기
>- 

<br>

[목차로 이동](#목차)

>### 쿠버네티스 기본 사용법 배우기
>- 

<br>

[목차로 이동](#목차)

>### 쿠버네티스 연결을 담당하는 서비스
>- 

<br>

[목차로 이동](#목차)

>### 알아두면 쓸모 있는 쿠버네티스 오브젝트
>- 

<br>

[목차로 이동](#목차)

---