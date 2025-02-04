---
title: Insecure Use of Cryptography
layout: col-sidebar
tags: owasp top-10 threats infrastructure infrastructure-threats security risks infrastructure-security-risks insecure use of cryptography isr05
---

# ISR05:2024 – 安全でない暗号技術の使用 (Insecure Use of Cryptography)

## 説明
暗号化はサイバー防御において重要な役割を果たします。
これは外部アプリケーションやシステムではよく知られていますが、内部ネットワークやインフラストラクチャについては見逃されがちです。
内部ネットワークで使用するシステムやプロトコルに十分な暗号化や暗号手法を実装していないと、隣接する脅威アクターが通信やシステム内のデータを読み取り、改変、注入できる可能性があることを、企業やユーザーは気に留める必要があります。このような暗号化の欠如はデータ漏洩や特権アカウントの侵害につながる可能性があります。
外部への防衛線だけでは内部システムを完全に保護できないことを留意することが重要です。攻撃者がフィッシングなどで内部ネットワークへのアクセスを獲得すると、外部の防衛線はほとんど意味をなしません。そのため、内部インフラストラクチャのセキュリティが外部システムと同等かそれ以上に安全である必要があります。


## リスク
安全でない暗号手法や暗号設定が使用されている場合、情報漏洩や (特権) アカウントの侵害のリスクが高くなります。
これにより、壊滅的なサイバー攻撃とその影響の可能性が大幅に高まります。たとえば、管理者が Remote Desktop Protocol - RDP などの一般的なリモートアクセスプロトコルで内部サーバーに接続する際、暗号化していないか、安全でないもので構成している場合、隣接するローカル脅威アクターがネットワークからこれらの認証情報を読み取り、特権アカウントを簡単に侵害できます。
同じことが内部ネットワーク上で送信される機密情報にも当てはまります。企業の社内インフラストラクチャには一般的にデータを転送したりさまざまなコンポーネントにアクセスする多くのプロトコル、ツール、システムがあります。データやアカウントを保護するために、十分かつ安全な暗号化や暗号手法を使用して、各々を構成しなければなりません。


## 対策
すべてのプロトコル、通信ツール、リモートアクセスツール、データ転送ツールなどが安全な暗号手法と構成を使用して構成されていることを確保することをお勧めします。
さらに、安全な暗号化をサポートしていないプロトコルは、TELNET -> SSH や FTP -> SFTP など、安全な代替手段に移行すべきです。
使用する暗号手法や構成が十分であることを確保し、ベストプラクティスに従って実装し、公式の推奨事項に従います。
暗号機能は自分で開発したり実装すべきではありません。代わりに、一般に公開されている既存のライブラリを使用します。「独自の暗号を作ってはいけません。」


## 攻撃シナリオの例
**シナリオ #1: 暗号化されていないリモートアクセスツール**
ある企業には従業員にアプリケーションを提供するためのさまざまな社内サーバーを備えた共通の社内 IT インフラストラクチャがあります。これらのアプリケーションの一つは企業の顧客について重要な機密データを保持する顧客関係管理 (CRM) です。
脅威アクターは、設定ミスによってインターネットに公開された未知の企業のシステムを介して社内インフラストラクチャにアクセスします。
脅威アクターは社内ネットワークを盗聴し、最初に侵入したマシンからすべての社内ネットワークトラフィックを傍受します。
管理者は社内ファイルサーバーに接続して、アップデートを実行し、telnet を使用してサーバーに接続します。
telnet はデータを暗号化しないため、管理者の認証情報は社内ネットワーク上でクリアテキストで送信されます。
脅威アクターはこのネットワークパケットをキャプチャできるため、管理者のアカウントを侵害できます。
これらの認証情報を使用して CRM サーバーにログオンし、すべての顧客データを抽出して競合他社に売却します。

**シナリオ #2: 暗号技術の不十分な使用**
ある企業にはいくつかの社内アプリケーションを備えた社内 IT インフラストラクチャがあります。
これらのアプリケーションの一つは企業顧客向けの請求書を作成するために使用されます。
脅威アクターは社内ネットワークへアクセスし、社内ネットワークトラフィックのパケットを検査して注入できます。
従業員は請求書システムに接続して、複数の請求書を作成します。
従業員のラップトップと請求書サーバーの間の通信プロトコルは暗号化されていますが、暗号化された ID や完全性チェックは行われません。
脅威アクターはこの脆弱性を使用して、Man in the Middle - MitM 攻撃を実行し、データパケットを注入および改変します。
脅威アクターは、従業員が請求書サーバーに送信するデータを変更して、お金を企業のアカウントではなく脅威アクターが管理するアカウントにリダイレクトします。
送られた請求書を顧客が支払うと、その企業ではなく攻撃者に送金されます。
