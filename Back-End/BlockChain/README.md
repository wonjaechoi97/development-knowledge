# **블록체인** 🔗

## 목차
>- [블록체인 네트워크의 이해](#블록체인-네트워크의-이해)
>>- [이더리움 블록체인 네트워크의 분류](#이더리움-블록체인-네트워크의-분류)
>>- [이더리움 네트워크 개념도](#이더리움-네트워크-개념도)
>>- [환경설정](#환경설정)
>>- [로컬 네트워크 활용 및 실습](#로컬-네트워크-활용-및-실습)
>>- [블록체인 네트워크 실습](#블록체인-네트워크-실습)

<br>

---

## 블록체인 네트워크의 이해
>- 프라이빗 네트워크 실습으로 이더리움 네트워크 시작하기

>### 이더리움 블록체인 네트워크의 분류
>- 이더리움 블록체인 네트워크의 분류
>>- 퍼블릭 네트워크
>>- 프라이빗 네트워크
>- 퍼블릭 네트워크
>>- 일반적으로 알고있는 거래소에 상장되어있는 이더리움 네트워크
>>- 전 세계 수많은 사람들이 동일한 데이터를 유지하고 있는 네트워크
>>- 종류
>>>- 메인넷
>>>>- 거래소 상에서 거래를 한다거나 실제 스마트 컨트랙트를 배포하는 등 실제 운용되는 네트워크
>>>- 테스트넷
>>>>- 메인넷에서 작업하기 전에 미리 이더리움을 경험해볼 수 있는 네트워크 환경
>>>>- 종류
>>>>>- 롭슨(Ropsten) - 우리가 사용할 테스트 환경
>>>>>- 코반(Kovan)
>>>>>- 링키비(Rinkeby)
>>>>>- 고얼리(Goerli)
>- 프라이빗 네트워크
>>- 로컬 환경에서 이더리움을 구동하거나 몇몇 소수끼리 이더리움을 구동하는 네트워크 환경
>>- 이더리움 거래를 할 순 있지만 메인넷처럼 가치가 있는 이더리움을 거래할 순 없음
>- 퍼블릭 네트워크와 프라이빗 네트워크의 차이점
>>- 퍼블릭 네트워크는 전 세계 사람들이 동일한 장부를 유지하고 있기 때문에 입력한 데이터가 불가역적으로 저장됨
>>- 프라이빗 네트워크는 불가역적으로 저장되지 않으므로 테스트용 또는 실습용으로만 사용하는것이 추천됨

<br>

[목차로이동](#목차)

>### 이더리움 네트워크 개념도
>![2](https://user-images.githubusercontent.com/65841586/185818846-b3b115a2-042c-4300-8344-4f6c103bad70.PNG)
>- 개요
>>- 이더리움 블록체인 네트워크는 다수의 노드로 구성됨
>>- 노드는 수많은 이더리움에 들어있는 데이터들을 모두 동기화하여 똑같은 데이터를 갖고 있음
>>- 우리는 노드 하나를 직접 운영하거나 노드에 명령을 내리는 터미널이나 브라우저를 활용하여 이더리움 네트워크를 활용할 수 있음
>>- 개념도는 이더리움 외 다른 블록체인들도 유사함
>- 블럭
>>- 노드는 다양한 데이터를 동기화하여 가지고 있는데 데이터를 블럭의 형태로 가지고 있음
>>- 이더리움에 참여하기 위해서는 이더리움 클라이언트가 필요함
>>>- 일반적으로 geth 클라이언트를 사용함

<br>

[목차로이동](#목차)

>### 환경설정
>- Prerequisites
>>- Chocolatey 설치
>>>- PowerShell 관리자 권한으로 실행
>>>- 설치
>>>```
>>>Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
>>>```
>>- 사전 필요 요소 설치
>>```
>>choco install git -y
>>choco install golang -y
>>choco install mingw -y
>>```
>- Geth 설치
>>- cmd 실행
>>- 설치
>>```
>>mkdir src\github.com\ethereum
>>git clone https://github.com/ethereum/go-ethereum --branch v1.9.24 src\github.com\ethereum\go-ethereum
>>cd src\github.com\ethereum\go-ethereum
>>go get -u -v golang.org/x/net/context
>>go install -v ./cmd/...
>>```
>>- 설치 확인
>>```
>>geth version
>>```
>- 가나슈 설치
>>- node.js 설치
>>```
>>choco install nodejs-lts
>>```
>>- ganache-cli 설치
>>```
>>npm install -g ganache-cli
>>```
>>- 설치 확인
>>```
>>ganache-cli --version
>>```
>- MateMask
>>- 크롬 확장프로그램에서 추가
>- 관련 이론
>>- 이더리움 계정
>>>1. 개인키 생성 : 256 bit 의 무작위 숫자 -> 64 자리의 Hex 값으로 인코딩
>>>2. 1 을 타원곡선전자서명 알고리즘 (ECDSA, secp256k1) 을 사용하여 공개키 생성
>>>3. 2 를 Keccak-256 hashing 함
>>>4. 3의 마지막 20Byte 가 계정 주소임
>>- 지갑 생성
>>>- 비대칭키 암호화
>>>>- 비대칭키란 복호화키와 암호화키가 다른 것


<br>

[목차로이동](#목차)

>### 로컬 네트워크 활용 및 실습
>- 가나슈 구동
>```
># 로컬 테스트넷 구동
>ganache-cli -d -m -p 7545 -a 5
>
># 명령어 옵션 확인
>genache-cli --help
>```
>>- -d -m (--deterministic --mnemonic) HD Wallet 생성 시 니모닉 구문 사용
>>- -p (--port) 포트 지정 (default 8545)
>>- -a (--account) 구동 시 생성할 계정 수 (default 10)
>- Geth 로 네트워크에 접속
>>```
>># geth 명령어로 가나슈 테스트넷에 접속 ( 새 명령 프롬프트)
>>geth attach http://localhost:7545
>>
>># 연결성 확인 Connectivity Check
>>net.listening
>>net.peerCount
>>
>># 계정 목록 확인
>>eth.accounts
>>
>># 계정 보유 잔액 확인
>>web3.fromWei(eth.getBalance(eth.accounts[0]))
>>```
>- 메타마스크 + Geth 활용
>>- 메타마스크로 네트워크에 접속
>>>1. 네트워크 추가 : 사용자 정의 RPC > 네트워크 추가
>>>2. 네트워크 정보 입력
>>>>- 네트워크 이름 : 자유
>>>>- RPC URL : http://localhost:7545
>>>>- 체인 ID : geth console 에서 chainId 확인
>>>>```
>>>>eth.chainId()
>>>>```
>>>>- 통화기호 : ETH
>>- 네트워크 변경 완료
>>>- 메타마스크 네트워크 상태를 확인
>>>- 가나슈 구동 화면에서 메타마스크가 주기적으로 가나슈 네트워크에 정보를 요청하는 것을 확인할 수 있음
>>>- 터미널과 브라우저의 연결 방법 차이점
>>>>- 터미널 : SSH
>>>>- 브라우저 : RPC-JSON
>>- 메타마스크 계정으로 이더 전송
>>>- geth consol 로 진행
>>>```
>>>tx = { from: "가나슈_제공_계정_중_하나", to: "메타마스크_계정", value: 1e18 }
>>>
>>>eth.sendTransaction(tx)
>>>```
>>>>- 이더리움 기본 단위는 Wei(웨이), 1 Ether = 10^18 Wei
>>>>- 계정은 16 진수 문자열 형태로 따옴표로 묶어 작성함
>>>```
>>># 트랜잭션 상태 확인
>>>eth.getTransaction("transactionHash")
>>>eth.getTransactionReceipt("transactionHash")
>>># Status 0x1 임을 확인
>>>
>>># from 주소 잔액 확인
>>>eth.getBalance(eth.accounts[0])
>>>
>>># 단위 환산
>>>web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
>>>```

<br>

[목차로이동](#목차)

>### 블록체인 네트워크 실습
>- 문제1 : geth console 에서 데이터를 담은 트랜잭션 생성
>>- 사전 준비 : 트랜잭션을 보낼 주소와 받을 주소를 선택하고 잔고를 확인함
>>1. sendTransaction 을 사용하여 트랜잭션을 보냄
>>2. "hello ethereum" 메시지를 포함시켜야함
>>3. API 호출 후 트랜잭션 처리 결과를 확인함
>>>- 트랜잭션 상태 확인
>>>- 트랜잭션 종결 후 "hello ethereum" 데이터 확인
>>>- 보낸 주소의 잔고
>- 문제 해결
>>- 계정 목록 확인 : eth.accounts
>>- 잔고 확인 : eth.getBalance("조회한 계정중 하나 선택")
>>- 트랜잭션 생성
>>>- 트랜잭션을 이더리움의 상태를 변경시키는 역할이라는 매우 중요한 기능을 함
>>>- 트랜잭션 메시지 생성
>>>>- data = web3.toHex("hello ethereum") 입력 시 "0x68656c6c6f20657468657265756d" 이라는 헥사데시멀 문자로 변환해줌
>>>- 트랜잭션 메시지 필드
>>>>- tx = {from: 보낼 계정, to: 받을 계정, data: "0x68656c6c6f20657468657265756d""} 
>>- 트랜잭션 전송
>>>- tx_hash = eth.sendTransaction(tx)
>>>- sendTransaction API 를 통해 트랜잭션 메시지의 signature (from 주소의 개인키를 이용한 전자 서명 생성) 가 생성되고 블록체인 네트워크로 전송됨
>>>- 트랜잭션 전송 결과로 트랜잭션 고유의 해시 값이 반환되는데 이 값을 통해 트랜잭션 상태를 조회할 수 있음
>>- 트랜잭션 조회
>>>- tx_detail = eth.getTransaction(tx_hash)
>>>- 조회 결과로 나오는 필드 설명
>>>>- input : 생성된 트랜잭션의 입력 값
>>>>- blockHash, blockNumber : 트랜잭션이 완결되면 (블록에 담겨 처리됨) 유효한 값 생성
>>>>- gas : 트랜잭션이 소요할 수 있는 최대 가스 카운트 (gas limit)
>>>>- nonce : 보낸 주소(from)로부터 생성된 트랜잭션의 순차적 수 (PoW의 nonce와 구분하기 위해 Transaction nonce 라고도 함)
>>>>- v, r, s : ECDSA (Ellipstic Curve Digital Signature)의 구성요소, 전자 서명을 검증하기 위한 값, 전자 서명을 생성하기 위한 입력 값 정보를 포함
>>- 트랜잭션 결과 확인
>>>- 트랜잭션 input 필드 검사
>>>>- web3.toAscii(tx_detail.input)
>>>- 보낸 주소의 계정 잔액 확인
>>>>- balance = eth.getBalance(from)
>>>>- web3.fromWei(balance, "ether")
>- 트랜잭션 조회 화면
>> <img width="719" alt="캡처" src="https://user-images.githubusercontent.com/65841586/185955285-e9c80680-811a-4863-9f1a-58051b36ca28.PNG">
>- 문제2 : 에세이 작성
>>1. 많은 사람들이 블록체인에 기록된 데이터를 신뢰할 수 있다고 말하는데 그 근거는 무엇인가
>>>- 블록체인은 중앙화되지 않고 여러 참여자들에 의해 데이터가 공유되고 검증되므로 데이터가 조작되는 것이 사실상 불가능하기 때문임
>>>- 블록체인의 데이터는 네트워크 참여자들에 의해 승인되므로 개별적으로 결정되어 기록된 데이터보다 높은 신뢰도를 가짐
>>2. 블록체인이 산업계에 미치는 영향을 언급하고, 이를 기반으로 한 응용 사례를 제시하라
>>>- 블록체인 데이터의 불가역성을 기반으로 네트워크 상에서 특정 데이터 및 로직을 신뢰할 수 있게 만들어주어 중앙화된 서비스보다 신뢰할 수 있는 서비스를 구축할 수 있어 3자가 개입하지 않고 거래 당사자들 끼리 신뢰할 수 있는 거래가 가능함
>>>- 블록체인 기술을 활용한 NFT 라는 디지털 자산의 소유주를 증명하는 가상의 토큰을 발급하여 증명서로 활용하여 소유권을 증명할 수 있음
>- 문제3 : Ropsten 테스트넷 동기화
>>- Day2 사전 작업 : 퍼블릭 네트워크인 Ropsten 테스트넷 활용
>>- 시간이 소요되므로 실습 하루 전 동기화 필요
>>- 하드웨어 권장 사양
>>>- CPU : Intel Core 4th Gen Series +
>>>- Memory : 4GB +
>>>- Storage : SSD 256GB +
>>>- Internet Speed : 100Mbps +
>>- 동기화 폴더 생성
>>>- Ropsten 테스트넷 동기화를 위해 C:\Users\{username}\workspace\datadir 폴더 생성
>>- 계정 생성
>>>- geth --datadir .\datadir\ account new
>>- Ropsten Faucet 에서 이더받기
>>>- 테스트 이더 받기 (왼쪽) : https://faucet.ropsten.be/
>>>>- 해당 사이트는 중지되었으므로 https://faucet.egorfine.com/ 사이트를 이용하는 것을 추천함
>>>- 이더스캔에서 결과 확인 (오른쪽)
>>- 동기화를 위한 포트 허용
>>>- 방화벽 열기
>>>>- 방화벽 및 네트워크 보호 > 고급 설정 > 인바운드 규칙 > 새 규칙 추가
>>>>- TCP 30303, UDP 30303
>>- 노드로 구동하기
>>>- 롭슨 테스트넷에 참여하기
>>>>- geth --ropsten --datadir C:\Users\{username}\workspace\datadir\ --http --http.addr 0.0.0.0 --http.api eth,net,web3,personal --http.corsdomain * --allow-insecure-unlock
>>>- geth 명령어
>>>>- 참조 링크 : https://geth.ethereum.org/docs/interface/command-line-options
>>- 동기화 상태 확인
>>>- Geth 접속
>>>>- geth attach http://localhost:8545
>>>>- 동기화 완료까지 평균 8시간 소요
>>>- 상태 확인 명령
>>>```
>>># geth console 내부
>>>> net.listening
>>>> eth.syncing
>>>> net.peerCount
>>>```
>>>- 동기화 진행을 숫자로 보고 싶은 경우 아래와 같이 응용 가능
>>>>- eth.syncing.currentBlock / eth.syncing.highestBlock * 100
>>- 동기화 완료 확인
>>>- 이더 잔고 확인
>>>>- eth.getBalance(eth.accounts[0])

<br>

[목차로이동](#목차)

