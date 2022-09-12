
# CSR 이란
서버가 인증서 발급을 위해 CA로 요청하는 인증서 신청 형식을 말합니다.
CSR에는 다음과 같은 데이터가 포함됩니다.
- 인증서 적용 도메인 정보
- 서버의 공개키
- 외 메타 정보 (회사명 등)

CSR은 Base64 인코딩되고 아래와 같은 형태이고, 보통 private 키와 같이 만들어지거나 먼저 만들어야합니다. (public 키를 CSR에 포함시켜야 하기 때문에)
```
-----BEGIN CERTIFICATE REQUEST-----
MIICuzCCAaMCAQAwdjELMAkGA1UEBhMCS1IxDjAMBgNVBAgMBVNlb3VsMRIwEAYD
VQQHDAlTZW9jaG8tZ28xEjAQBgNVBAoMCU5hdmVyQ29ycDEUMBIGA1UECwwLU3lz
dGVtIFRlYW0xGTAXBgNVBAMMEHd3dy5zdGVucmluZS5jb20wggEiMA0GCSqGSIb3
DQEBAQUAA4IBDwAwggEKAoIBAQDRDmcdViBlMy7m+vjwmoCjzRKQwqoyhvLGHSOo
oUVRQ5mczzAtxS5uYEJo1YscfN9rmNii8xFeXb4VSOzLBKX0P+IXrRQNlH8k+/Nm
NQrkPuV/1q9ljwPCiPhsbUjmD0CZdYvgpcGf8rWL49uPdHX/Y2c9LQZDT1xSTdKF
U61JIJmnr1RPUKSRDZhChg2m9YORnAhU9j+nwQzIGMyOCd70BzSSMK32ZkVUqgXv
AkSJ2/cQLWoEqcBNHmG9odVcipbZuw7YJJlOnzrFFgJEvMAqcTswkd/c4tZOKwrX
vdkvraZ7TciDMUrvElK4tjrRFO5CLOKwpgYADRAjxYoC4AV3AgMBAAGgADANBgkq
hkiG9w0BAQUFAAOCAQEAbcV6YkhCkm2+lPhfF+ewflybLWxmOxAnOsAdsEbi/SUx
1WQF35CoXLCwlP41hEpzaI8/MFHWry2KdE3Usj/Td1l4PEwEGFvVyM4ItyXUpF76
QLmOQiKYRbn9OvYOY/g8iBNvubwE50LXugsk36RB18nLjSVdjlaD5Dp5tdf0nPBr
tz61ywYkBGZOQPUDg1yRMpFbCbfNgoVsPX5c5paZ3W91gJYE2jWnlxqVid1orzyS
R5OBbZLQ5mZn2T/2eyhxaWAE/TveK83N8rOjVl+oVvqXpyNayTgHZZn9pu7EHOfv
9gfblDBmXt+BXAsrolV4fD8XvkixaxT27fx+hPYCZQ==
-----END CERTIFICATE REQUEST-----
```

[[OpenSSL#1 CSR 만들기]] 을 참고해도 좋다.

CSR 생성 사이트 : https://www.anycert.co.kr/extra_svc/csr_setup

