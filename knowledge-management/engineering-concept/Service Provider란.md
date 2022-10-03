# Service Provider란 (SPI, SPF)

# SP(Service Provider) 란

우선 Service Provider엔 2가지 종류가 있다.

-   SPI - Service Provider Interface
-   SPF - Service Provider Framework

3가지 개념을 이해해야함.

-   Service
    -   Service Provider 제공하는 역할.
    -   내부적인 기능은 구현하지 않고, Service Provider에 맡김.
    -   (내가 이해하기 쉬운 정의) service loader로 SPI와 Provider를 dynamic하게 binding 시켜서 원하는 Provider를 제공해주는 녀석.
-   Service Provider Interface
    -   서비스가 정의하는 public interface. 어플리케이션에서 직접 사용할 수 있는 인터페이스를 정의하고 있음.
-   Service Provider
    -   SPI 구현체. 개발자 벤더 고객에서 구현.

---

한번 예제를 보는게 이해하기 쉽다.

나 같은 경우는 java에서 제공하는 ServiceLoader의 동작을 살피니 바로 이해할 수 있었다.

```java
// SPI
interface CoffeeShop {
	fun getCoffee(customer: Customer, receipt: Receipt)
	fun doPayment(customer: Customer, payment: Payment)
  fun enrollCustomer(customer: Customer)
}

---

// SP
package com.coffee.starbucks
class StarBucks : CoffeeShop { ... }

---
// META-INF/Services
// 여기에 SP package를 등록하면 SPI에서 사용할 수 있도록 service loader가 로드 및 binding 해준다.

com.coffee.starbucks

---
// Service

val loader: ServiceLoader<CoffeeShop> = ServiceLoader.load(CoffeeShop::class.java)

for (service in loader) {
	service.getCoffee(...)
}
```

ref: [](https://velog.io/@jihoson94/Service-Provider-Framework-Interface)[https://velog.io/@jihoson94/Service-Provider-Framework-Interface](https://velog.io/@jihoson94/Service-Provider-Framework-Interface)