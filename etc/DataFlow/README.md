# **Data Flow**

>### Data Flow 란
>- 3 Tier
>>- Presentation Tier
>>- Logic Tier (Application Tier)
>>- Data Tier
>- Monolithic Architecture of Thtree Tier
>>- Presentation Tier
>>>- 클라이언트
>>>- Protocol(Http://, socket.io, MQTT, Stomp)
>>- Logic Tier (Application Tier)
>>>- Nginx, Apache
>>>- Load Balancer, L4, HA Proxy
>>>- Log4J (입력 및 출력시)
>>>- Docker
>>>- Jenkins
>>>- k8s, argo
>>>- JPA(Hibernate), MyBatis, elasticsearch
>>- Data Tier
>>>- elastic
>>>- Firebase
>- Micro Service Architecture
>>- 모놀리식과의 차이점
>>>1. DB가 N개
>>>2. BFF (Backend For Frontend) 필요 (GateWay)
>>>3. 각각의 BFF 서버가 각각의 DB 와 elastic 에 접속할때 커넥션이 매우 복잡해지므로 관리도구가 필요
>>>>- RabbitMQ 또는 Kafka
>- HTTPS
>>- Certbot - TLS 지원
>>- 구간 암호화를 통한 중간 탈취 방지
>>>- 클라이언트 요청시 암호화, 서버에서 복호화
>- Encrypt
>>- 단방향
>>>- SHA1-1024, Bcrypt 등
>>- 양방향
>>>- AES256 등
>- Web Arichitecture 101
>><img width="504" alt="캡처" src="https://user-images.githubusercontent.com/65841586/189296026-c7d174cc-3730-4f59-b8c3-363e84c0accb.PNG">