---
title: Certificate Compatibility
slug: certificate-compatibility
top_graphic: 1
date: 2016-12-05
lastmod: 2016-12-05
---

{{< lastmod >}}

Let's Encrypt는 보안을 손상시키지 않으면서 최대한 많은 소프트웨어와 호환되는 것을 목표로 합니다. 특정 플랫폼에서 Let's Encrypt 인증서의 유효성을 검사 할 수 있는지 여부를 결정하는 주요 요인은 해당 플랫폼에 IdenTrust의 DST Root X3 인증서가 트러스트 스토어에 포함되어 있는지 여부입니다. 두 번째 요소는 해당 플랫폼이 현대적인 [SHA-2](https://konklone.com/post/why-google-is-hurrying-the-web-to-kill-sha-1) 인증서를 지원하는지 여부입니다. 모든 Let's Encrypt 인증서가 SHA-2를 사용하기 때문입니다.

인증서가 아래 "알려진 호환성" 플랫폼 중 일부에 유효성을 검사하지만 다른 인증서는 유효하지 않은 경우, 문제는 웹 서버 구성 오류일 수 있으며 대부분 올바른 인증서 체인을 제공하지 못하게 됩니다. [SSL Labs' Server Test](https://www.ssllabs.com/ssltest/)를 사용하여 사이트를 테스트하십시오. 그래도 문제가 확인되지 않으면 [커뮤니티 포럼](https://community.letsencrypt.org/)에 도움을 요청하십시오.

호환성에 대한 자세한 내용은 [이 커뮤니티 포럼 토론방](https://community.letsencrypt.org/t/which-browsers-and-operating-systems-support-lets-encrypt/)을 방문하십시오.

# 알려진 호환성 플랫폼

* Mozilla Firefox >= v2.0
* Google Chrome
* Internet Explorer on Windows XP SP3 and higher
* Microsoft Edge
* Android OS >= v2.3.6
* Safari >= v4.0 on macOS
* Safari on iOS >= v3.1
* Debian Linux >= v6
* Ubuntu Linux >= v12.04
* NSS Library >= v3.11.9
* Amazon FireOS (Silk Browser)
* Cyanogen > v10
* Jolla Sailfish OS > v1.1.2.16
* Kindle > v3.4.1
* Java 7 >= 7u111
* Java 8 >= 8u101
* Blackberry >= 10.3.3
* PS4 game console with firmware >= 5.00

# 알려진 비호환성 플랫폼

* Blackberry < v10.3.3
* Android < v2.3.6
* Nintendo 3DS
* Windows XP prior to SP3
  * cannot handle SHA-2 signed certificates
* Java 7 < 7u111
* Java 8 < 8u101
* Windows Live Mail (2012 mail client, not webmail)
  * cannot handle certificates without a CRL
* PS3 game console
* PS4 game console with firmware < 5.00
