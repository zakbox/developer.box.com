---
rank: 3
related_endpoints: []
related_guides: []
required_guides: []
related_resources: []
alias_paths:
  - /docs/branding-guidelines
category_id: best-practices
subcategory_id: null
is_index: false
id: best-practices/cors
type: guide
total_steps: 3
sibling_id: best-practices
parent_id: best-practices
next_page_id: best-practices
previous_page_id: best-practices/branding-guidelines
source_url: >-
  https://github.com/box/developer.box.com/blob/main/content/guides/best-practices/cors.md
fullyTranslated: true
---
# クロスオリジンリソース共有 (CORS)

[クロスオリジンリソース共有 (CORS)][mdn_cors] は、悪意のあるウェブサイトが明示的な権限を持たずに他のサイトのデータ (Box APIなど) にアクセスするのを防ぐために、ウェブブラウザで利用されているセキュリティメカニズムです。

<Message warning>

CORSは、ウェブブラウザを使用してウェブページから送信されるBox APIリクエストのみに適用され、そのブラウザによって渡される`HTTP Origin`ヘッダーを利用します。サーバー側環境では機能しません。

</Message>

<CTA to="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS">

CORSの全般的な情報の詳細については、MDN web docsを参照してください。

</CTA>

## CORSのしくみ

あるドメイン (`company.com`など) のブラウザで、別のドメイン (`box.com`) から画像、ファイル、またはAPIリソースを取得しようとする場合、適切なCORSヘッダーが存在しない限り、そのウェブブラウザにより、これらのアセットへのアクセスが阻止されます。

```mermaid;width=1000px
sequenceDiagram
    participant b as Browser (company.com)
    participant s1 as Server (company.com)
    participant s2 as Server (box.com)
    Note over b,s1: Same-origin request, always allowed
    b ->> s1: GET /data.json (Origin: https://company.com)
    s1 ->> b: JSON (No extra headers)
    Note over b,s2: Cross-origin request, controlled by CORS
    b ->> s2: GET /2.0/folders/0 (Origin: https://company.com)
    s2 ->> b: JSON (Access-Control-Allow-Origin: *)
```

ブラウザからクロスオリジンリクエストを送信すると、そのリクエストを送信するサイトのドメインを含む`Origin`リクエストヘッダーがリクエストとともに渡されます。このヘッダーは変更できないため、ウェブブラウザのセキュリティにとって重要な部分となります。

デフォルトでは、`Access-Control-Allow-Origin`応答ヘッダーが存在しない場合、ブラウザは、別のドメインから読み込まれたアセットを受け入れることはありません。Boxなどのサーバーは、そのサーバー上のリソースへのアクセスが許可されたドメインのリストを明示的に取得することも、APIへのアクセスを任意のドメインに許可するために`*`値を返すこともできます。

## BoxでのCORSの使用方法

Boxは、`Origin`リクエストヘッダーと`Access-Control-Allow-Origin`応答ヘッダーを使用して、開発者が定義したCORSルールを適用します。

### `Origin`ヘッダーの検証

Box APIは、アプリケーション開発者が設定した、許可されたドメインのリストに対して`Origin`リクエストヘッダーを検証します。許可されたオリジンは複数設定でき、リストにないオリジンは`HTTP 403`エラーで返されます。

<!-- markdownlint-disable line-length -->

```json
{
  "type":"error",
  "status":403,
  "code":"cors_origin_not_whitelisted",
  "context_info":{
    "origin":"https:\/\/company.com"
  },
  "help_url":"http:\/\/developers.box.com\/docs\/#errors",
  "message":"Access denied - Did you forget to safelist your origin in the CORS config of your app?",
  "request_id":"4dsdfsa832213"
}
```

<!-- markdownlint-enable line-length -->

オリジンが設定されていない場合は、このアプリケーションのBox APIに対するすべてのリクエストでエラーが返されます。

### `Access-Control-Allow-Origin`応答ヘッダー

Box APIは、`Origin`ヘッダーを検証した後、リクエストされたデータのほか、値`*`が設定された`Origin`応答ヘッダーを返します。

