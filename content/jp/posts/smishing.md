---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "SMSで送信元を偽装したメッセージを送る"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2019-09-05T10:33:10+09:00
lastmod: 2019-09-05T10:33:10+09:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

「スミッシング（Smishing）」とは、SMSを悪用したフィッシング詐欺の呼称で、「SMS」と「Phishing」を組み合わせた造語である。日本国内では通信事業者や宅配業者を装ったスミッシングが多発しており、2019年6月にはJC3（日本サイバー犯罪対策センター）が[「通信事業者を騙るスミッシング詐欺の手法に係る注意喚起」](https://www.jc3.or.jp/topics/smscert.html)を行なっている。

**通信事業者が送信するメッセージが表示されるスレッド内に、他の者がメッセージを挿入することを可能。**

![図](./smscert.png)

Akaki氏の記事：
- [SMSで送信元を偽装したメッセージを送る](https://akaki.io/2019/sms_spoofing.html)でクラウド電話サービス「Twilio」を利用して実際に試してみました。

以下、その内容の転記となる。

##### 送信元表記が送信者IDのケース

SMSのメッセージを受信した際に表示される送信元には、電話番号の代わりに任意の英数字も表記できる。この英数字の送信元表記を「送信者ID（Sender ID）」という。JC3の図では 通信事業者Ａ が送信者IDに当たる。

なお送信者IDの利用可否は受信側の通信事業者の対応状況によって異なる。Twilioの販売パートナーであるKWCの説明によると、日本国内ではNTT DOCOMOとSoftBankが送信者IDに対応し、KDDIは対応していないとのこと。

まずはiOSの公式メッセージアプリに届いていたAmazonからのメッセージのスレッドで偽装を試みる。送信者IDは Amazon となっているため、TwilioでSMSを送信する際のFromの値に Amazon を指定する。

```
curl -X POST https://api.twilio.com/2010-04-01/Accounts/{$ACCOUNT_SID}/Messages.json \
--data-urlencode "From=Amazon" \
--data-urlencode "Body=hello" \
--data-urlencode "To=$IPHONE_NUMBER" \
-u $ACCOUNT_SID:$AUTH_TOKEN
```

<img src="./amazon_hello.png" width="400"/>


Twilioから送信したメッセージ「hello」が正規のスレッドに含まれた。JC3の説明は本当だった。日本語やURLリンクを含めた現実的なフィッシングメッセージの送信も試みる。Curlでは長文のメッセージが送りづらかったので、TwilioのPythonモジュールを使用してメッセージを送る。

```
import os
from twilio.rest import Client

account_sid = os.environ.get('ACCOUNT_SID')
auth_token = os.environ.get('AUTH_TOKEN')
client = Client(account_sid, auth_token)

message = client.messages.create(
    body=("Amazon Pay キャッシュバックキャンペーンに当選しました。\n\n"
          "下記のリンクからログインして1万円分のポイントをご利用ください。\n"
          "https://amazon.akaki.io/"),
    from_='Amazon',
    to=os.environ.get('IPHONE_NUMBER')
)
```

<img src="./amazon_fake.png" width="400"/>

URLリンクを含んだ日本語のメッセージも送信できた。Androidの公式メッセージアプリでも同様に送信元の偽装を試みる。Googleからのメッセージの送信者IDである Google をFromの値に指定すると、送信したメッセージが正規のスレッドに含まれた。

<img src="./google_fake.png" width="400"/>

なお当初は Amazon ではなく Apple のスレッドでの実証を予定していたが、Fromの値に Apple を指定するとError 21212が発生した。Fromの値を Applo に変更するとメッセージを送信できるため、Twilio側で特定の送信者IDの指定を禁止していると推測する。

```
$ curl -X POST https://api.twilio.com/2010-04-01/Accounts/{$ACCOUNT_SID}/Messages.json \
> --data-urlencode "From=Apple" \
> --data-urlencode "Body=hello" \
> --data-urlencode "To=$IPHONE_NUMBER" \
> -u $ACCOUNT_SID:$AUTH_TOKEN
{"code": 21212, "message": "The 'From' number Apple is not a valid phone number, shortcode, or alphanumeric sender ID.", "more_info": "https://www.twilio.com/docs/errors/21212", "status": 400}
```

##### 送信元表記が電話番号のケース

送信元が電話番号で表記されているスレッドにも偽装したメッセージを含められるのか。スーパーコールフリー番号が送信元になっているPayPayからのメッセージのスレッドで偽装を試みる。

<img src="./paypay_auth.png" width="400"/>

送信元の電話番号である 0120 990 637 をFromの値に指定するとError 21212が発生する。Fromの値にスペースを含めたのが原因と考え、国番号を追加した +81120990637 に変更するとエラーコードが変化する。新たに発生したError 21606の解説によると、Twilioはスパム対策のためアカウントに紐付いていない電話番号をFromの値に指定できない仕様であった。

```
$ curl -X POST https://api.twilio.com/2010-04-01/Accounts/{$ACCOUNT_SID}/Messages.json \
> --data-urlencode "From=+81120990637" \
> --data-urlencode "Body=hello" \
> --data-urlencode "To=$IPHONE_NUMBER" \
> -u $ACCOUNT_SID:$AUTH_TOKEN
{"code": 21606, "message": "The From phone number +81120990637 is not a valid, SMS-capable inbound phone number or short code for your account.", "more_info": "https://www.twilio.com/docs/errors/21606", "status": 400}
```

Twilioを利用した検証では、送信元の電話番号を偽装して正規のスレッドにメッセージを含めることはできなかった。しかし正規のスレッドに含めることを目的としなければ、独自に送信者IDを指定するだけで偽装したメッセージを送れる。送信者IDを PayPay と指定すればPayPayからの公式メッセージを装える。

<img src="./paypay_list.png" width="400"/>
<img src ="./paypay_fake.png" width ="400" />

iOSとAndroidの公式メッセージアプリには、メッセージ内のURLリンクを画像やタイトルを含んだリンクに置き換えるリンクプレビュー機能が搭載されている。iOSでは連絡先に登録していない送信元からのメッセージだとプレビューが機能しなかった。しかしAndroidではアプリの設定で自動プレビューが有効になっていると、連絡先に登録していない送信元であってもプレビューが機能した。プレビューを読み込むと正規サイトとの共通点が増えるため、異変に気づきにくくなる。


