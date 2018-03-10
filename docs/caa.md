---
title: Certificate Authority Authorization (CAA)
slug: caa
top_graphic: 1
date: 2017-07-27
lastmod: 2017-07-27
---

{{< lastmod >}}

CAA는 사이트 소유자가 도메인 이름을 포함한 인증서를 발급할 수 있는 CA (인증 기관)를 지정할 수 있도록 하는 DNS 레코드 유형입니다. CA가 의도하지 않은 인증서 오용의 위험을 줄이기 위해 [RFC 6844](https://tools.ietf.org/html/rfc6844)에 의해 2013년에 표준화되었습니다. 기본적으로 모든 공용 CA는 공용 DNS의 모든 도메인 이름에 대한 인증서를 발급할 수 있습니다 (단, 해당 도메인 이름의 제어를 확인한 경우). 즉 많은 공용 CA의 유효성 검사 프로세스 중 하나에 버그가 있는 경우, 모든 도메인 이름이 잠재적으로 영향을 받습니다. CAA는 도메인 소유자가 이 위험을 줄일 수 있는 방법을 제공합니다.

# CAA 사용하기

CAA에 대해 신경쓰지 않는다면 일반적으로 아무것도 할 필요가 없습니다 (아래 CAA 오류 참조). CAA를 사용하여 도메인의 인증서를 발급할 수 있는 인증 기관을 제한하려면 CAA 레코드 설정을 지원하는 DNS 공급자를 사용해야 합니다. 해당 공급자 목록을 보려면 [SSLMate의 CAA 페이지](https://sslmate.com/caa/support)를 확인하십시오. 공급자가 나열되면, [SSLMate의 CAA 레코드 생성기](https://sslmate.com/caa/)를 사용하여 허용하려는 CA를 나열한 CAA 레코드 집합을 생성할 수 있습니다.

Let's Encrypt의 CAA 도메인 이름 식별자는 `letsencrypt.org`입니다. 이것은 공식적으로 CPS (인증서 사용 약관) 4.2.1절에 문서화되어 있습니다.

## 레코드 저장 위치

You can set CAA records on your main domain, or at any depth of subdomain.
For instance, if you had `www.community.example.com`, you could set CAA records
for the full name, or for `community.example.com`, or for `example.com`. CAs
will check each version, from left to right, and stop as soon as they see any
CAA record. So for instance, a CAA record at `community.example.com` would take
precedence over one at `example.com`. Most people who add CAA records will want
to add them to their registered domain (`example.com`) so that they apply to all
subdomains. Also note that CAA records for subdomains take precedence over their
parent domains regardless of whether they are more permissive or more
restrictive. So a subdomain can loosen a restriction put in place by a parent
domain.

주 도메인이나 하위 도메인의 모든 CAA 레코드를 설정할 수 있습니다. 예를 들어,`www.community.example.com`이 있다면, 전체 이름이나`community.example.com` 또는`example.com`에 대한 CAA 레코드를 설정할 수 있습니다. CA는 각 버전을 왼쪽에서 오른쪽으로 검사하고 CAA 레코드를 보는 즉시 중지합니다. 예를 들어`community.example.com`의 CAA 레코드는`example.com '의 CAA 레코드보다 우선합니다. CAA 레코드를 추가하는 대부분의 사람들은 등록 된 도메인 (예 : example.com)에 추가하여 모든 하위 도메인에 적용하려고합니다. 또한 하위 도메인에 대한 CAA 레코드는 허용 또는 제한이 더 큰지 여부에 관계없이 상위 도메인보다 우선합니다. 따라서 하위 도메인은 상위 도메인에서 제한을 완화 할 수 있습니다.

CAA validation follows CNAMEs, like all other DNS requests. If
`www.community.example.com` is a CNAME to `web1.example.net`, the CA will first
request CAA records for `www.community.example.com`, then seeing that there is a
CNAME for that domain name instead of CAA records, will request CAA records for
`web1.example.net` instead. Note that if a domain name has a CNAME record, it is
not allowed to have any other records according to the DNS standards.

CAA 유효성 검사는 다른 모든 DNS 요청과 마찬가지로 CNAME을 따릅니다. `www.community.example.com`이`web1.example.net`에 대한 CNAME 인 경우 CA는 먼저`www.community.example.com`에 대한 CAA 레코드를 요청한 다음 해당 도메인에 대한 CNAME이 있음을 확인합니다 대신에 CAA 레코드를 요청할 것이고, 대신에`web1.example.net`에 대한 CAA 레코드를 요청할 것이다. 도메인 이름에 CNAME 레코드가있는 경우 DNS 표준에 따라 다른 레코드를 가질 수 없습니다.

The [CAA RFC](https://tools.ietf.org/html/rfc6844) specifies an additional
behavior called "tree-climbing" that requires CAs to also check the parent
domains of the result of CNAME resolution. This additional behavior was later
removed by [an erratum](https://www.rfc-editor.org/errata/eid5065), so Let's
Encrypt and other CAs do not implement it.

# CAA errors

Since Let's Encrypt checks CAA records before every certificate we issue, sometimes
we get errors even for domains that haven't set any CAA records. When we
get an error, there's no way to tell whether we are allowed to issue for the
affected domain, since there could be CAA records present that forbid issuance,
but are not visible because of the error.

If you receive CAA-related errors, try a few more times against our [staging
environment](/docs/staging-environment/) to see if they
are temporary or permanent. If they are permanent, you will need to file a
support issue with your DNS provider, or switch providers. If you're not sure
who your DNS provider is, ask your hosting provider.

Some DNS providers that are unfamiliar with CAA initially reply to problem
reports with "We do not support CAA records." Your DNS provider does not need
to specifically support CAA records; it only needs to reply with a
NOERROR response for unknown query types (including CAA). Returning other
opcodes, including NOTIMP, for unrecognized qtypes is a violation of [RFC
1035](https://tools.ietf.org/html/rfc1035), and needs to be fixed.

# SERVFAIL

One of the most common errors that people encounter is SERVFAIL. Most often this
indicates a failure of DNSSEC validation. If you get a SERVFAIL error, your
first step should be to use a DNSSEC debugger like
[dnsviz.net](http://dnsviz.net/). If that doesn't work, it's possible that your
nameservers generate incorrect signatures only when the response is empty. And
CAA responses are most commonly empty.  For instance, PowerDNS [had this bug in
version 4.0.3 and below](https://community.letsencrypt.org/t/caa-servfail-changes/38298/2?u=jsha).

If you don't have DNSSEC enabled and get a SERVFAIL, the second most likely
reason is that your authoritative nameserver returned NOTIMP, which as described
above is an RFC 1035 violation; it should instead return NOERROR with an empty
response. If this is the case, file a bug or a support ticket with your DNS provider.

Lastly, SERVFAILs may be caused by outages at your authoritative nameservers.
Check the NS records for your nameservers and ensure that each server is
available.

# Timeout

Sometimes CAA queries time out. That is, the authoritative name server never
replies with an answer at all, even after multiple retries. Most commonly this
happens when your nameserver has a misconfigured firewall in front of it that
drops DNS queries with unknown qtypes. File a support ticket with your DNS
provider and ask them if they have such a firewall configured.
