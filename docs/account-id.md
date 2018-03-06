---
title: Finding Account IDs
slug: account-id
top_graphic: 1
date: 2016-08-10
lastmod: 2016-08-10
---

{{< lastmod >}}

When reporting issues it can be useful to provide your Let's Encrypt account ID.
Most of the time, the process of creating an account is handled automatically by
the ACME client software you use to talk to Let's Encrypt, and you may have
multiple accounts configured if you run ACME clients on multiple servers.

이슈를 제보할 때 Let's Encrypt 계정 ID를 제공하는 것이 유용할 수 있습니다. 대부분의 경우, 계정을 만드는 프로세스는 Let's Encrypt와 통신할 때 사용하는 ACME 클라이언트 소프트웨어에 의해 자동으로 처리됩니다. 그리고, 여러 서버에서 ACME 클라이언트를 실행하면 여러 계정이 구성될 수 있습니다.

Your account ID is a URL of the form
`https://acme-v01.api.letsencrypt.org/acme/reg/12345678`. You can also provide
just the digits at the end of that URL as a shorthand.

If you're using Certbot, you can find your account ID by looking at the "uri"
field in
`/etc/letsencrypt/accounts/acme-v01.api.letsencrypt.org/directory/*/regr.json`.

If you're using another ACME client, the instructions will be client-dependent.
Check your logs for URLs of the form described above. If your ACME client does
not record the account ID, you can retrieve it by submitting a new registration
request with the same key. See the [ACME spec for more
details](https://github.com/ietf-wg-acme/acme/blob/master/draft-ietf-acme-acme.md#registration).
You can also find the numeric form of your ID in the Boulder-Requester header in
the response to each POST your ACME client makes.
