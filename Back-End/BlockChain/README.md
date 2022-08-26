# **블록체인** 🔗

## 목차
>- [프라이빗 네트워크 실습으로 이더리움 네트워크 시작하기](#프라이빗-네트워크-실습으로-이더리움-네트워크-시작하기)
>>- [이더리움 블록체인 네트워크의 분류1](#이더리움-블록체인-네트워크의-분류1)
>>- [이더리움 네트워크 개념도](#이더리움-네트워크-개념도)
>>- [프라이빗 네트워크 실습 환경설정](#프라이빗-네트워크-실습-환경설정)
>>- [로컬 네트워크 활용 및 실습](#로컬-네트워크-활용-및-실습)
>>- [블록체인 네트워크 실습1](#블록체인-네트워크-실습1)
>- [퍼블릭 블록체인 실습](#퍼블릭-블록체인-실습)
>>- [이더리움 블록체인 네트워크의 분류2](#이더리움-블록체인-네트워크의-분류2)
>>- [퍼블릭 블록체인 실습 환경설정](#퍼블릭-블록체인-실습-환경설정)
>>- [퍼블릭 네트워크 활용 및 실습](#퍼블릭-네트워크-활용-및-실습)
>>- [블록체인 네트워크 실습2](#블록체인-네트워크-실습2)
>- [스마트 컨트랙트 (SmartContract)](#스마트-컨트랙트-smartcontract)
>>- [SmartContract 란?](#smartcontract-란)
>>- [SmartContract 환경설정](#smartcontract-환경설정)
>>- [SmartContract 배포](#smartcontract-배포)
>>- [SmartContract 호출](#smartcontract-호출)
>>- [SmartContract 실습](#smartcontract-실습)
>- [Solidity ( 솔리디티 )](#solidity--솔리디티)
>>- [솔리디티 컨트랙트 기본 구조](#솔리디티-컨트랙트-기본-구조)
>>- [솔리디티 구현하며 기본기 익히기](#솔리디티-구현하며-기본기-익히기)
>>- [func()](#func)
>>- [currentCollection() & withdraw()](#currentcollection--withdraw)
>>- [솔리디티 실습](#솔리디티-실습)
>- [DApp : Web3 를 이용하여 이더리움과 상호작용하기](#dapp--web3-를-이용하여-이더리움과-상호작용하기)
>>- [DApp - Decentralized Application](#dapp---decentralized-application)
>>- [DApp 의 구성 요소](#dapp-의-구성-요소)
>>- [web3.js](#web3js)
>>- [web3.js 실습](#web3js-실습)
>>- [DApp 실습](#dapp-실습)

<br>

---

## 프라이빗 네트워크 실습으로 이더리움 네트워크 시작하기

>### 이더리움 블록체인 네트워크의 분류1
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

>### 프라이빗 네트워크 실습 환경설정
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

>### 블록체인 네트워크 실습1
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

---

## 퍼블릭 블록체인 실습

### 이더리움 블록체인 네트워크의 분류2
>- 개요
>>- 프라이빗 네트워크는 진정한 의미의 블록체인으로 보기 어려움
>>- 블록체인은 수많은 사람들이 자료를 공유하고 자료에 대한 합의를 하는것이 기본적인 개념임
>- 퍼블릭 네트워크의 분류
>>- 메인넷이 기본임
>>- 메인넷에 들어가기전 마음껏 테스트하기 위한 환경인 테스트넷을 이더리움 재단에서 운영중임
>- 메인넷을 구성하는 다양한 이더리움 클라이언트 종류
>>- 다양한 종류가 있으나 geth가 80% 이상을 차지함
>>- geth는 이더리움 재단에서 공식적으로 제공하고 있고 많은 이더리움 클라이언트들이 geth로 구동되고 있기 때문임

<br>

[목차로이동](#목차)

### 퍼블릭 블록체인 실습 환경설정
>- Ropsten 네트워크 동기화 상태 확인
>>- Geth 접속
>>>- geth attach http://localhost:8545
>>>- 동기화 완료까지 평균 8시간 소요
>>- 상태 확인 명령
>>```
>># geth console 내부
>>> net.listening
>>> eth.syncing
>>> net.peerCount
>>```
>>>- 동기화 완료시 eth.syncing 명령에서 false 가 리턴됨
>>- 동기화 완료 확인
>>>- 이더 잔고 확인
>>>>- eth.getBalance(eth.accounts[0])

<br>

[목차로이동](#목차)

### 퍼블릭 네트워크 활용 및 실습
>- 지갑을 통해 네트워크 활용하기
>>1. 메타마스크와 노드 연결
>>>- 메타마스크 열고 Ropsten 네트워크에 연결
>>2. 계정 등록하기
>>>- 키스토어 가져오기 (import)
>>>>- Ropsten Faucet 으로부터 이더를 수령한 계정
>>3. 메타마스크로 트랜잭션 생성하기
>>>1. 메타마스크에서 계정 추가
>>>```
>>>personal.newAccount(PASSWORD)
>>>```
>>>>- geth console 에서 새로운 계정 생성
>>>2. 메타마스크에서 새로만든 계정으로 0.1Ether 전송
>>>3. 받은 계정의 잔액 확인하기
>>>>- 메타마스크에서 계정 가져오기로 확인
>>>>- geth console 에서 새로운 계정을 생성한 경우, 잔액 확인 방법
>>>>```
>>>>>eth.getBalance(eth.accounts[1])
>>>>>web3.fromWei(eth.getBalance(eth.accounts[1]), "ether")
>>>>```
>>- 메타마스크를 통한 네트워크 참여는 직접 노드를 통해 참여하는것이 아닌 어딘가에 있는 노드를 통해서 참여하고 있는 것임
>- 노드 서비스로 네트워크 활용하기
>>- 직접 엔드포인트를 점유해서 누군가가 제공해주는 노드에 직접 접속해서 퍼블릭 이더리움 네트워크에 참여할 수 있음
>>1. 노드 서비스 가입하기
>>>- Infura 회원 가입 및 프로젝트 생성
>>>>- https://infura.io/
>>>- 프로젝트 페이지 > SETTINGS
>>>- ENDPOINT를 Ropsten 으로 변경
>>>>- Infura Project 에서 생성된 ENDPOINT 는 고정된 값으로 이후 과정에서 이를 이용하기 위해 URL을 기록하는 것을 권장함
>>> 노드 이용을 위한 JSON RPC APIs
>>>- JSON RPC API 참고 링크
>>>>- https://eth.wiki/json-rpc/API
>>>>- https://infura.io/docs/ethereum#section/Make-Requests/JSON-RPC-Methods
>>2. 노드 서비스 이용하기
>>>- 노드 클라이언트 조회
>>>>- Powershell(Invoke-WebRequest) 혹은 cmd(curl) 에서 수행
>>>>```
>>>># cmd
>>>>> curl -X POST \
>>>>-H "Content-Type: application/json" \
>>>>-d '{"jsonrpc": "2.0", "id": 1, "method": "eth_blockNumber", "params": []}' \
>>>>"https://ropsten.infura.io/v3/PROJECT_ID"
>>>>
>>>># powershell
>>>>> $body='{
>>>>>> "jsonrpc": "2.0",
>>>>>> "method": "web3_clientVersion",
>>>>>> "params": [],
>>>>>> "id": 100
>>>>>> }'
>>>>> $R=Invoke-WebRequest https://ropsten.infura.io/v3/PROJECT_ID -method post -body $body -contenttype "application/json"
>>>>> $R
>>>>```
>- 노드로 직접 참여하기
>>1. 네트워크 동기화 완료 확인
>>>- 이더 잔고 확인
>>>>- eth.getBalance(eth.accounts[0])
>>2. geth console 이용하기
>>```
>># 연결성 확인 (Connectivity Check)
>>> net.listening
>>> net.peerCount
>>
>># 계정 생성
>>personal.newAccount("pasword")
>>
>># 트랜잭션 생성
>>## 트랜잭션 생성을 위한 계정 보안 해제 (Unlock)
>>> personal.unlockAccount(eth.accounts[0])
>>
>>## 트랜잭션 오브젝트 구조
>>> tx = { from: eth.accounts[0], to: eth.accounts[1], value: 1e17, gas: 90e3, gasPrice: 20e9, nonce: 0 }
>>
>>## 트랜잭션 보내기
>>> eth.sendTransaction(tx)
>>```

<br>

[목차로이동](#목차)

### 블록체인 네트워크 실습2
>- 문제1 : geth 를 통해 데이터를 담은 트랜잭션 생성하기
>>- 설명 : 과제는 eth console 내에서 진행함
>>>1. "hello ethereum" 메시지를 담은 트랜잭션을 보내기
>>>2. 트랜잭션 전송은 sendTransaction API 를 사용할것
>>>3. API 호출 후 트랜잭션 처리 결과를 확인하기
>>>4. getTransaction 과 getTransactionReceipt 를 사용해보고 비교하기
>>>5. 트랜잭션 결과를 Etherscan 에서 조회하기
>- 문제 해결
>```
># 보낼 주소와 받을 주소
>> from = eth.accounts[0]
>> to = eth.accounts[1]
>
># 메시지 필드 생성
>> data = web3.toHex("hello ethereum")
>
># 트랜잭션 메시지 필드
>> tx = { from: from, to: to, data: data }
>
># 주소 잠금 해제
>> personal.unlockAccount(from)
>
># 트랜잭션 전송
>> tx_hash = eth.sendTransaction(tx)
>
># 트랜잭션 조회
>> eth.getTransaction(tx_hash)
>> eth.getTransactionReceipt(tx_hash)
>## 전자 서명 시점(전송 시)에 변경되지 않는 정보가 생성된 것이 Transaction
>## TransactionReceipt 에는 Transaction 수행 결과가 기록되므로 cumulativeGasUsed, gasUsed, logs, status 등 Transaction 에는 없는 필드들을 확인할 수 있음
>```
>- 문제2 : geth의 또 다른 API 를 사용하여 트랜잭션 보내기
>>- 설명 : 과제는 eth console 내에서 진행함
>>>1. sendRawTransaction API 를 사용하기
>>>2. 보낼 주소와 받을 주소를 준비하여 0.1Ether 를 전송하고 API 호출 후 트랜잭션 처리 결과를 확인하기
>>>3. 받은 계정의 잔고를 확인하기
>>>4. 트랜잭션 결과를 Etherscan 에서 조회하기
>>>5. sendTransaction 과 sendRawTransaction 의 차이는 무엇인지 정리하기
>- 문제 해결
>>- sendRawTransaction 을 통해 트랜잭션 전송하는 순서
>>>1. 트랜잭션 생성
>>>2. 보내는 주소 잠금 해제
>>>3. 트랜잭션 서명 (Digital Signature)
>>>4. 트랜잭션 전송
>>>>- Hexadecimal character 로 출력된 raw transaction 을 sendRawTransaction 을 통해 전송
>>```
>># 트랜잭션 메시지 생성
>>## 보낼 주소와 받을 주소
>>> from = eth.accounts[0]
>>> to = eth.accounts[1]
>>
>>## raw transaction 을 생성하기 위해서는 nonce, gas, gasPrice 가 필수적으로 지정되어야 함
>>
>>## nonce : 특정 주소로부터 생성된 트랜잭션의 수를 나타내며 순차적으로 증가함
>>> nonce = eth.getTransactionCount(from)
>>
>>## gas, gasPrice : 이더 전송에 소요되는 가스는 21000 이므로 gas (gas limit) 을 알맞게 지정해야 함
>>## geth 의 gas 기본값은 90000(90e3), gasPrice 기본값은 20Gwei(20e9) 임
>>
>>> tx = { from: from, to: to, value: 1e17, gasPrice:20e9, gas:90e3, nonce:1 }
>>
>># 주소 잠금 해제
>>> personal.unlockAccount(from)
>>
>># 트랜잭션 서명
>>## sendTransaction 은 전자 서명까지 내부적으로 수행하지만, sendRawTransaction 은 이미 서명된 트랜잭션인 raw transaction 을 단순 전송하는 역할만 하므로 생성된 트랜잭션 메시지를 직접 전자 서명 해야함
>>> signed_tx = eth.signTransaction(tx)
>>
>># 트랜잭션 전송
>>tx_hash = eth.sendRawTransaction(signed_tx.raw)
>>
>># 트랜잭션 결과 확인
>>> eth.getTransaction
>>> eth.getTransactionReceipt
>>
>>## 주소 잔액 조회를 통한 결과 확인
>>> web3.fromWei(eth.getBalance(from), "ether")
>- 문제3 : 노드 서비스에 조회 요청 보내기
>>- 설명
>>>- Invoke-WebRequest(Powershell) 혹은 curl(명령프롬프트) 명령어를 사용하여 진행함
>>>- 노드 서비스로 다음 요청들을 보내기
>>>>1. 네트워크 ID 확인
>>>>2. 노드 상태 조회 : 동기화 상태, 최근 블록 번호, 피어 수, listening 상태
>>>>3. 블록 조회 : 조회할 블록 번호 : 9361332
>>>>4. 트랜잭션 조회
>>>>>- 조회할 트랜잭션 해시 : 0xa1aff823cdff25ea042feb606ebe46ca53de135c0903e7a9e758a5d33682b221
>>>>>- 해당 트랜잭션에 적힌 메시지가 무엇인지 확인하기
>- 문제 해결
>>- Infura 프로젝트에서 생성한 가용 엔트포인트를 먼저 준비하기
>>- Ropsten 네트워크 이용을 위한 엔트포인트여야 함
>>- Ethereum JSON RPC API 공식 문서에 따라 curl 명령어 답안이 포함되어 있음
>```
># 네트워크 ID 확인
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "eth_syncing", "params": [], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>
>## 결과 : 동기화하지 않은 상태임을 나타냄
>{ "jsonrpc": "2.0", "id": 1, "result": false }
>
># 노드 상태 조회 (최근 블록 번호)
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "eth_blockNumber", "params": [], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>
>## 결과 : 16진수로 표기된 latest block 번호를 수신함
>{ "jsonrpc": "2.0", "id": 1, "result": "0x8ed7ac" }
>
># 노드 상태 조회 (피어수)
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "net_peerCount", "params": [], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>
>## 결과 : 노드가 연결한 피어의 수가 16진수로 반환됨
>{ "jsonrpc": "2.0", "id": 1, "result": "0x64" }
>
># 노드 상태 조회 (listening 상태)
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "net_listening", "params": [], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>
>## 결과 : listening 이 활성화되어 block 을 수신할 수 있는 상태임을 의미함
>{ "jsonrpc": "2.0", "id": 1, "result": true }
>
># 블록 조회
>## 조회할 블록 번호 : 9361332
>## 요청을 위해 16진수로 변환한 파라미터가 필요
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "eth.getBlockByNumber", "params": ["0x8ed7b4", true], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>## 두 번쨰 flag 는 조회할 블록에 포함된 트랜잭션 목록까지 조회를 할지 여부임
>
># 트랜잭션 조회
>## 조회할 트랜잭션 번호 : 0xa1aff823cdff25ea042feb606ebe46ca53de135c0903e7a9e758a5d33682b221
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "eth_getTransactionByHash", "params": ["0xa1aff823cdff25ea042feb606ebe46ca53de135c0903e7a9e758a5d33682b221"], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>
>## 응답
>### 수신한 트랜잭션의 input : "0x68656c6c6f207373616679"
>### geth console 을 통해 확인
>> web3.toAscii("0x68656c6c6f207373616679")
>"hello ssafy"
>### 이더스캔에서 조회
>https://ropsten.etherscan.io/tx/0xa1aff823cdff25ea042feb606ebe46ca53de135c0903e7a9e758a5d33682b221
>```
>- 문제4 : 노드 서비스를 통해 트랜잭션 보내기
>>- 설명
>>>- Invoke-WebRequest(Powershell) 혹은 curl(명령프롬프트) 명령어를 사용하여 진행함
>>>- geth console 이 필요하다면 함께 사용함
>>>>1. Infura 에서 제공받은 엔트포인트로 트랜잭션을 보냄
>>>>>- Infura 노드를 통해 트랜잭션을 보내기 위해 사용할 수 있는 API 는 어떤 것이 있는지 생각해보고 과제를 수행함
>>>>>- 보낼 주소, 받는 주소를 준비하여 잔액을 확인하고 트랜잭션의 내용으로 이더와 메시지를 전송함
>- 문제 해결
>>- 노드 서비스를 통해 트랜잭션을 보내야 하므로 앞선 문제를 응용하여 raw transaction 을 우선 생성함
>>- sendRawTransaction 을 curl 로 요청하기 위한 JSON RPC API 형식을 확인함
>```
># Raw Transaction 생성
>## 문제2 에서 수행했던 것처럼 raw transaction 을 생성함
>
># 트랜잭션 전송
>## 요청
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "eth_sendRawTransaction", "params": ["RAW_TRANSACTION_HEX_STRING"], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>
>## 결과
>{ "jsonrpc": "2.0", "id": 1, "result": "TRANSACTION_HASH" }
>
>## 트랜잭션 조회를 통한 결과 확인 : eth.getTransaction, eth.getTransactionReceipt 를 통해 가능함
>> curl -X POST \
>-d '{"jsonrpc": "2.0", "method": "eth_getTransaction", "params": ["TRANSACTION_HASH"], "id": 1}' \
>-H "Content-Type: application/json" \
>"https://ropsten.infura.io/v3/PROJECT_ID"
>```
>- 문제5 : 네트워크 사용 방법의 장단점 비교하기
>>- 설명
>>>- 노드를 직접 구동하는 방법과 노드 서비스를 이용하는 방법의 차이와 장단점 비교하기
>- 문제 해결
>
>>&nbsp;| 노드를 직접 구동하는 방법 | 노드 서비스를 이용하는 방법
>>:--|:--|:--
>>장점|필요한 기능에 맞추어 노드를 구동할 수 있고 노드를 이용할 때 자유도가 높음|별도의 자원이 필요하지 않음
>>단점|노드를 구동하기 위한 자원이 필요하며 동기화 상태 유지를 위한 비용이 발생함<br>노드를 구동하고 운영하기 위한 사전 지식이 필요함|노드를 공유하므로 제한된 수의 요청만 보낼 수 있음<br>직접 구동하는 방법에 비해 자유도가 낮음


<br>

[목차로이동](#목차)

---

## 스마트 컨트랙트 (SmartContract)

>### SmartContract 란?
>- 개요
>>- Nick Szabo 에 의해 최초로 정의
>>- 스마트 하지 않은 단순 컴퓨터 프로그램, 법적 맥락 없음, 다소 잘못된 용어임에 불구하고 자리잡음
>>- 불록체인에서의 정의 : 불변의 컴퓨터 프로그램 (마스터링 이더리움 서적 참고)
>>>- 컴퓨터 프로그램
>>>- 불변 (immutable) 한번 배포되면 변경 불가
>>>- 결정적 (deterministic) 실행한 결과가 모두 같음
>>>- EVM 위에서 동작
>>>- 탈중앙화된 World Computer 동일한 상태를 유지
>- 배포와 호출 과정
>>- 배포
>>>1. 스마트 컨트랙트 코드 작성
>>>2. 1 을 컴파일하여 (ABI 생성) 바이드 코드
>>>3. 2 에서 트랜잭션을 생성하여 컨트랙트 배포 트랜잭션
>>>4. 3 을 트랜잭션 처리 (CA생성) 하여 블록에 담김
>>>5. 네트워크에 블록을 전파하여 블록 동기화
>>- 호출
>>>1. 트랜잭션 생성
>>>>- 사용자 계정 (EOA, Externall Owned Accounts)
>>>>- 컨트랙트 계정 (CA, Contract Accounts)
>>>>- ABI (Application Binary Interface)
>>>>- 함수의 주소, 매개변수
>>>2. 1의 컨트랙트 호출 트랜잭션 생성
>>>3. 2를 트랜잭션 처리하여 블록에 담김
>>>4. 3을 네트워크에 블록 전파하고 트랜잭션을 실행하여 블록 동기화

<br>

[목차로이동](#목차)


>### SmartContract 환경설정
>-  Remix IDE
>>- remix.ethereum.org (on chrome browser)
>>- 스마트 컨트랙트 IDE
>>>- 별도의 개발 환경 설정 없이 스마트 컨트랙트를 작성하고 배포, 호출

<br>

[목차로이동](#목차)

>### SmartContract 배포
>- 배포할 컨트랙트 준비
>>- Remix IDE 에서 기본 컨트렉트 1_Storage.sol 사용
>- 컴파일
>>- SOLIDITY COMPILER 탭에서 컴파일
>- 배포
>>- DEPLOY & RUN TRANSACTIONS 탭에서 배포

<br>

[목차로이동](#목차)

>### SmartContract 호출
>-  DEPLOY & RUN TRANSACTIONS 탭의 Deployed Contracts 항목에서 컨트렉트 호출 가능

<br>

[목차로이동](#목차)

>### SmartContract 실습
>- 문제1 : 리믹스에서 3_Ballot.sol 예제 코드 배포 & 호출해보기
>>1. 코드를 열고 컴파일
>>2. 배포탭에서 해당 내용을 인자로 입력하고 배포하기
>>>- ["0x7472756d70000000000000000000000000000000000000000000000000000000", "0x626964656e000000000000000000000000000000000000000000000000000000"]
>>- 배포결과
>><img width="738" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186648531-f42b6a32-7f5f-4476-8c78-1b1c29d351d4.PNG">
>>
>>- 컨트랙트 호출
>><img width="741" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186652364-a64bdfdb-f915-4c99-99ad-044f2d8e07d9.PNG">
>- 문제2 : 리믹스에서 Ganache 테스트넷에 컨트랙트 배포 & 호출해보기
>>- 설명
>>>- Ganache-cli 를 구동한 후 Ganache 테스트넷에 문제1에서 수행한 것과 같이 3_Ballot.sol 을 배포 & 호출하기
>>>- 배포 및 호출은 Remix IDE 를 통하여 수행하기
>- 해결
>>1. Ganache 구동
>>>- $ ganache-cli -d -m -a 5 -p 7545
>>2. DEPLOY & RUN TRANSACTIONS 에서 ENVIRONMENT 를 Ganache Provider 로 변경 후 엔트포인트 http://127.0.0.1:7545 로 지정
>>3. 변경된 network 정보와 Account 목록을 확인
>>4. 컨트랙트 배포
>>5. 가나슈에서 배포 결과 확인
>><img width="503" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186653514-a2505508-0ca6-4f1b-9d1c-394103fea984.PNG">
>- 문제3 : geth console 을 이용하여 Ganache 테스트넷에 컨트랙트 배포하기
>>- 설명
>>>- Ganache-cli 를 구동한 후 geth console 을 통해 Ganache 테스트넷에 3_Ballot.sol 을 배포 & 호출하기
>>>- 배포 및 호출도 geth console 을 이용하되, 컴파일된 ABI, Bytecode 를 이용하기 위해 Remix IDE 의 컴파일러 사용
>>><img width="478" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186657477-55fce025-4e69-4709-99c7-9a72a09197c2.PNG">
>- 문제4 : 리믹스와 MataMask 계정을 통해 Ganache 네트워크로 컨트랙트 배포해보기
>>- 설명
>>>- Ganache-cli 를 구동한 후 계정을 MetaMask 에 import 한 후 Remix를 통해 Ganache 테스트넷으로 3_Ballot.sol 을 배포하기
>- 해결
>>1. 가나슈 구동 후 MetaMask 로 가져오기 할 주소의 private key 복사
>>2. MetaMask 에서 가나슈로 연결하고 private key 로 지갑을 추가
>>3. Remix 와 MetaMask 연결하기
>>>- Remix 의 ENVIRONMENT 를 Injection Provider - Metamask 로 변경하면 MataMask가 자동으로 켜지면서 연동되며, Metamask 에서 연결됨 상태인지 확인
>>4. Remix 에서 컨트랙트 배포시도시 Metamask 의 승인창을 통해 배포됨
>>- 컨트랙트 배포 결과
>><img width="480" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186662646-fef010d2-b44a-4f03-b671-f62563d0a5e3.PNG">
>><img width="735" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186662773-04c847d9-06de-425c-9cdb-0e7ca9ddeb31.PNG">

<br>

[목차로이동](#목차)

---

## Solidity ( 솔리디티 )

>### 솔리디티 컨트랙트 기본 구조
>- sol 파일 레이아웃
><img width="404" alt="캡처" src="https://user-images.githubusercontent.com/65841586/186796992-296eecbb-e214-47ff-be73-57c291b66e09.PNG">
>>- 1번라인 : 소스코드의 SPDX 라이선스를 명시하도록 권장
>>- 3번라인 : 소스코드가 이용하는 솔리디티 컴파일러 버전 명시
>>- 컨트랙트의 범위 : contract ContractName{...}
>>- 상태 변수 ( State Variable )
>>>- 블록체인에 상태가 동기화되는 변수
>>>- 접근 제어자 지정 가능
>>>- 예시) uint : 부호 없는 정수 (unsigned integer)
>>- 함수
>>>- 컨트랙트의 단위 기능
>>>- 매개 변수, 제어자, 반환값 지정 가능

<br>

[목차로이동](#목차)

>### 솔리디티 구현하며 기본기 익히기
>- Fund Raising
>>- 개발환경 : Remix IDE
>>- 일회성으로 동작하는 모금 컨트랙트
>>>- 일정 기간 동안만 이더를 지불하여 모금에 참여할 수 있음
>>>- 펀드, 현재 모금액, 모금액 수령 기능을 제공함
>>- 솔리디티 0.7.0 문서 참조
>>>- https://docs.soliditylang.org/en/v0.7.0/index.html
>- sol 파일 레이아웃
>```sol
>// SPDX-License-Identifier: GPL-3.0
>
>pragma solidity >=0.7.0 <0.9.0;
>
>contract FundRaising {
>    
>}
>```
>- 생성자 생성
>>- 컨트랙트가 배포될 때 호출되는 특수 함수
>- 상태 변수 추가 - 모금 유효 기간, 수령자
>>- 컨트랙트 배포 시 모금 기간과 모금액 수령자 지정
>>>- uint duration :  몇 초 동안 모금이 유효한지 의미, 3600 = 1시간
>>>- 정수형 연산자 '+' : 현재 타임스탬프 + duration 을 ticketingCloses 의 값으로 지정
>>>- block.timestamp : 특수 전역 변수 중 하나로 현재 시각의 유닉스 타임 스탬프 값을 가짐
>>>- public 가시성 : 어디서나 열람 가능
>```sol
>// SPDX-License-Identifier: GPL-3.0
>
>pragma solidity >=0.7.0 <0.9.0;
>
>contract FundRaising {
>    uint public FundRaisingCloses;
>    address public beneficiary;
>
>    constructor(uint _duration, address _beneficiary) {
>        FundRaisingCloses = block.timestamp + _duration;
>        beneficiary = _beneficiary;
>    }
>}
>```
>- 상태 추가 - 최소 모금액 지정
>>- 최소 모금액 = 0.01 ether
>>- 이더리움 기본 단위 wei
>>>- 10^18 wei = 1 ether
>>- 1e16 == 0.01 ether == 10 ** 16
>- 함수 선언
>>- 필수 함수
>>>1. 모금 - fund
>>>2. 현재 모금액 - currentCollection
>>>3. 모금액 수령 - withdraw
>```
>contract FundRaising {
>  uint public constant MINIMUM_AMOUNT = 1e16;
>  uint public FundRaisingCloses;
>  address public beneficiary;
>
>  constructor(uint _duration, address _beneficiary) {
>    FundRaisingCloses = block.timestamp + _duration;
>    beneficiary = _beneficiary;
>  }
>
>  function fund() public {}
>  function currentCollection() public {}
>  function withdraw() public {}
>}
>```

<br>

[목차로이동](#목차)

>### fund()
>- 함수 구현 - 모금 함수 fund
>>- 요구사항
>>>1. 0.01 ether 이상으로 모금에 참여할 수 있음
>>>2. 지정된 모금 시간 이내에만 참여할 수 있음
>>>3. 모금이 완료되면 모금자를 저장함
>>- 이더를 받을 수 있는 payable 함수
>>>- msg.value : 트랜잭션에 얼마를 보냈는지 알 수 있는 전역 변수
>>- 유효성 체크 함수
>>>- require(판별문, "에러 메시지");
>>>- 판별문이 true 가 아닌 경우 "에러 메시지" 출력 후 함수 바로 종료
>>- 주소형 address
>>>- 이더리움 주소를 저장할 수 있는 자료형, 초기값은 0x0
>>- msg.sender
>>>- 메시지 송신자를 알 수 있는 전역 변수
>```
>// SPDX-License-Identifier: GPL-3.0
>
>pragma solidity >=0.7.0 <0.9.0;
>
>contract FundRaising {
>    uint public constant MINIMUM_AMOUNT = 1e16;
>    uint public FundRaisingCloses;
>    address public beneficiary;
>    address[] funders;
>
>    constructor(uint _duration, address _beneficiary) {
>        FundRaisingCloses = block.timestamp + _duration;
>        beneficiary = _beneficiary;
>    }
>
>    function fund() public payable {
>        require(msg.value >= MINIMUM_AMOUNT, "MINIMUM AMOUNT: 0.01 ether");
>        require(block.timestamp < FundRaisingCloses, "FUND RAISING CLOSED");
>
>        address funder = msg.sender;
>        funders.push(funder);
>    }
>    
>    function currentCollection() public {}
>    function withdraw() public {}
>}
>```

<br>

[목차로이동](#목차)

>### currentCollection() & withdraw()
>- 함수 구현 - 현재 모금액 : currentCollection
>>- 요구사항
>>>- 현재까지 모금된 금액을 누구나 확인할 수 있음
>>- 수의 반환값 선언
>>>- returns(type)
>>- 함수의 반환문 작성
>>>- address(this).balance
>>>- return address(this).balance;
>>- view
>>>- 상태 변수에 변화를 가하지 않고 읽기만 하는 함수
>```
>function currentCollection() public view returns(uint256) {
>  return address(this).balance;
>}
>```
>- 함수 구현 - 모금액 수령 : withdraw
>>- 요구사항
>>>1. 지정된 수령자만 호출할 수 있음
>>>2. 모금 종료 이후에만 호출할 수 있음
>>>3. 수령자에게 컨트랙트가 보유한 이더를 송금함
>>- 이더 전송이 일어나는 payable 함수
>>- 유효성 체크
>>- 함수 modifier 작성
>>- address 의 멤버 : balance, transfer
>>>- 컨트랙트가 보유한 이더 : <address>.balance
>>>- 요청 주소에게 컨트랙트 보유 이더 송금 : <address payable>.transfer(uint256 amount)
>```sol
>modifier onlyBeneficiary() {
>  require(msg.sender == beneficiary);
>  _;
>}
>
>function withdraw() public payable onlyBeneficiary {
>  require(block.timestamp > FundRaisingCloses);
>
>  payable(msg.sender).transfer(address(this).balance);
>}
>```
>- 전체 코드
>```
>// SPDX-License-Identifier: GPL-3.0
>
>pragma solidity >=0.7.0 <0.9.0;
>
>contract FundRaising {
>  uint public constant MINIMUM_AMOUNT = 1e16;
>  uint public FundRaisingCloses;
>  address public beneficiary;
>  address[] funders;
>
>  constructor(uint _duration, address _beneficiary) {
>    FundRaisingCloses = block.timestamp + _duration;
>    beneficiary = _beneficiary;
>  }
>
>  function fund() public payable {
>    require(msg.value >= MINIMUM_AMOUNT, "MINIMUM AMOUNT: 0.01 ether");
>    require(block.timestamp < FundRaisingCloses, "FUND RAISING CLOSED");
>
>    address funder = msg.sender;
>    funders.push(funder);
>  }
>
>  function currentCollection() public view returns(uint256) {
>    return address(this).balance;
>  }
>
>  modifier onlyBeneficiary() {
>    require(msg.sender == beneficiary);
>    _;
>  }
>
>  function withdraw() public payable onlyBeneficiary {
>    require(block.timestamp > FundRaisingCloses);
>
>    payable(msg.sender).transfer(address(this).balance);
>  }
>}
>```

<br>

[목차로이동](#목차)

>### 솔리디티 실습
>- 문제 요약
>
>번호|이름|산출물
>:-:|:--|:--
>1|withraw 함수 변경|withdraw 함수의 require 문을 modifier 로 변경<br>modifier 이름은 onlyAfterFundCloses
>2|추가 함수 구현<br>selectRandomFunder|모금자 주소를 Funder 중 한 명을 랜덤하게 반환하는 함수를 구현<br>SHA-3 인 keccak256 내장 함수를 이용하여 구현
>3|함수 수정<br>selectRandomFunder|문제 2 에서 구현한 함수를 모금액도 함께 반환하도록 수정<br>Array 를 사용하여 해결
>4|함수 개선<br>selectRandomFunder|문제 3 에선 모금자 계정과 모금액을 따로 관리해야해서 비효율적임<br>mapping 을 사용하도록 변경하여 관리하도록 개선
>- 문제 1 : withdraw 함수 변경
>```
>modifier onlyAfterFundCloses {
>  require(block.timestamp > FundRaisingCloses);
>  _;
>}
>
>function withdraw() public payable onlyBeneficiary onlyAfterFundCloses {
>  payable(msg.sender).transfer(address(this).balance);
>}
>```
>- 문제 2 : 함수 구현 - selectRandomFunder
>```
>function selectRandomFunder() public view returns (address) {
>  bytes32 rand = keccak256(abi.encodePacked(blockhash(block.number)));
>  address funder = funders[uint256(rand) % funders.length];
>
>  return funder;
>}
>```
>- 문제 3 : 함수 수정 - selectRandomFunder
>```
>contract FundRaising {
>  ...
>  uint[] public amount;
>
>  function fund() public payable {
>    ...
>    amount.push(msg.value);
>  }
>
>  ...
>
>  function selectRandomFunder() public view returns (address, uint256) {
>      ...
>      uint fundersAmount = amount[uint256(rand) % funders.length];
>
>      return (funder, fundersAmount);
>  }
>}
>```
>- 문제 4 : 함수 개선 - selectRandomFunder
>>- 최종 코드
>```
>// SPDX-License-Identifier: GPL-3.0
>
>pragma solidity >=0.7.0 <0.9.0;
>
>contract FundRaising {
>  uint public constant MINIMUM_AMOUNT = 1e16;
>  uint public FundRaisingCloses;
>  address public beneficiary;
>  mapping(address => uint256) funderToAmount;
>  address[] funders;
>
>  constructor(uint _duration, address _beneficiary) {
>    FundRaisingCloses = block.timestamp + _duration;
>    beneficiary = _beneficiary;
>  }
>
>  function fund() public payable {
>    require(msg.value >= MINIMUM_AMOUNT, "MINIMUM AMOUNT: 0.01 ether");
>    require(block.timestamp < FundRaisingCloses, "FUND RAISING CLOSED");
>
>    addFunder(msg.sender);
>    funderToAmount[msg.sender] += msg.value;
>  }
>
>  function addFunder(address _funder) internal {
>    if(funderToAmount[_funder] == 0) {
>      funders.push(_funder);
>    }
>  }
>
>  function currentCollection() public view returns(uint256) {
>    return address(this).balance;
>  }
>
>  modifier onlyBeneficiary() {
>    require(msg.sender == beneficiary);
>    _;
>  }
>
>  modifier onlyAfterFundCloses {
>    require(block.timestamp > FundRaisingCloses);
>    _;
>  }
>
>  function withdraw() public payable onlyBeneficiary onlyAfterFundCloses {
>    payable(msg.sender).transfer(address(this).balance);
>  }
>
>  function selectRandomFunder() public view returns (address, uint256) {
>    bytes32 rand = keccak256(abi.encodePacked(blockhash(block.number)));
>    address selected = funders[uint256(rand) % funders.length];
>
>    return (selected, funderToAmount[selected]);
>  }
>}
>```

<br>

[목차로이동](#목차)

---

## DApp : Web3 를 이용하여 이더리움과 상호작용하기

>### DApp - Decentralized Application
>- DApp 이란
>>- 탈중앙화된 P2P 네트워크 상에 백엔드 로직이 구동되는 응용프로그램
>>>- 블록체인 상의 스마트 컨트랙트가 기존의 중앙화된 서버에 의해 서비스를 제공하는 시스템 대체
>>- 더 좁은 의미에서 DApp 은 사용자 인터페이스를 통해 블록체인의 스마트 컨트랙트를 호출함으로써 동작하는 응용프로그램
>>- DApp = Frontend + Smart Contracts on Blockchain

<br>

[목차로이동](#목차)

>### DApp 의 구성 요소
>- 스마트 컨트랙트
>>- 서비스 로직이 구현된 이더리움 네트워크에 배포된 바이트코드
>- 사용자 인터페이스
>>- DApp 의 사용자 인터페이스
>>- 주로 HTML, CSS, Javascript 등 프론트엔드 기술로 구현
>- Web3 API for Javascript
>>- 이더리움 스마트 컨트랙트와 Javascript 코드 간의 상호작용 지원

<br>

[목차로이동](#목차)

>### web3.js
>- 이더리움 네트워크와 상호작용할 수 있게 하는 Javascript 라이브러리 모음

<br>

[목차로이동](#목차)

>### web3.js 실습
>- 실습 환경
>>- 준비
>>>- ganache-cli 구동
>>>>- ganache-cli -d -m -p 7545 -a 5
>- 프로젝트 생성 및 준비
>>- node.js 설치
>>- 폴더 생성
>>>- mkdir PROJECT_LOCATION
>>>- cd PROJECT_LOCATION
>>- web3.js 설치
>>>- npm i web3
>1. web3 객체 생성
>```
>// Add the web3 node module
>const Web3 = require('web3')
>
>// Ganache node on local environment
>const ENDPOINT = 'http://localhost:7545';
>
>const web3 = new Web3(new Web3.providers.HttpProvider(ENDPOINT));
>```
>>- 새로운 js 파일 생성
>>- web3 객체 생성
>2. 네트워크 기본 정보
>```
>// 네트워크 id : 현재 상호작용하는 노드가 속한 네트워크의 고유 번호
>web3.eth.net.getId()
>.then(id => console.log("Network Id: ", id));
>
>// 피어 수 : 노드와 직접 연결되어 있는 피어의 수
>web3.eth.net.getPeerCount()
>.then(peerCount => console.log("No. of Peers: ", peerCount));
>
>// 현재 블록 번호 : 네트워크에서 생성된 가장 최근 블록의 번호
>web3.eth.getBlockNumber()
>.then(blockNo => console.log("Latest Block Number: ", blockNo));
>```
>3. 트랜잭션 생성
>```
>web3.eth.sendTransaction({
>  from: FROM_ADDRESS,
>  to: TO_ADDRESS,
>  value: VALUE_IN_WEI
>})
>.on('transactionHash', hash => {...})
>.on('receipt', receipt => {...})
>.on('confirmation', confirmNum => {...})
>.on('error', console.error);
>```
>>- 비동기 처리 지원
>>>- PromiEvent
>>>>- sending
>>>>- sent
>>>>- transactionHash
>>>>- receipt
>>>>- confirmation
>>>>- error
>4. 트랜잭션 결과 확인
>```
>...
># 잔액 확인
>## 보낸 주소, 받은 주소의 잔액 확인
>## 출력 시 ether 단위로 변환
>.on('confirmation', confirmNum => {
>  // 잔액 확인
>  web3.eth.getBalance(fromAddr)
>  .then(balance => console.log(`${fromAddr}:${web3.utils.fromWei(balance, "ether")}
>  ether`));
>  ...
>})
>.on('error', console.error);
>
># 트랜잭션 해시로 검색
>web3.eth.getTranscation(TRANSACTION_HASH)
>.then(console.log)
>.catch(console.log);
>```
>5. 컨트랙트 배포하기
>>- Remix IDE 를 통해 ganache 에 1_Storage.sol 배포
>6. 컨트랙트와 상호작용하기
```
# 컨트랙트 접근을 위한 인스턴스 생성
## ABI
## CONTRACT_ADDRESS
const Web3 = require('web3')
const ENDPOINT = 'http://localhost:7545';

const web3 = new Web3(new Web3.providers.HttpProvider(ENDPOINT));

const abi = [
	{
		"inputs": [
			{
				"internalType": "uint256",
				"name": "num",
				"type": "uint256"
			}
		],
		"name": "store",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "retrieve",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
];

# retrieve() 호출
## 비용이 소요되지 않는 호출, call

const call = () => {
  const CONTRACT_ADDRESS = '0xe78A0F7E598Cc8b0Bb87894B0F60dD2a88d6a8Ab';
  const address = '0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1';
  const testContract = new web3.eth.Contract(abi, CONTRACT_ADDRESS);
  
  testContract.methods.retrieve().call({from: address}).then(console.log);
}
call();

# store() 호출
## 트랜잭션을 생성하는 send

const send = () => {
  const CONTRACT_ADDRESS = '0xe78A0F7E598Cc8b0Bb87894B0F60dD2a88d6a8Ab';
  const address = '0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1';
  const testContract = new web3.eth.Contract(abi, CONTRACT_ADDRESS);
  
  testContract.methods.store(100)
	.send({from: address}).then(console.log);
}
send();
```

<br>

[목차로이동](#목차)

>### DApp 실습
>- 문제1 : Web3 를 통해 컨트랙트 배포
>>- 설명 : ganache-cli 를 구동하고 1_Storage.sol 을 Web3 API 를 사용하여 배포하기
>- 해결
>>- 사전 준비
>>>- Remix IDE 를 통해 ganache-cli 에 컨트랙트를 호출하는 경우 절차
>>>>1. 가나슈 구동
>>>>2. MetaMask 를 가나슈 테스트넷에 연결
>>>>3. MetaMask 에 가나슈 계정 import
>>>>4. MetaMask 와 Remix IDE 연결
>```
># web3 인스턴스 초기화
>const Web3 = require('web3')
>const ENDPOINT = 'http://localhost:8545';
>const web3 = new Web3(new Web3.providers.HttpProvider(ENDPOINT));
>
># 컨트랙트 ABI, Bytecode 및 배포할 계정 정의
>const from = '0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1';
>const abi = [
>	{
>		"inputs": [],
>		"name": "retrieve",
>		"outputs": [
>			{
>				"internalType": "uint256",
>				"name": "",
>				"type": "uint256"
>			}
>		],
>		"stateMutability": "view",
>		"type": "function"
>	},
>	{
>		"inputs": [
>			{
>				"internalType": "uint256",
>				"name": "num",
>				"type": "uint256"
>			}
>		],
>		"name": "store",
>		"outputs": [],
>		"stateMutability": "nonpayable",
>		"type": "function"
>	}
>];
>const bytecode = '0x608060405234801561001057600080fd5b50610150806100206000396000f3fe608060405234801561001057600080fd5b50600436106100365760003560e01c80632e64cec11461003b5780636057361d14610059575b600080fd5b610043610075565b60405161005091906100d9565b60405180910390f35b610073600480360381019061006e919061009d565b61007e565b005b60008054905090565b8060008190555050565b60008135905061009781610103565b92915050565b6000602082840312156100b3576100b26100fe565b5b60006100c184828501610088565b91505092915050565b6100d3816100f4565b82525050565b60006020820190506100ee60008301846100ca565b92915050565b6000819050919050565b600080fd5b61010c816100f4565b811461011757600080fd5b5056fea26469706673582212209a159a4f3847890f10bfb87871a61eba91c5dbf5ee3cf6398207e292eee22a1664736f6c63430008070033'
>
># 컨트랙트 객체를 이용한 컨트랙트 배포
>## 컨트랙트 객체 생성
>const simpleStorageContract = new web3.eth.Contract(abi);
>
>## 컨트랙트 객체 배포
>simpleStorageContract.deploy({
>  data: bytecode,
>}).send({
>  from: address,
>  gasLimit: 3000000
>}).then(newContractInstance => {
>  console.log(`CA: ${newContractInstance.options.address}`)
>}).catch(e => console.log(e));
>```
>- 문제2 : FundRaising 연동하기
>>- Ropsten 테스트넷 동기화가 안되는 이슈가 있어 web3 호출 함수만 작성함
>```
>const Web3 = require('web3')
>const ENDPOINT = 'http://localhost:8545';
>const web3 = new Web3(new Web3.providers.HttpProvider(ENDPOINT));
>
>const address = '0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1';
>const abi = [
>	{
>		"inputs": [
>			{
>				"internalType": "uint256",
>				"name": "_duration",
>				"type": "uint256"
>			},
>			{
>				"internalType": "address",
>				"name": "_beneficiary",
>				"type": "address"
>			}
>		],
>		"stateMutability": "nonpayable",
>		"type": "constructor"
>	},
>	{
>		"inputs": [],
>		"name": "FundRaisingCloses",
>		"outputs": [
>			{
>				"internalType": "uint256",
>				"name": "",
>				"type": "uint256"
>			}
>		],
>		"stateMutability": "view",
>		"type": "function"
>	},
>	{
>		"inputs": [],
>		"name": "MINIMUM_AMOUNT",
>		"outputs": [
>			{
>				"internalType": "uint256",
>				"name": "",
>				"type": "uint256"
>			}
>		],
>		"stateMutability": "view",
>		"type": "function"
>	},
>	{
>		"inputs": [],
>		"name": "beneficiary",
>		"outputs": [
>			{
>				"internalType": "address",
>				"name": "",
>				"type": "address"
>			}
>		],
>		"stateMutability": "view",
>		"type": "function"
>	},
>	{
>		"inputs": [],
>		"name": "currentCollection",
>		"outputs": [
>			{
>				"internalType": "uint256",
>				"name": "",
>				"type": "uint256"
>			}
>		],
>		"stateMutability": "view",
>		"type": "function"
>	},
>	{
>		"inputs": [],
>		"name": "fund",
>		"outputs": [],
>		"stateMutability": "payable",
>		"type": "function"
>	},
>	{
>		"inputs": [],
>		"name": "selectRandomFunder",
>		"outputs": [
>			{
>				"internalType": "address",
>				"name": "",
>				"type": "address"
>			},
>			{
>				"internalType": "uint256",
>				"name": "",
>				"type": "uint256"
>			}
>		],
>		"stateMutability": "view",
>		"type": "function"
>	},
>	{
>		"inputs": [],
>		"name": "withdraw",
>		"outputs": [],
>		"stateMutability": "payable",
>		"type": "function"
>	}
>];
>const bytecode = '0x608060405234801561001057600080fd5b50604051620009ec380380620009ec833981810160405281019061003491906100b8565b814261004091906100f8565b60008190555080600160006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff16021790555050506101ec565b60008151905061009d816101be565b92915050565b6000815190506100b2816101d5565b92915050565b600080604083850312156100cf576100ce6101b9565b5b60006100dd858286016100a3565b92505060206100ee8582860161008e565b9150509250929050565b600061010382610180565b915061010e83610180565b9250827fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff038211156101435761014261018a565b5b828201905092915050565b600061015982610160565b9050919050565b600073ffffffffffffffffffffffffffffffffffffffff82169050919050565b6000819050919050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052601160045260246000fd5b600080fd5b6101c78161014e565b81146101d257600080fd5b50565b6101de81610180565b81146101e957600080fd5b50565b6107f080620001fc6000396000f3fe6080604052600436106100705760003560e01c8063604a7c921161004e578063604a7c92146100d5578063728a63a514610100578063b60d42881461012c578063d5799beb1461013657610070565b8063257d9bb81461007557806338af3eed146100a05780633ccfd60b146100cb575b600080fd5b34801561008157600080fd5b5061008a610161565b60405161009791906105d8565b60405180910390f35b3480156100ac57600080fd5b506100b561016c565b6040516100c29190610554565b60405180910390f35b6100d3610192565b005b3480156100e157600080fd5b506100ea610243565b6040516100f791906105d8565b60405180910390f35b34801561010c57600080fd5b5061011561024b565b60405161012392919061056f565b60405180910390f35b61013461031a565b005b34801561014257600080fd5b5061014b610409565b60405161015891906105d8565b60405180910390f35b662386f26fc1000081565b600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1681565b600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff16146101ec57600080fd5b60005442116101fa57600080fd5b3373ffffffffffffffffffffffffffffffffffffffff166108fc479081150290604051600060405180830381858888f19350505050158015610240573d6000803e3d6000fd5b50565b600047905090565b600080600043406040516020016102629190610539565b6040516020818303038152906040528051906020012090506000600380805490508360001c61029191906106aa565b815481106102a2576102a1610739565b5b9060005260206000200160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff16905080600260008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549350935050509091565b662386f26fc10000341015610364576040517f08c379a000000000000000000000000000000000000000000000000000000000815260040161035b906105b8565b60405180910390fd5b60005442106103a8576040517f08c379a000000000000000000000000000000000000000000000000000000000815260040161039f90610598565b60405180910390fd5b6103b13361040f565b34600260003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008282546104009190610604565b92505081905550565b60005481565b6000600260008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205414156104bb576003819080600181540180825580915050600190039060005260206000200160009091909190916101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b6104c78161065a565b82525050565b6104de6104d98261066c565b6106a0565b82525050565b60006104f16013836105f3565b91506104fc82610768565b602082019050919050565b6000610514601a836105f3565b915061051f82610791565b602082019050919050565b61053381610696565b82525050565b600061054582846104cd565b60208201915081905092915050565b600060208201905061056960008301846104be565b92915050565b600060408201905061058460008301856104be565b610591602083018461052a565b9392505050565b600060208201905081810360008301526105b1816104e4565b9050919050565b600060208201905081810360008301526105d181610507565b9050919050565b60006020820190506105ed600083018461052a565b92915050565b600082825260208201905092915050565b600061060f82610696565b915061061a83610696565b9250827fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff0382111561064f5761064e6106db565b5b828201905092915050565b600061066582610676565b9050919050565b6000819050919050565b600073ffffffffffffffffffffffffffffffffffffffff82169050919050565b6000819050919050565b6000819050919050565b60006106b582610696565b91506106c083610696565b9250826106d0576106cf61070a565b5b828206905092915050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052601160045260246000fd5b7f4e487b7100000000000000000000000000000000000000000000000000000000600052601260045260246000fd5b7f4e487b7100000000000000000000000000000000000000000000000000000000600052603260045260246000fd5b7f46554e442052414953494e4720434c4f53454400000000000000000000000000600082015250565b7f4d494e494d554d20414d4f554e543a20302e303120657468657200000000000060008201525056fea2646970667358221220a7006e003f1d34c3319a09f079fdd0f3709c2a59ecdaa4422835a28d5ddc54f764736f6c634300080700330000000000000000000000000000000000000000000000000000000000000e1000000000000000000000000090f8bf6a479f320ead074411a4b0e7944ea8c9c1'
>
>const fundRaising = new web3.eth.Contract(abi, address);
>
>const call = () => {
>  const tc = {from : from};
>  fundRaising.methods.beneficiary().call(tx).then(beneficiary => {
>    console.log("Beneficiary Address: ", beneficiary)});
>  fundRaising.methods.currentCollection().call(tx).then(currentCollection => {
>    console.log("Current Collection: ", currentCollection)
>  }).catch(console.error);
>  fundRaising.methods.fundRaisingCloses().call(tx).then(closesAt => {
>    console.log("Fund Close At: ", closesAt);
>  });
>  fundRaising.methods.selectRandomFunder().call(tx).then(result => {
>    console.log("Randomly Selected Funder Info: ", result[0], " (", result[1], " wei)");
>  });
>}
>
>const fund = async() => {
>  const tc = {from: from, value: 10e16, to: FUNDRAISING_CONTRACT_ADDRESS};
>  const gasAmount = await fundRaising.methods.fund().estimateGas(tx);
>  const count = await web3.eth.getTransactionCount(from);
>  tx.gas = gasAmount;
>  tx.gasPrice = web3.utils.toHex(web3.utils.toWei('20', 'gwei'));
>  tx.nonce = count;
>
>  const signedTx = await web3.eth.accounts.signTransaction(tx, privateKey);
>
>  web3.eth.sendSignedTracsaction(signedTx.rawTransaction).on('receipt', console.log);
>}
>```

<br>

[목차로이동](#목차)