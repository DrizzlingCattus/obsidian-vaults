
# options



# how-to

## Self-signed certification 생성하기
### 1. CSR 만들기
```bash
openssl genrsa -des3 -out server.key 2048
```
Private key를 먼저 만든다.
- genrsa
	- des3
	- out

```bash
openssl req -new -key server.key -out server.csr
```
Private key로 CSR을 만든다.
