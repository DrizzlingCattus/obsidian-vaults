
# TLS 1.2에서 TLS 1.3으로 변화 #anki 
TLS 1.2가 가지고 있던 문제점이 몇 가지 존재한다.
- 새로운 커넥션을 맺기까지 느리다.
	- 암호화 방식을 협의하기 위해 필요한 RTT 수가 많다. (2회) - 비효율
- 너무 다양해진 암호화/키교환/HMAC 지원 방식에 따른 [[인증서 개념#Cipher suite anki]] format이 복잡해짐
- 취약한 보안을 가진 키 교환 방식이나 암호화 방식을 지원
	- PFS가 지원되지 않는 RSA, 정적 DH & RC4, DES, 3DES, MD5나 SHA-1과 같이 취약한 MAC 함수

## 1-RTT Handshake #anki 
암호화 방식을 협의하기 위해 RTT 수를 단 1회로 줄이도록 개선되었다.
TLS 1.2에서는 아래와 같이 handshake가 2회 필요했다.
![[Pasted image 20220912172004.png]]

TLS 1.3에서는 handshake가 1회만 이뤄진다.
![[Pasted image 20220912172013.png]]


## 0-RTT Resumption #anki 
TLS 1.2에서는 처음 핸드셰이크 과정을 거칠 때 ‘Session id’를 받아둔 후, 다시 핸드셰이크할 때 ‘Session id’와 서로 교환했던 키를 이용해 핸드셰이크를 마무리한다. 하지만 ‘Session id’를 이용해 TLS 핸드셰이크를 다시 진행할 때는 라운드 트립이 1번 발생해야 한다. TLS 1.3에서는 이 부분을 개선해 라운드 트립이 발생하지 않도록 만들었다.
TLS 1.3의 핸드셰이크는 키 하나를 만들어서 공유한다. 이 키를 PSK(Pre-Shared Key)라 부른다. 이를 암호화된 통신으로 주고받으면, 다음 커넥션이 맺어질 때 진행하는 핸드셰이크에서는 클라이언트가 PSK와 애플리케이션 데이터(Application Data)를 포함 시켜서 보낸다. (애플리케이션 계층이 HTTP라면 HTTP로 요청한다.) TLS 1.3에서 0-RTT 재개(0-RTT Resumption) 방식으로 TLS 핸드셰이크 과정을 거친다면, 핸드셰이크를 위한 클라이언트-서버 간 통신 없이도 새로운 TCP 커넥션을 맺을 수 있다.

```
          Client                                               Server

   Initial Handshake:
          ClientHello
          + key_share               -------->
                                                          ServerHello
                                                          + key_share
                                                {EncryptedExtensions}
                                                {CertificateRequest*}
                                                       {Certificate*}
                                                 {CertificateVerify*}
                                                           {Finished}
                                    <--------     [Application Data*]
          {Certificate*}
          {CertificateVerify*}
          {Finished}                -------->
                                    <--------      [NewSessionTicket]
          [Application Data]        <------->      [Application Data]




   Subsequent Handshake:
          ClientHello
          + key_share*
          + pre_shared_key          -------->
                                                          ServerHello
                                                     + pre_shared_key
                                                         + key_share*
                                                {EncryptedExtensions}
                                                           {Finished}
                                    <--------     [Application Data*]
          {Finished}                -------->
          [Application Data]        <------->      [Application Data]

               Figure 3: Message Flow for Resumption and PSK
```

> As the server is authenticating via a PSK, it does not send a
   Certificate or a CertificateVerify message.  When a client offers
   resumption via a PSK, it SHOULD also supply a "key_share" extension
   to the server to allow the server to decline resumption and fall back
   to a full handshake, if needed.  The server responds with a
   "pre_shared_key" extension to negotiate the use of PSK key
   establishment and can (as shown here) respond with a "key_share"
   extension to do (EC)DHE key establishment, thus providing forward
   secrecy.

```
        Client                                               Server

         ClientHello
         + early_data
         + key_share*
         + psk_key_exchange_modes
         + pre_shared_key
         (Application Data*)     -------->
                                                         ServerHello
                                                    + pre_shared_key
                                                        + key_share*
                                               {EncryptedExtensions}
                                                       + early_data*
                                                          {Finished}
                                 <--------       [Application Data*]
         (EndOfEarlyData)
         {Finished}              -------->
         [Application Data]      <------->        [Application Data]
```

>When clients and servers share a PSK (either obtained externally or
   via a previous handshake), TLS 1.3 allows clients to send data on the
   first flight ("early data").  The client uses the PSK to authenticate
   the server and to encrypt the early data.

## 보안 강화
Perfect Forward Secrecy가 지원되지 않는 키 교환 방식을 모두 지원 제거. (RSA, DH)
Message Authentication에 사용되는 보안에 취약한 해쉬 함수 지원 제거. (SHA-1, MD5)
KeyExchange 이후 통신은 암호화됨.

# Reference
- TLS 1.2와의 차이점 및 분석
	- http://www.secmem.org/blog/2021/08/21/TLS-13/
	- https://luavis.me/server/tls-1.3
- TLS 1.3 RFC 8446 : https://www.rfc-editor.org/rfc/rfc8446#section-1.2
- PFS (Perfect Forward Secrecy) https://rsec.kr/?p=465