```yaml
HTTP/1.1 200 OK
Date: Wed, 23 Sep 2020 14:07:29 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
Cache-Control: no-cache, no-store
Access-Control-Allow-Origin: *
Vary: Origin
BOX-REQUEST-ID: 032cfb446dae4fd0b4c2bff80a1a97ba7
```

このヘッダーを返すことにより、Box APIはウェブブラウザに対して、データをリクエストしたサイトで応答を使用できることを通知します。

## ドメインのCORSの有効化

アプリケーションが動作するドメインでCORSを有効にするには、開発者コンソールに移動して、アプリケーションを選択し、\[**構成**] パネルの一番の下までスクロールして \[**CORSドメイン**] の設定を見つけます。

アプリケーションでのAPIリクエストの発信元になると予想されるすべてのオリジンをカンマ区切りリストとして追加します。ドメインにはスキーマ(`http`または`https`)が必要で、`*.example.com`のようにサブドメインのワイルドカードを含めることができます。

サイトが非標準のポート上で動作する場合は、サイトにそのポートも含める必要があります。これは、特に、`localhost`または`127.0.0.1`で動作しているサイトに当てはまります。

オリジンのリストの例は、次のようになります。

```sh
https://company.com,https://*.internal.company.com,http://localhost:3000
```

## CORSのデバッグ

Box APIに対してAPI呼び出しを実行した場合に発生する可能性のあるCORSエラーはいくつかあります。

### `HTTP 403` - 許可済みオリジンが定義されていない

オリジンのリストを指定した後でも、このエラーが発生する場合があります。多くの場合、指定したオリジンに誤字があることが原因です。

1. **オリジンを確認する** - 開発者コンソールに戻り、オリジンがAPI呼び出しの実行元のサイトをマップしていることを確認します。オリジンにはスキーム (`http(s)`) が含まれても、パスまたは末尾の`/`は含まれないことに注意してください。ブラウザのデバッグコンソールを使用してページを調査し、`Origin`リクエストヘッダーの値を確認することをお勧めします。この値は、開発者コンソールで指定された値のいずれかと一致する必要があります。
2. **資格情報を確認する** - このエラーのもう1つの理由として、オリジンを設定したアプリケーションとは別のアプリケーションとして認証している可能性が考えられます。資格情報が、使用するアプリケーションのものと一致することを確認してください。サーバー側スクリプトから呼び出しを実行して、API呼び出しが動作するかどうかを検証することをお勧めします。

### `Cross-Origin Request Blocked`

場合によっては、CORSに言及するJavaScriptエラーが発生します。

```sh
Cross-Origin Request Blocked: The Same Origin Policy disallows reading
the remote resource at https://api.box.com/2.0/users/me. (Reason: CORS
request did not succeed).
```

多くの場合、このエラーはCORSとはほとんど関係がありません。代わりに、以下を確認することをお勧めします。

1. **認証ヘッダーを確認する** - 認証ヘッダーが指定されていないか、またはその形式が正しくない場合、このAPIでは、必要な`Access-Control-Allow-Origin`ヘッダーなしで一般的なエラーが返されます。その結果、ブラウザで前述のエラーが発生します。必ず`Authorization: Bearer ...`ヘッダーを使用してアクセストークンを渡してください。
2. **VPNやプロキシなどでブロックされたリクエストがないか確認する** - 場合によっては、VPN、会社のプロキシ、ブラウザの機能拡張、DNSプロバイダ、またはネットワークトラフィックを妨害する可能性があるその他のサービスによってBox APIがブロックされている可能性があります。このようなサービスはどれも、リクエストをインターセプトし、必要な`Access-Control-Allow-Origin`ヘッダーが含まれていないまったく新しいリクエストを返す場合があります。このケースをテストするには、ブラウザ以外の環境、シークレットウィンドウ、またはまったく別の (会社が所有していない) デバイスから同じAPI呼び出しを実行してみてください。

[mdn_cors]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
