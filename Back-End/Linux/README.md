# **Linux**

### 참고자료

- 리눅스를 다루는 기술

## 목차
>- [명령행 인터페이스](#명령행-인터페이스)
>>- [명령행 인터페이스로 시스템 관리하기](#명령행-인터페이스로-시스템-관리하기)
>>- [시작하고 종료하기](#시작하고-종료하기)
>>- [사용자 관리하기](#사용자-관리하기)
>>- [파일 관리하기](#파일-관리하기)


<br>

---

## 명령행 인터페이스

>### 명령행 인터페이스로 시스템 관리하기
>- 명령행 인터페이스 (Command Line Interface, CLI)
>>- 사용자가 키보드로 문자열 명령을 입력하고 컴퓨터가 처리한 결과를 화면에서 확인하는 상호작용
>- 명령행 인터페이스를 사용하는 이유
>>- 시스템 자원을 덜 소모하고 효율적으로 시스템을 관리할 수 있음
>>- 서비스를 빠르고 안정적으로 제공하는 일이 더 중요한 서버 컴퓨터에서는 GUI보다 CLI가 선호됨
>>- 시스템에 문제가 발생해서 GUI를 사용할 수 없는 응급 상황에서도 CLI는 사용 가능함
>- 셸 (shell)
>>- 사용자가 입력한 명령을 운영체제가 이해할 수 있게 변환하기 때문에 명령어 해석기라고도 함
>>- 커널 내부에서 이루어지는 일에 신경을 쓰지 않아도 셸로 시스템을 제어할 수 있음
>>- 다른 프로그램들을 실행하는 프로그램임
>>- 우분투를 비롯한 다른 여러 리눅스 운영체제의 기본 셸은 배시 (Bourne Again SHell, BASH)
>- 셸의 명령 입출력 과정
>>- user@ubuntu:~$
>>>- 셸이 사용자에게서 명령을 받을 준비가 되었음을 의미하는 프롬프트
>>- user@ubuntu:~$ echo HelloBash
>>>- echo : 배시가 제공하는 문자열 출력 명령
>>- 화면에 문자열을 표시하는 아주 단순한 명령이지만 echo 명령이 어떤 일을 하는지, 명령에 필요한 인자와 사용 가능한 옵션에는 무엇이 있는지 알고 있어야 함
>>- CLI에 익숙하다는 것은 셸을 잘 다룬다는 의미
>>- 능숙한 리눅스 시스템 관리자는 셸에서 사용 가능한 명령들을 알고 있어야 하며, 원하는 결과를 얻는 데 필요한 명령들을 조합하여 처리할 수 있어야 함
>- 셸 명령 옵션
>>- 어떤 명령은 세부 기능을 선택할 수 있도록 옵션을 제공함
>>- 옵션은 기호 --나 -로 시작하며ㅡ 영문 대,소문자로 입력함
>>- --로 시작하는 옵션은 이름으로 의미를 알기 쉽지만 여러 문자를 입력해야 함
>>- -로 시작하는 축약형 옵션은 편리하지만 어느 정도 암기가 필요
>>- 예시
>>>- user@ubuntu:~$ useradd --help 와 user@ubuntu:~$ useradd -h 는 같음
>>- 여러 옵션을 조합하여 편리하게 사용할 수 있음
>>>- $ ls : 파일 목록을 화면에 보여줌
>>>>- -a : 숨김 파일까지 모두 표시
>>>>- -l : 파일 정보를 함께 출력
>>>- user@ubuntu:~$ ls -al 처럼 옵션 조합을 할 수 있음 (대부분 순서와 상관 없이 명령을 처리함)

<br>

[목차로 이동](#목차)

>### 시작하고 종료하기
> 명령|설명
> :--|:--
> logout|셸 사용을 종료하고 로그인 대기 상태로 돌아감
> printenv|설정된 모든 환경변수를 출력
> export|환경변수를 등록
> unset|등록한 환경변수를 삭제
> shutdown|시스템을 종료
> reboot|시스템을 다시 시작
>- 셸 시작하기
>>- user@ubuntu:~$
>>>- user : 로그인한 사용자 계정
>>>- ubuntu : 호스트 이름 (네트워크에서 컴퓨터를 식별할 수 있는 이름)
>>>- ~ : 틸트라고 불리며 현재 작업 디렉토리 위치를 나타냄
>>>- $ : 로그인한 사용자 권한을 나타냄
>>>>- $ 은 일반 사용자 권한
>>>>- \# 는 루트 권한을 의미하지만 우분투는 기본적으로 루트 로그인을 권장하지 않으므로 시스템 복구할 때 외에는 볼 일이 없음
>- 셸 환경변수
>>- $ printenv
>>>- 현재 설정된 모든 환경변수를 출력
>>- 환경변수 : 사용자가로그인한 시스템이 동작하는 방식에 영향을 미치는 변수
>>- 중요한 환경변수 설명
>>>- SHELL : 현재 로그인한 셸
>>>>- 배시 셸의 경우 /bin/bash
>>>- PWD : 현재 작업 디렉토리 경로
>>>- LOGNAME : 로그인한 사용자 이름
>>>- HOME : 사용자 홈 디렉토리 경로
>>>- LANG : 로케일 설정
>>>- PATH : 실행할 명령을 찾는 경로
>>- 변수 정의
>>>- [변수명]=[값] 형식으로 입력하여 셸에서 사용할 변수를 정의
>>>>- $ VAR=1
>>>>- $ echo \$VAR
>>>>>- 변수를 사용할 때 변수 이름 앞에 스트링($)을 붙여야 함
>>>>-  사용자가 실행하는 명령은 셸의 자식 프로세스로 동작하기때문에 위에서 등록한 VAR 변수는 자식 프로세스에서 사용할 수 없고 printenv에 출력되지 않음
>>- 환경변수 설정 및 삭제
>>>- $ export VAR=1 
>>>>- 변수를 환경변수로 내보냄
>>>>- 이후 printenv 에 출력됨
>>>- $ unset VAR
>>>>- 환경변수를 삭제함
>>>- 환경변수 저장
>>>>- export 명령으로 내보낸 환경변수는 로그인 상태에서만 유지되며 터미널을 닫거나 로그아웃하면 초기화됨
>>>>- 환경변수를 계속 유지하려면 환경 설정 파일에 등록해야 함
>>>>- 현재 로그인한 사용자에 해당하는 환경변수는 홈 디렉토리의 배시 환경 설정 파일 .bashrc에 export 명령으로 등록
>>>>- 시스템 전체에 적용하려면 /etc/environment에 등록
>- 시스템 종료
>>- $ sudo shutdown [시간] [옵션]
>>>- -h : 명령을 실행한 이후 전원을 차단하는 옵션
>>>- -now : 지금 즉시 명령 실행
>>>- $ sudo shutdown -h now : 즉시 시스템 종료
>>>- $ sudo shutdown -h 15 : 15분 후 시스템 종료
>>>- $ sudo shutdown -h 03:30 : 3시 30분에 시스템 종료
>>>- $ sudo shutdown -c : 예약 종료 취소
>>>- $ sudo shutdown -r now : 시스템 재부팅
>>>>- $ sudo reboot 시스템을 재부팅할 수 있는 또 다른 명령

<br>

[목차로 이동](#목차)

>### 사용자 관리하기
> 명령|설명
> :--|:--
> $ whoami|로그인한 사용자 계정 표시
> $ who|사용자의 접속 정보 표시
> $ w|현재 접속한 사용자 정보와 시스템 정보를 함께 표시
> $ sudo|시스템 관리 권한 획득
> $ useradd, $ userdel|사용자 계정 추가, 삭제
> $ passwd|로그인 비밀번호 변경
> $ usermod|사용자 계정 정보 수정
> $ groups|사용자가 속해 있는 그룹 조회
> $ groupadd, $ groupdel|그릅 생성, 삭제
> $ gpasswd|그룹 정보 수정
>- 로그인한 사용자 정보 조회
>>- $ whoami
>>>- 현재 로그인한 사용자 계정을 화면에 표시
>>- $ who
>>>- 로그인한 사용자 계정과 접속한 터미널 정보, 로그인한 시각과 접속한 IP 주소 정보 출력
>>>- 접속자가 여럿이면 각각의 접속 정보가 표시
>>>- :0 은 그래픽 환경에서 생성된 첫 번째 터미널임을 의미
>>>>- 접속 환경에 따라 터미널 정보가 달라짐
>>>>- 우분투 서버에 직접 접속하는 콘솔 환경에서 who 명령 실행 시 터미널 정보는 tty1로 표시
>>>>- ssh를 이용하여 원격 접속 시 터미널 정보는 pts/0으로 표시
>>>>>- tty : 기본 터미널 장치 (teletypewriter)
>>>>>- pts : 의사 터미널 장치 (pseudo terminal)
>>- $ w
>>>- 현재 접속해 있는 사용자 정보와 함께 시스템 정보를 화면에 출력
>- 루트 권한 획득하기
>>- 리눅스는 여러 사용자가 동시에 사용 가능한 다중 사용자 운영체제임
>>>- 사용자는 권한에 따라 시스템 자원에 대한 접근, 사용 여부가 결정됨
>>- 시스템을 제어하는 명령을 실행하려면 시스템 관리 권한(루트) 권한이 필요
>>>- 시스템 관리(루트) 권한을 가지고 있는 사용자 계정을 루트 사용자 또는 슈퍼유저라고 함
>>>- 루트는 시스템을 제어할 수 있는 최상위 계정의 권한을 의미
>>- 리눅스는 루트 사용자의 로그인을 제한하기 때문에 시스템 관리자도 일반 사용자 계정으로 로그인해야 하며, 루트 권한이 필요할 때 루트 권한을 획득하는 과정을 거침
>>- 루트 권한이 필요한 명령을 처리할 때 sudo 명령을 사용
>>>- sudo는 다른 사용자 권한을 획득해서 명령을 실행하는 명령
>>- $ apt update : 권한이 없어 오류 메시지 표시됨
>>- $ sudo apt update : 패스워드 입력 시 패키지 목록을 업데이트
>>>- 패스워드를 잘못 입력하거나 해당 사용자가 루트 권한이 없다면 오류발생
>>- sudo로 획득한 루트 권한은 일정 기간 유지되어 매번 패스워드를 입력할 필요 없음
>- 사용자 계정 추가하기
>>- $ sudo useradd [옵션] [사용자 계정]
>>>- 시스템에 새로운 사용자 계정을 추가
>>>- -m (--create-home) 옵션을 붙이면 사용자 계정을 추가함과 동시에 홈 디렉토리를 함께 생성
>>>>- $ sudo useradd -m user123
>>>>- $ ls /home
>>- $ sudo passwd [사용자 계정]
>>>- 새로 추가한 사용자 계정의 패스워드를 변경
>>>- 현재 로그인한 사용자 계정의 패스워드를 변경할 때는 $ passwd만 입력
>>- $ cat /etc/passwd
>>>- 사용자 계정 목록을 확인
>>>- $ cat : 파일 내용 출력
>>>- [사용자 계정]:[패스워드]:[UID]:[GID]:[추가정보]:[홈 디렉토리]:[로그인 셸] 형식으로 출력
>>>>- 사용자 계정 : 시스템 운영을 위해 자동으로 생성된 사용자 계정이 대부분이며, 설치 과정에서 등록한 사용자, useradd로 추가한 사용자 계정을 확인할 수 있음
>>>>- 패스워드 : 아주 오래된 리눅스 시스템은 패스워드를 평문으로 저장, 현재 패스워드는 암호화해서 저장되므로 x로 표시
>>>>- UID : 사용자 계정을 식별하는 고유 번호 (User ID)
>>>>- GID : 사용자가 속해 있는 그룹 식별 번호 (Group ID)
>>>>- 추가정보 : 설치 과정에서 실제 이름을 입력했다면 추가 정보로 저장되며 adduser 명령에서 입력하는 정보들이 여기에 저장됨
>>>>>- $ adduser [사용자 계정]
>>>>>>- useradd와 passwd를 한 번에 사용하는 효과
>>>>>>- 사용자 추가 정보를 입력할 수 있음
>>>>- 홈 디렉토리 : 로그인 후 사용하는 기본 작업 디렉토리 경로
>>>>- 로그인 셸 : 로그인 후 사용할 기본 셸을 지정
>>- $ sudo userdel [옵션] [사용자 계정]
>>>- -r (--remove) 옵션 사용시 사용자 홈 디렉토리까지 함께 제거
>>>- -r 옵션을 사용하지 않은 경우 처리
>>>>- $ sudo userdel user123
>>>>- $ sudo rm -rf /home/user123
>- 사용자 계정 전환하기
>>- $ sudo cat /etc/sudoers
>>>- sudo를 실행할 수 있는 권한은 sudo 설정 파일인 /etc/sudoers 에서 지정함
>>>- 명령 실행시 사용자 계정 root와 그룹 admin, 그룹 sudo에 속하는 사용자는 모든 명령에 대해 sudo로 루트 권한을 얻을 수 있음을 확인할 수 있음
>>- $ su [옵션][사용자 계정]
>>>- 사용자 계정을 전환
>>>- $ whoami : su 명령으로 전환한 사용자 계정이 표시
>>>- $ who : 로그인한 사용자 계정 정보를 표시되므로 su 명령을 자주 사용할 때 처음 로그인한 사용자 계정을 확인하기 위해 쓰임
>>- $ groups
>>>- 사용자가 속해 있는 그룹을 조회
>>>- 계정 생성시 자동으로 같은 이름의 사용자 그룹이 생성됨
>>>- sudoers 파일에 등록된 사용자가 아닌 경우 sudo를 사용할 수 없음
>>>- exit를 입력하면 su 명령 전 원래 사용자 계정으로 복귀함
>>- $ sudo usermod [옵션] [사용자 계정]
>>>- -a (--append) : 변경 대신 정보를 추가하는 옵션
>>>>- 해당 옵션이 없으면 사용자 계정의 그룹은 추가가 아닌 변경이 되어 기존 정보가 모두 삭제됨
>>>- -G (--groups) : 사용자 계정의 그룹을 대상으로 함
>>>- $ sudo usermod -a -G sudo user123
>>>>- user123 계정을 sudo 그룹에 포함시킴
>>>- $ sudo useradd -m -G sudo user123
>>>>- 새로운 사용자 계정을 추가하는 과정에서 그룹을 지정하여 번거로움을 피할 수 있음
>>>- $ sudo chsh -s /bin/bash user123
>>>>- 사용자 계정의 사용 셸 변경
>>>- $ sudo gpasswd -d [사용자 계정] [삭제할그룹]
>>>>- 단순히 그룹에서 사용자를 삭제하려면 그룹 관리 명령인 gpasswd가 편리함
>- 그룹 관리하기
>>- $ sudo groupadd [옵션] [그룹]
>>>- 새로운 그룹을 생성
>>- $ sudo gpasswd -a [사용자 계정] [그룹명]
>>>- -a (--add) : 그룹에 사용자를 추가
>>- $ sudo gpasswd -M [사용자 계정],[사용자 계정],..., [그룹명]
>>>- 그룹에 여러 사용자를 한번에 추가
>>- $ groups [사용자 계정]
>>>- 해당 사용자가 속해 있는 그룹을 출력
>>- $ sudo cat /etc/group
>>>- 모든 그룹 정보 출력
>>- $ sudo gpasswd -d [사용자 계정] [그룹명]
>>>- 그룹에서 사용자를 삭제, -d (--delete)
>>- $ sudo groupdel [그룹명]
>>>- 그룹 삭제

<br>

[목차로 이동](#목차)

>### 파일 관리하기
> 명령|설명
> :--|:--
> ls|파일 목록 조회
> chown|파일 소유권 변경
> chmod|파일 접근 권한 변경
> pwd|작업하고 있는 디렉토리 위치 출력
> cd|작업 디렉토리 변경
> mkdir, rmdir|디렉토리 생성, 삭제
> touch|빈 파일 생성
> cp, mv, rm|파일 복사, 이동, 삭제
> cat|파일 내용을 화면에 표시
> more, less|파일 내용을 스크롤
> head, tail|파일 내용 중 일부를 표시
> find, which, whereis, grep|파일 검색
>- 리눅스에서의 파일
>>- 리눅스에선 텍스트, 이미지, 영상 뿐 아니라 파일을 묶는 디렉터리, 네트워크 소켓 등 자료 흐름과 시스템 장치까지 모든 것이 파일임
>>- 다중 사용자 운영체제인 리눅스에서는 모든 파일마다 파일을 소유하는 사용자 계정 정보와 해당 파일에 대한 접근 권한이 부여됨
>>- 사용자는 파일을 사용하기 전에 그 파일을 제어할 수 있는 소유권과 접근 권한을 갖고 있는지 확인해야 함
>- 파일 목록 화면에 표시
>>- $ ls [옵션] [파일]
>>>- -l : 긴 리스트 포맷을 사용하여 출력
>>>- -h (--human-readable) : 파일 용량을 MB, GB 단위로 바꾸어 출력
>>>- -a (--all) : 숨김 파일까지 출력
>- 파일 정보에 대해
>>- ls -l 명령으로 출력되는 파일 목록 형식
>>>- 접근 권한 | 링크 | 소유자 | 소유 그룹 | 크기 | 최종 변경한 날짜와 시간 | 파일이름
>>- 파일 접근 권한 해석
>>>- drwxrwxr-x
>>>- 첫 문자가 d 이면 디렉터리, - 이면 일반 파일, l 이면 링크
>>>- 다음 세 문자는 차례로 소유자, 소유자가 포함된 그룹, 다른 모든 사용자에 대한 읽기, 쓰기, 실행 권한의 유무
>>>- rwx 대신 -가 표시되어 있다면 각 권한이 없음을 의미함
>- 파일 소유권과 접근 권한 변경하기
>>- 