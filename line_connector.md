# LINE広告コネクタ for カスタムオーディエンス(LINE Ads Connector for Custom Audience)

## 概要

LINE広告コネクタ for カスタムオーディエンスは、Snowflakeのデータを利用してLINE広告のカスタムオーディエンスを効率的に管理するツールです。このコネクタを使用することで、Snowflake上のデータを直接LINE広告プラットフォームにアップロードし、ターゲティング広告のためのカスタムオーディエンスを作成・更新することができます。

## 事前準備

コネクタを使用するには、以下の情報が必要です：

1. **Ad Account ID**: LINE広告アカウントのID
2. **API access key**: 有効なAPIアクセスキー
3. **API secret key**: 有効なAPIシークレットキー
4. **SlackのWebhook URL**: 通知用
5. **Snowflakeのウェアハウス**: データソース

LINE広告のAPI認証については、[LINE Ads API Documentation](https://ads.line.me/public-docs/v3/3.10.4/data-general-partner#_authentication)を参照してください。

## データ形式

Snowflake上に以下の形式のViewを作成する必要があります：

```
id,audience_name,action,id_type,hashed_email
111111111,サンプルセグメント1,REPLACE,HASHED_EMAIL,4d9a8c3198bd10bf13695c08c96a5ada090e74bb4e6e59bcb30e808b54799102
222222222,サンプルセグメント2,REPLACE,HASHED_EMAIL,d2456f5c82a856828a72cc526dadb53f7b07e9e52fb616e1e267b59bfc17da5a
333333333,サンプルセグメント3,REPLACE,HASHED_EMAIL,2d9ecb06a0a26dca7d0c2492fa21957f91887567df7469accba4b2186a3d7fa2
444444444,サンプルセグメント4,REPLACE,HASHED_EMAIL,c78440a573b10a67d1c22c4beee9210c54ec122cfd6cc24d7eff2c34f5d2263e
555555555,サンプルセグメント5,REPLACE,HASHED_EMAIL,f7f113289db0bf7623fa9f78fcda32d28c20e00f040c6d20c142875c9dd36aba
```

各列の説明：
- `id`: カスタムオーディエンスのID（オプション、REPLACEアクションの場合は必須）
- `audience_name`: カスタムオーディエンスの名前
- `action`: 実行するアクション
- `id_type`: IDのタイプ（現在はHASHED_EMAILのみサポート）
- `hashed_email`: ハッシュ化されたメールアドレス

## メールアドレスのハッシュ化

メールアドレスは、Snowflakeの`SHA2`関数を使用してハッシュ化する必要があります：

```sql
SELECT SHA2(raw_email, 256) AS hashed_email;
```

## 使用方法

1. Snowflake上で必要なViewを作成します。
2. コネクタを実行し、必要な認証情報を入力します。
3. SnowflakeのデータをLINE広告プラットフォームにアップロードします。
4. アップロード結果はSlackを通じて通知されます。

## 注意事項

- 現在、IDタイプはメールアドレス（HASHED_EMAIL）のみサポートしています。
- メールアドレスは必ずハッシュ化してください。平文のメールアドレスは使用できません。
- 大量のデータを扱う場合は、SnowflakeのパフォーマンスとLINE広告APIの制限に注意してください。

## トラブルシューティング

- API認証エラーが発生した場合は、Ad Account ID、API access key、API secret keyが正しいことを確認してください。
- データアップロードに失敗した場合は、Viewの形式が正しいこと、特にメールアドレスが適切にハッシュ化されていることを確認してください。
- Slackの通知が届かない場合は、Webhook URLが正しく設定されているか確認してください。



---

# LINE Ads Connector for Custom Audience

## Overview

The LINE Ads Connector for Custom Audience is a tool designed to efficiently manage custom audiences for LINE Ads using Snowflake data. This connector allows you to directly upload data from Snowflake to the LINE Ads platform, enabling the creation and updating of custom audiences for targeted advertising.

## Prerequisites

To use this connector, you will need the following:

1. **Ad Account ID**: Your LINE Ads account ID
2. **API access key**: A valid API access key
3. **API secret key**: A valid API secret key
4. **Slack Webhook URL**: For notifications
5. **Snowflake warehouse**: As the data source

For LINE Ads API authentication, please refer to the [LINE Ads API Documentation](https://ads.line.me/public-docs/v3/3.10.4/data-general-partner#_authentication).

## Data Format

You need to create a view in Snowflake with the following format:

```
id,audience_name,action,id_type,hashed_email
111111111,Sample Segment 1,REPLACE,HASHED_EMAIL,4d9a8c3198bd10bf13695c08c96a5ada090e74bb4e6e59bcb30e808b54799102
222222222,Sample Segment 2,REPLACE,HASHED_EMAIL,d2456f5c82a856828a72cc526dadb53f7b07e9e52fb616e1e267b59bfc17da5a
333333333,Sample Segment 3,REPLACE,HASHED_EMAIL,2d9ecb06a0a26dca7d0c2492fa21957f91887567df7469accba4b2186a3d7fa2
444444444,Sample Segment 4,REPLACE,HASHED_EMAIL,c78440a573b10a67d1c22c4beee9210c54ec122cfd6cc24d7eff2c34f5d2263e
555555555,Sample Segment 5,REPLACE,HASHED_EMAIL,f7f113289db0bf7623fa9f78fcda32d28c20e00f040c6d20c142875c9dd36aba
```

Column descriptions:
- `id`: Custom audience ID (optional, required for REPLACE action)
- `audience_name`: Name of the custom audience
- `action`: Action to perform (currently only REPLACE is supported)
- `id_type`: Type of ID (currently only HASHED_EMAIL is supported)
- `hashed_email`: Hashed email address

## Email Hashing

Email addresses must be hashed using Snowflake's `SHA2` function:

```sql
SELECT SHA2(raw_email, 256) AS hashed_email;
```

## Usage

1. Create the necessary view in Snowflake.
2. Enter the required authentication information
3. Run the connector to upload data from Snowflake to the LINE Ads platform.
4. Upload results will be notified via Slack.

## Notes

- Currently, only email addresses (HASHED_EMAIL) are supported as the ID type.
- Always hash email addresses. Plain text email addresses are not allowed.
- When dealing with large amounts of data, be mindful of Snowflake's performance and LINE Ads API limitations.

## Troubleshooting

- If you encounter API authentication errors, verify that your Ad Account ID, API access key, and API secret key are correct.
- If data upload fails, check that the view format is correct, especially ensuring that email addresses are properly hashed.
- If you're not receiving Slack notifications, verify that the Webhook URL is correctly configured.

