---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-14"

keywords: configuration domain, Free Trial plan, CIS instance

subcollection: cis

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}



# FAQ
{:#faq}

## 以前はカタログにあった Early Access プランはどうなったのでしょうか?
{:#cis-faq-early-access-plan}
{: faq}

Early Access プランは 2018 年 5 月 31 日付けでカタログから削除されました。標準有料プランと新しい 30 日間の無料トライアル・プランに置き換えられました。Early Access プランのインスタンスがある場合は、データが失われないように、直ちに標準プランにアップグレードしてください。Early Access ベータ版に加入していた場合は、無料トライアルのインスタンスの作成は許可されません。

## 無料トライアル・プランでは何ができますか?
{:#cis-faq-free-trial-plan}
{: faq}

無料トライアル・プランでは、設計上、1 アカウントにつき 1 ゾーンだけを使用できます。1 アカウントにつき 1 インスタンスだけを作成し、ゾーン名を確認することをお勧めします。 ゾーン名を確認してから追加することが重要です。 ゾーンを削除すると、無料トライアル・プランの間は別のゾーンも同じゾーンも追加できません。

## 無料トライアルのインスタンスはいくつまで持つことができますか?
{:#cis-faq-free-trial-instances}
{: faq}

1 アカウントにつき、そのアカウントの存続期間内で最大 1 つの無料トライアルのインスタンスを持つことができます。無料トライアル・インスタンスを既に持っている場合、無料トライアル・インスタンスを削除した場合、または無料トライアルの期限が切れた場合は、別の無料トライアル・インスタンスを作成することは許可されません。しかし、作成したかもしれない無料トライアルとは無関係に、その他の有料タイプのプラン (標準など) を作成できます。

## Early Access プランに加入しているサービス・インスタンスがあります。無料トライアルに変更できますか?
{:#cis-faq-early-access-to-free-trial-plan}
{: faq}

いいえ。Early Access プランは有料プランのみにアップグレードできます。現時点では標準プランが該当します。

## Early Access インスタンスがありましたが、削除したかどうか定かではありません。現時点で無料トライアル・インスタンスを作成できますか?
{:#cis-faq-early-access-and-free-trial-plan}
{: faq}

いいえ。各アカウントあたり 1 つの無料インスタンスのみの資格が付与されます。Early Access プランと、その置換後の無料トライアル・プランの両方とも、無料プランとしてカウントされます。つまり、無料トライアルのインスタンスを最大で 1 つしか持てないということです。

## 標準から無料トライアルにダウングレードできますか?
{:#cis-faq-downgrade-standard-to-free-plan}
{: faq}

いいえ。これは許可されません。

## 無料トライアルが期限切れになりました。どんなオプションがありますか?
{:#cis-faq-free-trial-plan-expired}
{: faq}

データが失われないように、有効期限日の前に無料トライアルから標準にアップグレードしなければなりません。アップグレード後には、プランのアップグレードか CIS インスタンスの削除のみサポートされています。(インスタンスの開始から) 45 日後にインスタンスのアップグレードや削除が行われていないと、構成ドメイン、GLB、プール、ヘルス・チェックが自動的に削除されます。

## どうすれば CIS インスタンスを削除できますか?
{:#{:#cis-faq-delete-instance}
{: faq}

CIS インスタンスを削除するには、最初に GLB、プール、ヘルス・チェックをすべて削除しなければなりません。次に、関連付けられているドメイン (ゾーン) を削除します。削除プロセスを開始するには、**「概要」**ページに移動し、**「サービスの詳細」**セクション内にあるドメイン名の隣のごみ箱アイコンをクリックします。

## 自分のアカウントにユーザーを追加し、Internet Services インスタンスの管理許可をこのユーザーに付与しました。このユーザーが認証の問題に直面しているのはなぜですか? 
{:#cis-faq-user-authentication-issue}
{: faq}

このユーザーに「サービスのアクセス役割」を割り当てていない可能性があります。「プラットフォームのアクセス」と「サービスのアクセス」という 2 つの別個の役割のセットがあることに注意してください。プラットフォームのアクセス役割はサービス・インスタンスを作成して管理するために必要ですが、サービスのアクセス役割はサービス・インスタンスに対するプラットフォーム・サービス固有の操作のために必要です。コンソール内で、**「管理」>「セキュリティー」>「ID およびアクセス (Identity and Access)」**を選択すると、これらの設定を更新できます。

## ドメインが「保留中」状態なのはなぜですか? どうすればドメインをアクティブにすることができますか?
{:#cis-faq-pending-domain}
{: faq}

ドメインを CIS に追加すると、レジストラー (または、サブドメインを追加した場合は、DNS プロバイダー) に構成するための 1 組のネームサーバーが表示されます。 それらのネームサーバーをお客様が正しく構成するまで、ドメインまたはサブドメインは保留中状態のままです。 必ず、両方のネームサーバーをレジストラーまたは DNS プロバイダーに追加してください。 IBM は、パブリック DNS システムを定期的にスキャンし、ネームサーバーが指示どおりに構成されたかどうかをチェックします。 ネームサーバーの変更を確認でき次第 (最大 24 時間かかる可能性があります)、IBM はお客様のドメインをアクティブ化します。 お客様は概要ページで**「ネームサーバーの再確認 (Recheck nameservers)」**をクリックして、ネームサーバーの再チェックを求める要求を送信できます。

## 私のドメインのレジストラーは誰ですか?
{:#cis-faq-who-is-registrar}
{: faq}

この情報は、https://whois.icann.org/ で調べてください。 **注意**: ドメインを CIS に追加したときにドメイン用に提供されるネームサーバーを更新または追加するには、レジストラーのドメイン構成を編集する管理特権を持っている必要があります。 CIS に追加しようとしているドメインのレジストラーがわからない場合、おそらく、レジストラーのドメイン構成を更新する権限はお客様にありません。 組織のドメインの管理者と一緒に、必要な変更を行ってください。

## 自分のドメイン (example.com) の現在の DNS プロバイダーを使用し続けたいと考えています。 現在の DNS プロバイダーからサブドメイン (subdomain.example.com) を CIS に委任することはできますか?
{:#cis-faq-keep-current-dns-provider}
{: faq}

はい。 このプロセスはドメインの追加と似ていますが、レジストラーではなく、上位ドメインの DNS プロバイダーで作業を行います。 サブドメインを CIS に追加すると、通常と同じく、構成するネームサーバーが 2 つ提供されます。 その 2 つのネームサーバーのそれぞれのネーム・サーバー (NS) レコードを、もう一方の DNS プロバイダーで管理されているドメイン内に DNS レコードとして構成します。 IBM は、必要な NS レコードが追加されたことを確認できたら、お客様のサブドメインをアクティブ化します。 組織内で上位ドメインを管理していない場合は、上位ドメインの管理者と一緒に作業を行い、NS レコードを追加する必要があります。

## TLS とは?
{:#cis-faq-what-is-tls}
{: faq}

TLS は、オンライン通信で Web サーバーとブラウザーの間に暗号化されたリンクを確立するための標準的なセキュリティー・プロトコルです。 Web サイトとの TLS 接続を作成するためには TLS 証明書が必要です。証明書にはドメイン名、会社名、追加データ (会社の住所、市、州、国、など) が含まれます。 証明書には期限日付や発行した認証局 (CA) の詳細なども記載されます。

## TLS はどのように機能しますか?
{:#cis-faq-how-does-tls-work}
{: faq}

ブラウザーは、TLS で保護されている Web サイトとの接続を開始するときに、まずサイトの TLS 証明書を取得して、その証明書がまだ有効かどうかをチェックします。 その CA がブラウザーが信頼する CA であること、また、証明書が被発行者の Web サイトに使用されていることを確認します。 こうしたチェックのいずれかが不合格になると、Web サイトが有効な証明書で保護されていないことを示す警告メッセージが表示されます。

TLS 証明書を Web サーバーにインストールすると、Web サーバーとそのサーバーに接続するブラウザーの間のセキュアな接続が可能になります。 Web サイトの URL の先頭は「http」ではなく「https」になり、アドレス・バーに南京錠が表示されます。 Web サイトで Extended Validation (EV) 証明書を使用している場合、ブラウザーには緑色のアドレス・バーも表示されます。

## なぜプライバシーの警告が表示されるのですか?
{:#cis-faq-privacy-warning}
{: faq}

IBM Cloud CIS が発行する TLS 証明書は、ルート・ドメイン (`example.com`) および第 1 レベルのサブドメイン (`*.example.com`) を対象としています。 第 2 レベルのサブドメイン (`*.*.example.com`) にアクセスしようとすると、ブラウザーにプライバシーに関する警告が表示されます。これは、これらのホスト名が SAN に追加されていないためです。

また、いずれかのパートナー認証局 (CA) が新規証明書を発行するまで、最大 15 分待ってください。 新規証明書が未発行の場合、ブラウザーにプライバシーに関する警告が表示されます。

## なぜ SSL 証明書が無効というエラーが表示されるのですか?
{:#cis-faq-invalid-ssl-cert-error}
{: faq}

サイトへのアクセス時に「エラー 526、SSL 証明書が無効です (Error 526, Invalid SSL Certificate)」が表示された場合は、起点証明書が無効である可能性があります。CIS プロキシーを有効にする場合、デフォルトの SSL モードの起点で、「エンドツーエンド - CA 署名」という有効な CA 署名証明書が必要です。以前の SSL モードのデフォルト設定は「エンドツーエンド - フレキシブル」だったことに注意してください。この場合、起点で提示される証明書の妥当性は無視されます。新しいデフォルトは、新たに追加されたドメインに限り適用されます。デフォルトの SSL モードがエンドツーエンド - フレキシブルだった時点で追加されたドメインの場合、この設定は上書きされません。このモードをより制限の緩いモードに変更できますが、実稼働環境の場合は推奨されていません。

## DDoS とは何ですか?
{:#cis-faq-what-is-ddos}
{: faq}

分散型サービス妨害 (DDoS) 攻撃とは、複数のソースからトラフィックを大量に送信することでオンライン・サービスを使用不可にする攻撃です。 DDoS 攻撃では、乗っ取られた複数のコンピューター・システムがサーバー、Web サイト、その他のネットワーク・リソースなどのターゲットを攻撃し、ターゲットのリソースのユーザーに影響を与えます。

ターゲット・システムに着信メッセージ、接続要求、不正な形式のパケットを大量送信することで、システムの処理を遅延させ、あるいは、クラッシュやシャットダウンを誘発して、正当なユーザーやシステムへのサービスを拒否させます。 DDoS 攻撃は、個人の犯罪者ハッカーから組織化された犯罪集団、政府機関まで、さまざまな攻撃者によって実行されています。

## DDoS 攻撃のターゲットになった場合の対応方法
{:#cis-faq-what-to-do-in-ddos}
{: faq}

**ステップ 1:** **「概要」**画面の「防御モード (Defense mode)」をオンにします。 

![防御モード](images/defense-mode.png)

**ステップ 2:** セキュリティーが最大になるように DNS レコードを設定します。

**ステップ 3:** IBM CIS からの要求の速度を制限したり抑えたりしないでください。IBM からこの状態に関する支援を受けるには、帯域幅が必要です。

**ステップ 4:** 必要に応じて、特定の国や訪問者をブロックします。

## 522 エラーが表示されました。何をすればよいですか?
{:#cis-faq-522-error}
{: faq}

522 エラーは、IBM がお客様のオリジン・サーバー (お客様のホスト) との接続を確立できなかったことを示しています。 接続に失敗した約 15 秒後に接続が閉じられ、522 エラー・ページが表示されます。

この問題は、通常、ファイアウォールまたはセキュリティー・ソフトウェアで誤って IBM の IP アドレスをブロックしたことが原因で発生します。 CIS はリバース・プロキシーとして機能するため、お客様のサイトへの接続は CIS の IP 範囲からの接続のように見えます。 この振る舞いにより、いくつかのファイアウォールがそれらの接続をブロックした結果、お客様のサイトの訪問者にコンテンツを正しく表示できないことがあります。

この問題を修正するには、CIS のすべての IP 範囲をホワイトリストに登録するようにホストに依頼してください (CIS の IP 範囲のリストについては[こちら](/docs/infrastructure/cis?topic=cis-ibm-cloud-cis-whitelisted-ip-addresses)を参照してください)。

522 エラーを回避するには、これらの IP のすべてをホワイトリストに登録する必要があります。 また、これらの範囲の IP がブロックされていないか確認することも大切です。

また、522 エラーはネットワーク接続の問題が原因で発生することもあるので、サーバーとネットワークが全体的に正常であり、過負荷になっていないことを確認してください。

前述の手順を実行してもまだエラーを受け取る場合は、IBM CIS サポートに連絡し、次を確認してください。

* IBM の IP 範囲をホワイトリストに登録済みであること
* サーバー/ネットワークがオンラインであり、全体的に正常であること

サポート・チームに連絡する場合は、最新の 522 エラーの ray ID を教えてください。 IBM はこれを使用して、お客様が利用していた CIS データ・センターを特定し、それ以降のテストを実行できます。

## プロキシー・レコードとは何ですか? なぜプロキシー・レコードが必要なのですか?
{:#cis-faq-proxied-record}
{: faq}

プロキシー・レコードとは、IBM CIS でトラフィックを中継するレコードのことです。 プロキシー・レコードだけが、お客様のオリジン IP アドレスを CIS の IP に置き換えて保護する IP マスクなどの CIS の利点を活用できます。

```
$ whois 104.28.22.57 | grep OrgName
OrgName:        IBM
```

(DNS は解決するものの) ドメインで CIS をバイパスしたい場合は、レコードをプロキシーしないという方法を取ることができます。

## DNS 検証エラー 1004 を受け取りましたが、どうすればよいですか?
{:#cis-faq-dns-validation-error}
{: faq}

ページ・ルールが機能するには、お客様のゾーンを DNS で解決する必要があります。 つまり、お客様のゾーンのプロキシー DNS レコードを作成する必要があります。 

## ルート・レコードに CNAME を追加できますか?
{:#cis-faq-add-cname-root-record}
{: faq}

はい。 IBM CIS は「CNAME フラット化」と呼ばれる機能をサポートしています。ユーザーはこの機能を使用して、CNAME をルート・レコードとして追加できます。権限 DNS サーバーは CNAME のターゲットのレコードを列挙し、CNAME 自体の代わりにこれらのレコードで応答するので、ユーザーがドメインのルートで CNAME を構成したことが効果的に隠されます。

## デフォルトのヘルス・チェック・タイムアウト値は何ですか?
{:#cis-faq-default-health-check-timeout}
{: faq}

無料トライアル・プランと標準プランのデフォルトのヘルス・チェック・タイムアウトは 60 秒です。

## 非 HTTP/HTTPS トラフィック用にヘルス・チェックを構成できますか?
{:#cis-faq-health-check-non-http-traffic}
{: faq}

いいえ。HTTP/HTTPS のみで構成できます。

## 非 HTTP/HTTPS トラフィック用に GLB を構成できますか?
{:#cis-faq-glb-non-http-traffic}
{: faq}

いいえ。HTTP/HTTPS のみで構成できます。

## 起点プール内の自分の起点をすべて無効にすると、起点プール自体も全体が無効になりますか?
{:#cis-faq-disabling-origins-disable-origin-pool}
{: faq}

はい。起点プールがロード・バランサー内で使用されている場合、トラフィックは次に優先度の高いプールかフォールバック・プールにルーティングされます。

## Kubernetes 入口でエラーが発生します。どうすればよいですか?
{:#cis-faq-kubernetes-ingress-error}
{: faq}

Kubernetes 入口のホスト名は、小文字の英数字、「-」、または「.」で構成されていなければならず、先頭と末尾は英数字でなければなりません。ロード・バランサー名に `_` を使用することは許可されていますが、Kubernetes クラスターで入口エラーが発生する可能性があります。Kubernetes クラスターで問題が発生しないようにするには、ロード・バランサー名に `-` を使用しないことをお勧めします。

## エッジ機能アクションを保存しようとして 502 エラーを受け取りました。どうすればよいですか?
{:#cis-faq-502-error}
{: faq}

[IBM サポート](/docs/infrastructure/cis?topic=cis-getting-help-and-support)に問い合わせて、保存しようとしたスクリプトを提供してください。