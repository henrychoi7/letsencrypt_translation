---
title: Integration Guide
slug: integration-guide
top_graphic: 1
date: 2016-08-08
lastmod: 2018-02-21
---

{{< lastmod >}}

This document contains helpful advice if you are a hosting provider or large website integrating Let's Encrypt, or you are writing client software for Let's Encrypt.
이 글은 호스팅 제공자나 큰 웹사이트 또는 Let's Encrypt에 대한 클라이언트 소프트웨어를 만들고 있는 귀하가 Let's Encrypt와 통합되길 원할 때 도움이 되는 조언을 포함하고 있습니다.

# 변화 계획

Both Let's Encrypt and the Web PKI will continue to evolve over time.  You should make sure you have the ability to easily update all services that use Let's Encrypt. If you're also deploying clients that rely on Let's Encrypt certificates, make especially sure that those clients receive regular updates.
Let's Encrypt와 웹 PKI는 성장을 계속할 것입니다. 귀하는 Let's Encrypt를 사용하면서 모든 서비스들을 쉽게 업데이트할 능력이 있다는 것을 확신할 수 있습니다. Let's Encrypt 증명서들을 신뢰하는 클라이언트들에게 배포할 것이라면, 정기적인 업데이트들을 받는 것을 확신하세요.

In the future, these things are likely to change:
앞으로는, 이러한 것들이 변화할 예정입니다:

  * the root and intermediate certificates from which we issue
  * the hash algorithms we use when signing certificates
  * the types of keys and key strength checks for which we are willing to sign end-entity certificates
  * and the ACME protocol
  ●  우리가 발급한 루트와 중급 단계의 인증서들
● 우리가 인증서 서명에 사용한 해시 알고리즘들
● 우리가 종단-엔티티 인증서들을 서명하기로 한 키들의 종류와 키 강도 점검
● 그리고 ACME 프로토콜

