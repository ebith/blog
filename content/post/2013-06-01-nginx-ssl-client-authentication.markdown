---
comments: true
date: 2013-06-01T00:00:00Z
published: true
title: Nginxでちょっと高度なクライアント認証する
url: /2013/06/01/nginx-ssl-client-authentication/
---

前にやった[オレオレ認証局をたててオレオレ証明書を発行する](http://blog.feelmy.net/2013/04/05/self-signed-certificate/)からもう一歩進む。  

## 証明書の中身で分岐したい
`$ssl_client_s_dn`にSubject(発行対象)、`$ssl_client_i_dn`にIssuer(発行者)の情報が入っていて、それぞれCN(Common Name), O(Organization)が含まれているのでこれらを見てよしなにする。  

### Common Nameにebithが含まれていない場合403 Forbiddenを返す
``` nginx
ssl on;
ssl_certificate server.crt;
ssl_certificate_key sever.key;
ssl_verify_client on;
ssl_client_certificate cacert.pem;
if ($ssl_client_s_dn !~ "CN=ebith") {
  return 403;
}
```

ルート証明書のインポートをお願いできる仲間内だと警告無しで正規のSSLと変わらない使い勝手でお手軽に暗号化できるし、今回のようにクライアント認証も使えば更に便利なので素晴らしい。
