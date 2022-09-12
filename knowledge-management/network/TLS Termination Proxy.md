
# TLS Termination Proxy란
외부 통신(public network)만 TLS로 수행하고 내부 통신(private network)은 암호화 없이 통신하기 위해 TLS 통신을 중계하는 Proxy.
하지만 [[mTLS (mutual TLS)]] 을 생각하면 내부 통신에도 결국 암호화가 필요한 경우가 있을 것으로 보인다.


![[Pasted image 20220912181652.png]]


# Reference
- https://en.wikipedia.org/wiki/TLS_termination_proxy