We will always aim to give as much advance notice as possible for such changes, though if a serious security flaw is found in some component we may need to make changes on a very short term or immediately. For intermediate changes in particular, you should not hardcode the intermediate to use, but should use the [`Link: rel="up"`](https://tools.ietf.org/html/draft-ietf-acme-acme-03#section-6.3.1) header from the ACME protocol, since intermediates are likely to change.
우리는 이러한 변경에 가능한 많은 사전 통지를 제공하는 것을 목표로 할 것입니다. 그러나 일부 구성 요소에서 심각한 보안 결함이 발견되는 경우, 매우 짧은 기간 또는 즉시 변경해야 할 수 있습니다. 특히, 중급 사항을 사용하려면, 중간체를 하드 코딩해서는 안되지만 중간체가 변경될 가능성이 있으므로 ACME 프로토콜의 Link: rel="up"헤더를 사용하시길 바랍니다.

Similarly, we're likely to change the URL of the terms of service (ToS) as we update it. Avoid hardcoding the ToS URL and instead rely on the [`Link: rel="terms-of-service"`](https://tools.ietf.org/html/draft-ietf-acme-acme-03#section-6.2) header to determine which ToS URL to use.
유사하게, 업데이트 시 URL의 서비스 약관(ToS)도 변경할 수 있습니다. 하드코딩된 ToS URL을 피하고 대신 Link: rel="terms-of-service" 헤더를 사용하여 사용할 ToS URL을 결정하세요.

You will also want a way to keep your TLS configuration up-to-date as new attacks are found on cipher suites or protocol versions.
새로운 공격이 암호화 스위트 또는 프로토콜 버전에서 발견될 때 마다 귀하의 TLS 구성을 최신 상태로 유지하는 방법이 필요할 것입니다.

# 업데이트 받기

To receive low-volume updates about important changes like the ones described
above, subscribe to our
[API Announcements](https://community.letsencrypt.org/t/about-the-api-announcements-category/23836) group.
This is useful for both client developers and hosting providers.
위에서 설명한 것과 같이 중요한 변화와 같은 소량 업데이트를 받으려면 우리 API Announcements 그룹에 가입하세요. 클라이언트 개발자와 호스팅 제공 업체 모두에게 유용합니다.

For higher-volume updates about maintenances and outages, visit our [status
page](https://letsencrypt.status.io/) and hit Subscribe in the upper right. This
is most useful for hosting providers.
유지 보수 및 중단에 대한 대량 업데이트는 상태 페이지를 방문하고 우측 상단의 가입을 눌러주세요. 호스팅 업체에게 가장 유용합니다.

Also, make sure you use a valid email address for your ACME account. We will use
that email to send you expiration notices and communicate about any issues
specific to your account.
또한 ACME 계정에 유효한 이메일 주소를 사용했는지 확인하세요. 해당 이메일로 만료 통지와 귀하의 계정과 관련된 모든 문제에 대해 알릴 예정입니다.

# 구독자는 누구인가

Our [CPS and Subscriber Agreement](/repository/) indicate that the Subscriber is whoever holds the private key for a certificate. For hosting providers, that's the provider, not the provider's customer. If you're writing software that people deploy themselves, that's whoever is deploying the software.
  CPS와 계약자 가입서는 가입자가 인증서의 개인키를 보유하고 있는 사람임을 나타냅니다. 호스팅 제공 업체의 경우 공급자의 고객이 아니라 공급자를 나타냅니다. 사람들이 스스로 배포하는 소프트웨어를 만드는 사람은 누구든지 소프트웨어를 배포하는 사람입니다.

The contact email provided when creating accounts (aka registrations) should go to the Subscriber. We'll send email to that address to warn of expiring certs, and notify about changes to our [privacy policy](/privacy).  If you're a hosting provider, those notifications should go to you rather than a customer. Ideally, set up a mailing list or alias so that multiple people can respond to notifications, in case you are on vacation.
구독자에게 가야하는 계정을 만들 때(등록이라고도 함) 이메일이 제공됩니다. 해당 주소로 유효 기간이 만료됨을 경고하거나 개인 정보 취급 방침의 변경 사항을 알리기 위해 이메일을 보낼 것입니다. 귀하가 호스트 제공 업체라면, 고객이 아닌 사용자에게 이러한 알림이 전달되어야 합니다. 귀하가 휴가일 때도 다양한 사람이 알림에 응답할 수 있도록 메일링 리스트나 별칭을 설정하는 것이 가장 이상적입니다.

The upshot of this is that, if you are a hosting provider, you do not need to send us your customers' email addresses or get them to agree to our Subscriber Agreement. You can simply issue certificates for the domains you control and start using them.
이 방법의 결과는 귀하 고객들의 이메일 주소를 우리에게 보내거나 가입자 계약에 동의하지 않아도 됩니다. 귀하가 제어하는 도메인에 대한 인증서를 발급하고 인증서를 사용하기만 하면 됩니다.

# 하나 또는 다수의 계정

In ACME, it's possible to create one account and use it for all authorizations and issuances, or create one account per customer. This flexibility may be valuable. For instance, some hosting providers may want to use one account per customer, and store the account keys in different contexts, so that an account key compromise doesn't allow issuance for all of their customers.
ACME에서는 모든 권한 부여 및 발급을 위해 하나의 계정을 만들어 사용하거나 고객 당 하나의 계정을 만들 수 있습니다. 이러한 유연성은 가치가 있습니다. 예를 들어, 호스팅 제공 업체에 따라 고객별로 하나의 계정을 사용하여 다른 컨텍스트에 계정 키를 저장함으로써 계정 키가 모든 고객에게 발급되지 못하도록 할 수 있습니다.

However, for most larger hosting providers we recommend using a single account and guarding the corresponding account key well. This makes it easier to identify certificates belonging to the same entity, easier to keep contact information up-to-date, and easier to provide rate limits adjustments if needed. We will be unable to effectively adjust rate limits if many different accounts are used.
그러나 대부분의 호스팅 업체의 경우는 단일 계정을 사용하여 해당 계정의 키를 보호하는 것을 권장합니다. 동일한 엔티티에 속하는 인증서를 더 쉽게 식별하고, 연락처 정보를 최신 상태로 유지하고, 필요한 경우 요금 제한 조정 기능을 제공하기 쉽습니다. 만약 많은 계정을 사용한다면, 우리는 효과적으로 요금 한도를 조절할 수 없을 것입니다.

# 멀티 도메인 (SAN) 인증서

Our [issuance policy](/docs/rate-limits/) allows for up to 100 names per certificate. Whether you use a separate certificate for every hostname, or group together many hostnames on a small number of certificates, is up to you.
우리의 인증서 발급 정책은 인증서당 100명까지 허용합니다. 호스트 이름마다 별도의 인증서를 사용할지 아니면 적은 수의 인증서로 많은 호스트 이름을 그룹화할지는 귀하가 선택하면 됩니다.

Using separate certificates per hostname means fewer moving parts are required to logically add and remove domains as they are provisioned and retired. Separate certificates also minimize certificate size, which can speed up HTTPS handshakes on low-bandwidth networks.
호스트 이름당 인증서를 사용하는 것은 프로비저닝 및 회수되는 도메인을 논리적으로 추가 및 제거하는데 필요한 부분이 줄어드는 것을 의미합니다. 별도의 인증서를 사용하면 인증서의 크기를 최소화 할 수 있으므로 낮은 대역폭 네트워크에서 HTTPS handshake의 속도를 높일 수 있습니다.

On the other hand, using large certificates with many hostnames allows you to manage fewer certificates overall. If you need to support older clients like Windows XP that do not support TLS Server Name Indication ([SNI](https://en.wikipedia.org/wiki/Server_Name_Indication)), you'll need a unique IP address for every certificate, so putting more names on each certificate reduces the number of IP addresses you'll need.
반면에 호스트 이름이 여러 개인 큰 인증서를 사용하면 전체적으로 더 적은 인증서를 관리할 수 있습니다. TLS 서버 이름 정보(SNI)를 지원하지 않는 윈도우 XP와 같은 이전 클라이언트를 지원하는 경우 모든 인증서에 대해 고유한 IP주소가 필요하므로 각 인증서에 더 많은 이름을 삽입하면 귀하가 필요로 하는 IP주소가 줄어들 것입니다.

For most deployments both choices offer the same security.
대부분의 배포에서는 두 옵션이 동일한 보안 수준을 제공합니다.

# 인증서와 키의 보관 및 재사용

A big part of Let's Encrypt's value is that it enables automatic issuance as part of provisioning a new website.  However, if you have infrastructure that may repeatedly create new frontends for the same website, those frontends should first try to use a certificate and private key from durable storage, and only issue a new one if no certificate is available, or all existing certificates are expired.
Let's Encrypt 값의 가장 큰 부분은 새로운 웹 사이트 프로비저닝의 부분으로 자동 발급을 허용하는 점입니다. 그러나 동일한 웹 사이트에 대해 새로운 프론트엔드를 반복적으로 만드는 인프라를 사용하는 경우에는 먼저 스토리지의 인증서와 개인키를 사용해야 하고, 사용 가능한 인증서가 없는 경우 새로 발급 받거나 모든 인증서가 만료되어야 합니다.

For Let's Encrypt, this helps us provide services efficiently to as many people as possible. For you, this ensures that you are able to deploy your website whenever you need to, regardless of the state of Let's Encrypt.
Let's Encrypt의 경우, 이를 통해 최대한 많은 사람들에게 효율적으로 서비스를 제공할 수 있습니다. 이렇게 하면 A의 상태에 관계 없이 웹 사이트를 배포할 수 있습니다.

As an example, many sites are starting to use Docker to provision new frontend instances as needed. If you set up your Docker containers to issue when they start up, and you don't store your certificates and keys durably, you are likely to hit rate limits if you bring up too many instances at once. In the worst case, if you have to destroy and re-create all of your instances at once, you may wind up in a situation where none of your instances is able to get a certificate, and your site is broken for several days until the rate limit expires. This type of problem isn't unique to rate limits, though. If Let's Encrypt is unavailable for any reason when you need to bring up your frontends, you would have the same problem.
예를 들어, 많은 사이트에서 필요에 따라 새 프론트엔드 인스턴스를 프로비저닝하는데 Docker를 사용하기 시작했습니다. Docker 컨테이너가 시작될 때 발급하도록 설정하고 인증서와 키들을 지속적으로 저장하지 않는 경우 너무 많은 인스턴스를 한번에 표시하면 요금 제한을 초과할 수 있습니다. 최악의 경우에는 모든 인스턴스를 한번에 삭제하고 다시 생성해야하는 경우 인증서를 받을 수 있는 인스턴스가 없어 사이트가 만료될 때까지 몇일 동안 손상될 수 있습니다. 하지만 이러한 유형의 문제는 등급 제한에만 국한되지 않습니다. 귀하의 프론트 엔드를 사용해야할 때 Let's Encrypt가 어떤 이유로든 이용 불가하다면, 같은 문제가 발생할 것입니다.

Note that some deployment philosophies state that crypto keys should never leave the physical machine on which they were generated. This model can work fine with Let's Encrypt, so long as you make sure that the machines and their data are long-lived, and you manage rate limits carefully.
일부 배포 철학에서는 암호 키가 생성된 물리적인 시스템을 그대로 두어서는 안된다고 설명합니다. 이 모델은 Let's Encrypt와 함께 잘 작동할 것이며, 시스템 및 데이터가 오래 지속된다면 속도 제한을 주의깊게 관리해야 합니다.

# 챌린지 타입 고르기

If you're using the http-01 ACME challenge, you will need to provision the challenge response to each of your frontends before notifying Let's Encrypt that you're ready to fulfill the challenge. If you have a large number of frontends, this may be challenging. In that case, using the dns-01 challenge is likely to be easier. Of course, if you have many geographically distributed DNS responders, you have to make sure the TXT record is available on each responder.
http-01 ACME 챌린지를 사용하는 경우, Let's Encrypt에 수행할 준비가 되어있는 챌린지를 알리기 전에 귀하의 각 프론트엔드에 챌린지 응답을 프로비저닝 해야합니다. 귀하가 많은 수의 프론트엔드를 소유하고 있다면, 이 것은 도전적일 것이다. 이러한 경우 dns-01 챌린지가 더 쉬울 수 있습니다. 물론 지리적으로 분산되어 있는 여러 DNS 알림이 있는 경우 각 응답자에 TXT 레코드가 있는지 확인해야 합니다.

Additionally, when using the dns-01 challenge, make sure to clean up old TXT records so the response to Let's Encrypt's query doesn't get too big.
또한, dns-01 챌린지를 사용하는 경우 Let's Encrypt의 쿼리 응답이 너무 커지지 않도록 TXT 레코드를 정리해야 합니다.

If you want to use the http-01 challenge anyhow, you may want to take advantage of HTTP redirects. You can set up each of your frontends to redirect /.well-known/acme-validation/XYZ to validation-server.example.com/XYZ for all XYZ. This delegates responsibility for issuance to validation-server, so you should protect that server well.
어떻게든 http-01 챌린지를 사용하고 싶다면, HTTP 리다이렉션을 활용하는 것이 좋습니다. 모든 XYZ에 대해 /.well-known/acme-validation/XYZ to validation-server.example.com/XYZ로 리다이렉션하도록 프론트엔드를 설정할 수 있습니다. 이는 유효성 검사 서버에 배포할 책임이 있으므로 해당 서버를 잘 보호해야 합니다.

# 중앙 유효 서버

Related to the above two points, it may make sense, if you have a lot of frontends, to use a smaller subset of servers to manage issuance. This makes it easier to use redirects for http-01 validation, and provides a place to store certificates and keys durably.
위의 두 가지 점과 관련하여, 귀하가 많은 프론트엔드를 가지고 있다면 발행 관리를 위해 소수의 서버로 구성하여 사용하는게 좋을 것입니다. http-01 유효성 검사를 쉽게 리다이렉트할 수 있고 인증서와 키를 안전하게 저장할 수 있는 공간을 제공할 수 있습니다.

# OCSP를 사용하지 않도록 구현

Many browsers will fetch OCSP from Let's Encrypt when they load your site. This is a [performance and privacy problem](https://blog.cloudflare.com/ocsp-stapling-how-cloudflare-just-made-ssl-30/).  Ideally, connections to your site should not wait for a secondary connection to Let's Encrypt. Also, OCSP requests tell Let's Encrypt which sites people are visiting. We have a good privacy policy and do not record individually identifying details from OCSP requests, we'd rather not even receive the data in the first place. Additionally, we anticipate our bandwidth costs for serving OCSP every time a browser visits a Let's Encrypt site for the first time will be a big part of our infrastructure expense.
많은 브라우저가 귀하의 페이지를 열 때 Let's Encrypt로 부터 OCSP를 불러올 것입니다. 이 것은 성능 및 프라이버시 문제입니다. 귀하의 페이지로의 연결은 Let's Encrypt로 보조 연결을 기다리지 않아야 합니다. 또한 OCSP는 사람들이 어떤 사이트를 방문하는지 Let's Encrypt에 요청합니다. 우리는 좋은 개인 정보 보호 정책을 가지고 있고, OCSP의 요청이 있어도 개인 정보를 기록하지 않으며, 우선 데이터를 받지 않는 것이 낫다고 생각합니다. 또한 OCSP에 제공하는 대역폭 비용은 처음으로 브라우저가 Let's Encrypt 사이트를 방문할 때마다 우리 인프라 비용의 큰 부분을 차지할 것으로 예상됩니다.

By turning on OCSP Stapling, you can improve the performance of your website, provide better privacy protections for your users, and help Let's Encrypt efficiently serve as many people as possible.
OCSP를 사용하지 않도록함으로써, 귀하의 웹 사이트 성능을 향상시킬 수 있고, 사용자에게 더 나은 개인 정보 보호 기능을 제공하고, Let's Encrypt가 최대한 많은 사람들에게 효율적으로 서비스할 수 있습니다.

# Let's Encrypt IP

Let's Encrypt will validate from a number of different IP addresses in the future, and will not announce which ones in advance. You should make sure your validation server is available to all IPs.
Let's Encrypt는 여러 IP로에서도 유효할 것이지만 사전에 어떤 IP인지 알려주지 않을 것입니다. 귀하의 유효성 검사 서버가 모든 IP에 대해 사용 가능한지 확인해야 합니다.

Some people who are issuing for non-HTTP services want to avoid exposing port 80 to anyone except Let's Encrypt's validation server. If you're in that category you may want to use the DNS challenge instead.
비 HTTPS 서비스에 대해 인증서 발급을 원하는 사용자는 Let's Encrypt의 유효성 검사 서버를 제외하고 누구에게도 80번 포트를 노출하지 않으려 합니다. 이 경우는 DNS 검사로 대신 수행할 수 있습니다.

# 지원하는 키 알고리즘

Let's Encrypt accepts RSA keys from 2048 to 4096 bits in length, and P-256 and P-384 ECDSA keys. That's true for both account keys and certificate keys. You can't reuse an account key as a certificate key.
Let's Encrypt는 2048 ~ 4096비트의 길이의 RSA 키와 P-256 및 P-384 ECDSA키를 사용합니다. 계정 키와 인증서 키 모두에 해당됩니다. 계정 키는 인증서 키로 재사용할 수 없습니다.

Our recommendation is to serve a dual-cert config, offering an RSA certificate by default, and a (much smaller) ECDSA certificate to those clients that indicate support.
우리의 권장 사항은 기본적으로 RSA 인증서를 제공하는 이중 인증서 구성과 지원을 나타내는 클라이언트에 대한 (더 작은) ECDSA 인증서를 제공하는 것입니다.

# 기본 HTTPS

For hosting providers, our recommendation is to automatically issue
certificates and configure HTTPS for all hostnames you control, and to offer a
user-configurable setting for whether to redirect HTTP URLs to their HTTPS
equivalents. We recommend that for existing accounts, the setting be disabled by
default, but for new accounts, the setting be enabled by default.
호스팅 제공 업체의 경우, 자동적으로 인증서를 발급하고 귀하의 설정에 따라 모든 호스트 이름에 대해 HTTPS를 구성하며, HTTP 또는 HTTPS URL로 리다이렉트 할지에 대해 사용자 설정을 제공하는 것이 좋습니다. 기존 계정의 경우 기본값으로 사용하지 않도록 설정되어있지만, 신규 계정의 경우 기본값으로 허용하도록 설정하는 것을 권장합니다.

Reasoning: Existing websites are likely to include some HTTP subresources
(scripts, CSS, and images). If those sites are automatically redirected to
their HTTPS versions, browsers will block some of those subresources due to
Mixed Content Blocking. This can break functionality on the site. However,
someone who creates a new site and finds that it redirects to HTTPS will
most likely include only HTTPS subresources, because if they try to include
an HTTP subresource they will notice immediately that it doesn't work.
추론: 기존 웹 사이트들은 HTTP 서브 리소스 (스크립트, CSS 및 이미지)를 포함할 수 있습니다. 이 사이트 들이 자동적으로 HTTPS 버전으로 리다이렉트 한다면 브라우저는 혼합된 컨텐츠 차단에 의해 이 서브 리소스들을 차단할 것입니다. 따라서 이 사이트의 기능을 제한할 수 있습니다. 그러나 새로 만들거나 HTTPS로 리다이렉트하는 사이트를 만든 사람은 HTTPS 서브 리소스만 포함했을 가능성이 높습니다. 만약 HTTP 서브 리소스를 포함하려고 한다면, 그 웹 사이트가 제대로 동작하지 않는다고 통지할 것입니다.

We recommend allowing customers to set an HTTP Strict-Transport-Security
(HSTS) header with a default max-age of sixty days. However, this setting
should be accompanied by a warning that if the customer needs to move to
a hosting provider that doesn't offer HTTPS, the cached HSTS setting in
browsers will make their site unavailable. Also, both customer and hosting
provider should be aware that the HSTS header will make certificate errors into
hard failures. For instance, while people can usually click through a browser
warning about a name mismatch or expired certificate, browsers do not allow such
a click through for hostnames with an active HSTS header.
우리는 고객들에게 기본값으로 설정된 60일의 기간을 가진 HTTP 제한 전송 보안(HSTS) 헤더를 사용하는 것을 권장합니다. 그러나 이 설정은 HTTPS를 제공하지 않는 호스팅 업체로 이동해야 하는 안내가 동반될 수 있고, 이미 HSTS가 설정된 브라우저는 그 사이트를 이용 불가할 것입니다. 고객과 호스트 제공 업체는 HSTS 헤더가 심각한 실패로 인증서 에러가 발생할 수 있다는 사실을 알아야합니다. 이 경우에 이름 불일치나 만료된 인증서에 대한 브라우저 경고를 클릭할 수 있지만, 브라우저는 활성 HSTS 헤더가 있는 호스트 이름을 클릭하는 것을 허용하지 않습니다.

# 갱신해야 할 시기

We recommend renewing certificates automatically when they have a third of their
total lifetime left. For Let's Encrypt's current 90-day certificates, that means
renewing 30 days before expiration.
전체 수명의 1/3이 남아 있는 경우 인증서를 자동으로 갱신하는 것이 좋습니다. 90일의 기간을 가진 Let's Encrypt의 인증서는 만료 30일 전에 갱신을 의미합니다.

If you are issuing for more than 10,000 hostnames, we also recommend automated
renewal in small runs, rather than batching up renewals into large chunks.
This reduces risk: If Let's Encrypt has an outage at the time you need to
renew, or there is a temporary failure in your renewal systems, it will only
affect a few of your certificates, rather than all of them. It also makes our
capacity planning easier.
10,000개 이상의 호스트 이름을 생성하는 경우 리뉴얼을 큰 부분으로 일괄 처리하는 대신 소규모 실행으로 자동 갱신하는 것도 권장합니다. 이 방법은 위험이 감소됩니다: Let's Encrypt에 갱신이 필요한 시점에 운영 중단이 있거나 시스템에 일시적인 장애가 있는 경우 모든 인증서가 아니라 일부 인증서에만 영향을 미칩니다. 또한 용량 계획이 쉬워 집니다.

You may want to bulk-issue certificates for all of your domains to get started
quickly, which is fine. You can then spread out renewal times by doing a
one-time process of renewing some certificates 1 day ahead of when you would
normally renew, some of them 2 days ahead, and so on.
모든 도메인이 신속하게 시작할 수 있도록 인증서를 다량 발행하는 것이 좋습니다. 그런 다음, 일반적으로 갱신하기 하루 전에 일부 인증서를 갱신하는 일회성 프로세스를 수행하고 일부는 일반적으로 이틀 전에 갱신하도록 하는 등의 방법으로 갱신 시간을 분산시킬 수 있습니다.

If you offer client software that automatically configures a periodic batch
job, please make sure to run at a randomized hour and minute during the day,
rather than always running at a specific time. This ensures that Let's Encrypt
doesn't receive arbitrary spikes of traffic at the top of the hour. Since Let's
Encrypt needs to provision capacity to meet peak load, reducing traffic spikes
can help keep our costs down.
정기 배치 작업을 자동으로 구성하는 클라이언트 소프트웨어를 제공하는 경우에는 항상 특정 시간에 실행되지 않고 당일 중 임의의 시간 간격으로 실행되도록 해주세요. 이렇게 하면 Let's Encrypt가 정상적으로 임의적인 트래픽 급증을 받지 않는 것을 보장합니다. Let's Encrypt가 최대 부하를 충족하기 위해 용량을 프로비저닝 해야하는 상황에서 트래픽 급증을 줄이는 것이 비용 절감에 도움이 될 수 있습니다.

# 실패 재시도

Renewal failure should not be treated as a fatal error. You should implement
graceful retry logic in your issuing services using an exponential backoff
pattern, maxing out at once per day per certificate. For instance, a reasonable
backoff schedul would be: 1st retry after one minute, 2nd retry after ten
minutes, third retry after 100 minutes, 4th and subsequent retries after one
day. You should of course have a way for administrators to
request early retries on a per-domain or global basis.
갱신 실패를 치명적인 오류로 간주해서는 안됩니다. 귀하의 발급 서비스에서 지수적인 백오프 패턴을 사용하여 인증서 하나당 하루에 한번씩 최댓값을 초과하는 우아한 재시도 논리를 구현해야 합니다. 예를 들어, 효과적인 백오프 일정은 다음과 같습니다: 1분 후에 첫 번째 시도, 10분 후에 두 번째 재시도, 100분 후에 세 번째 재시도, 하루 뒤에 4번째 그리고 연속적인 재시도입니다. 물론 관리자가 도메인 단위 또는 글로벌 단위로 초기 재시도를 요청할 수 있는 방법이 있습니다.

Backoffs on retry means that your issuance software should keep track of
failures as well as successes, and check if there was a recent failure before
attempting a fresh issuance. There's no point in attempting issuance hundreds
of times per hour, since repeated failures are likely to be persistent.
재시도를 철회한다는 것은 귀하의 발급 소프트웨어가 실패와 성공 여부를 계속 추적하고, 신규 발행을 시도하기 전에 최근에 실패한 부분이 있는지 확인한다는 것을 의미합니다. 반복적인 실패는 영구적일 수 있기 때문에 시간 당 수백번 발행을 시도하는 것은 의미가 없습니다.

All errors should be sent to the administrator in charge, in order to see if
specific problems need fixing.
모든 오류는 특정 문제가 해결해야 하는지 확인하기 위해 담당 관리자에게 전송되어야 합니다.
