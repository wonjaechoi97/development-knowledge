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

## 

>###
>- 