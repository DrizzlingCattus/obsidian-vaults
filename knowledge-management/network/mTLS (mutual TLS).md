
# mutual TLS 란 #anki 
'상호 인증 방법' 중 하나로서, 기존 TLS가 서버의 Certification만을 검증하는 일반적인 과정에서 Client Certification도 검증하는 과정을 추가로 거친다.
mTLS는 보통 Zero Trust한 환경에서 사용된다.

> Zero Trust means that no user, device, or network traffic is trusted by default, an approach that helps eliminate many security vulnerabilities.

![[Pasted image 20220912161334.png]]

# istio 에서의 mTLS
- 네트워크 간 보안이 필요한 이유 : istio ingress gateway 외부로 나가는 통신은 안전하게 보호될 수 있지만, k8s cluster 내부 통신은 효율을 위해 http를 사용하게 되므로 변조되거나 손실될 수 있다.
	- 수많은 마이크로서비스 모두에 https를 적용시키는건 https 처리를 위한 로직도 추가되어야 하므로 비효율적이다.
- 그렇다면, istio에선 어떤 방식으로 mTLS가 사용될까? (효율성 <--> 보안의 타협점)
	- Envoy Proxy와 Proxy 사이에서 이뤄지는 통신, 즉 pod 내부 통신이 아닌 cluster로 나갈 수 있는 통신에 대해서 mTLS를 적용하여 TLS가 아닌 모든 통신을 차단하게 만들었다.
	- Proxy에서 TLS 처리를 위임받았기에 마이크로서비스는 https인지 신경쓸 필요가 없다.
	- pod 내부 통신은 http를 사용하니 트래픽 비용에 대해서도 부담을 줄였다.

![[Pasted image 20220912160836.png]]


# Reference
- https://earth-95.tistory.com/140
- MSA에서의 mTLS: https://m.blog.naver.com/freepsw/221974847879
- cloudfare mTLS 문서: https://developers.cloudflare.com/cloudflare-one/identity/devices/access-integrations/mutual-tls-authentication/
- mtls 설명과 nginx 실습: https://www.jacobbaek.com/1040
- 왜 mtls가 필요한가? 어떤것을 막을 수 있는가? https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/