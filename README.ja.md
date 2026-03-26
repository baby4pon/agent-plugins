# Agent Plugins for AWS

[English](README.md)

> [!IMPORTANT]
> 生成 AI は誤りを犯す可能性があります。選択した AI モデルおよびエージェント型コーディングアシスタントが生成したすべての出力とコストを確認することを検討してください。[AWS の責任ある AI ポリシー](https://aws.amazon.com/ai/responsible-ai/policy/) をご覧ください。

Agent Plugins for AWS は、AI コーディングエージェントに AWS 上でのアーキテクチャ設計・デプロイ・運用を支援するスキルを提供します。Agent プラグインは現在 Claude Code および Cursor に対応しています。

AI コーディングエージェントはソフトウェア開発において広く活用されるようになり、開発者のコード作成・レビュー・デプロイをより効率的に支援しています。エージェントスキルとエージェントプラグインのパッケージングモデルは、モデルのコンテキストを肥大化させることなく、コーディングエージェントを信頼性の高い成果へと導くためのベストプラクティスとして浸透しつつあります。長い AWS ガイダンスをプロンプトに繰り返し貼り付ける代わりに、そのガイダンスを再利用可能なバージョン管理されたスキルとしてエンコードし、必要なときにエージェントが呼び出せるようにすることで、決定論性の向上・コンテキストオーバーヘッドの削減・チーム横断でのエージェント動作の標準化が実現します。エージェントプラグインは異なる種類のエキスパートアーティファクトをまとめたコンテナとして機能します。1 つのエージェントプラグインには以下を含めることができます。

- エージェントスキル – デプロイ・コードレビュー・アーキテクチャ計画などの複雑なタスクを AI がこなせるよう、構造化されたワークフローとベストプラクティスのプレイブック。ドメイン知識をステップバイステップのプロセスとしてエンコードします。
- MCP サーバー – 外部サービス・データソース・API との接続。MCP サーバーはライブドキュメント・価格データ・その他のリソースへのランタイムアクセスを提供します。詳しくは [AWS 向け MCP サーバー](https://github.com/awslabs/mcp) をご覧ください。
- フック – 開発者のアクションに応じて実行される自動化とガードレール。変更の検証・標準の適用・ワークフローのトリガーなどを自動化します。
- リファレンス – エージェントスキルが参照するドキュメント・設定デフォルト・ナレッジ。プロンプトを肥大化させることなくエージェントスキルをよりスマートにします。

この分野で新しい種類のエキスパートアーティファクトが登場した場合もエージェントプラグインとしてパッケージングでき、進化が開発者にとって透明な形で提供されます。

## ベストプラクティス

セキュリティとコード品質を維持しながらプラグイン支援開発のメリットを最大化するために、次の重要なガイドラインに従ってください。

- デプロイ前に生成されたコードを常に確認する（例: セキュリティ・コスト・耐障害性に関する制約に照らして）
- プラグインは開発者の判断や専門知識の代替ではなく、加速ツールとして活用する
- 最新の AWS ベストプラクティスを享受するためにプラグインを最新の状態に保つ
- AWS 認証情報の設定時には最小権限の原則に従う
- 生成されたインフラコードにセキュリティスキャンツールを実行する

## プラグイン一覧

| プラグイン | 説明 | ステータス |
| --- | --- | --- |
| **amazon-location-service** | Amazon Location Service を使ってアプリにマップ・ジオコーディング・ルーティング・場所検索・地理空間機能を追加する | 利用可能 |
| **aws-amplify** | AWS Amplify Gen 2 を使い、認証・データ・ストレージ・関数のガイドワークフローでフルスタックアプリを構築する | 利用可能 |
| **aws-serverless** | Lambda・API Gateway・EventBridge・Step Functions・永続関数でサーバーレスアプリを構築する | 利用可能 |
| **databases-on-aws** | AWS データベースポートフォリオのガイダンス — スキーマ設計・クエリ・マイグレーション・マルチテナントパターン | 一部利用可能（Aurora DSQL） |
| **deploy-on-aws** | アーキテクチャ推奨・コスト見積もり・IaC デプロイで AWS へのアプリデプロイを支援する | 利用可能 |
| **migration-to-aws** | リソース探索・アーキテクチャマッピング・コスト分析・実行計画を通じて GCP インフラを AWS に移行する | 利用可能 |

## インストール

### Claude Code

#### マーケットプレイスを追加する

```bash
/plugin marketplace add awslabs/agent-plugins
```

#### プラグインをインストールする

```bash
/plugin install amazon-location-service@agent-plugins-for-aws
```

または

```bash
/plugin install aws-amplify@agent-plugins-for-aws
```

または

```bash
/plugin install aws-serverless@agent-plugins-for-aws
```

または

```bash
/plugin install databases-on-aws@agent-plugins-for-aws
```

または

```bash
/plugin install deploy-on-aws@agent-plugins-for-aws
```

または

```bash
/plugin install migration-to-aws@agent-plugins-for-aws
```

### Cursor

**deploy-on-aws** プラグインは [Cursor マーケットプレイス](https://cursor.com/marketplace/aws) からインストールできます。詳細については [Cursor プラグインドキュメント](https://cursor.com/docs/plugins) を参照してください。Cursor アプリケーション内からもインストールできます。

- Cursor の設定を開く
- `Plugins` に移動する
- `AWS` を検索する
- インストールしたいプラグインを選択して `Add to Cursor` をクリックする
- インストールするプラグインのスコープを選択する
- プラグインが `Plugins -> Installed` に表示されます

## amazon-location-service

Amazon Location Service を使ったマップ・場所検索・ジオコーディング・ルーティング・その他の地理空間機能の追加を、認証設定・SDK 統合・ベストプラクティスを含めてガイドします。

### スキルのトリガー

| エージェントスキル | トリガーフレーズ |
| --- | --- |
| **amazon-location-service** | "add a map"、"geocode an address"、"calculate a route"、"location-aware app"、"Amazon Location Service"、"geospatial"、"places search"、"地図を追加"、"ジオコーディング"、"ルート計算"、"場所検索" |

### MCP サーバー

| サーバー | 目的 |
| --- | --- |
| **aws-mcp** | AWS ドキュメントおよびサービスガイダンス |

## aws-amplify

TypeScript コードファースト開発と公式 AWS エージェント標準オペレーション手順（SOP）のガイダンスにより、AWS Amplify Gen 2 でフルスタックアプリを構築します。

### ワークフロー

1. **バックエンド** – Amplify Gen 2 リソースを作成（認証・データ・ストレージ・関数）
2. **サンドボックス** – テスト用サンドボックスにデプロイ
3. **フロントエンド & テスト** – フロントエンドをバックエンドに接続してローカルで確認
4. **本番環境** – 本番環境にデプロイ

### スキルのトリガー

| エージェントスキル | トリガーフレーズ |
| --- | --- |
| **amplify-workflow** | "build Amplify app"、"create Amplify project"、"add auth to Amplify"、"deploy Amplify"、"full-stack Amplify"、"Amplifyアプリを作成"、"認証を追加"、"Amplifyデプロイ" |

### MCP サーバー

| サーバー | 目的 |
| --- | --- |
| **aws-mcp** | AWS ドキュメントおよびサービスガイダンス |

## aws-serverless

AWS Lambda・API Gateway・EventBridge・Step Functions・永続関数を使ったサーバーレスアプリの設計・構築・デプロイ・テスト・デバッグを支援します。SAM および CDK デプロイワークフロー・SAM テンプレート検証フック・AWS Lambda 永続関数スキルを含みます。

### スキルのトリガー

| エージェントスキル | トリガーフレーズ |
| --- | --- |
| **aws-lambda** | "Lambda function"、"event source"、"serverless application"、"API Gateway"、"EventBridge"、"Step Functions"、"Lambda関数"、"イベントソース"、"サーバーレスアプリ"、"イベント駆動アーキテクチャ" |
| **aws-serverless-deployment** | "use SAM"、"SAM template"、"SAM deploy"、"CDK serverless"、"CDK Lambda construct"、"serverless CI/CD pipeline"、"SAMテンプレート"、"SAMデプロイ"、"CDKサーバーレス"、"サーバーレスCI/CDパイプライン" |
| **aws-lambda-durable-functions** | "lambda durable functions"、"workflow orchestration"、"state machines"、"saga pattern"、"Lambda永続関数"、"ワークフローオーケストレーション"、"ステートマシン"、"サガパターン" |

### MCP サーバー

| サーバー | 目的 |
| --- | --- |
| **aws-serverless-mcp** | サーバーレス開発ガイダンス・プロジェクトスキャフォールド・IaC 生成・デプロイ |

### フック

| フック | トリガー | アクション |
| --- | --- | --- |
| **SAM テンプレート検証** | `template.yaml`/`template.yml` 編集後 | `sam validate` を実行してエラーをインラインで報告する |

## databases-on-aws

AWS データベースポートフォリオのガイダンス。スキーマ設計・クエリの実行・マイグレーション・アプリ構築・ワークロードに最適なデータベース選択を支援します。現在は Aurora DSQL（サーバーレスの PostgreSQL 互換分散 SQL データベース）を含みます。

### スキルのトリガー

| エージェントスキル | トリガーフレーズ |
| --- | --- |
| **dsql** | "Aurora DSQL"、"DSQL schema"、"distributed SQL database"、"serverless PostgreSQL-compatible database"、"create DSQL table"、"DSQLテーブルを作成"、"DSQLスキーマ"、"DSQLに移行"、"分散SQLデータベース" |

### MCP サーバー

| サーバー | 目的 |
| --- | --- |
| **awsknowledge** | AWS ドキュメント・アーキテクチャガイダンス・ベストプラクティス |
| **aurora-dsql** | 直接データベース操作 — クエリ・スキーマ・トランザクション（デフォルトで無効） |

### フック

| フック | トリガー | アクション |
| --- | --- | --- |
| **スキーマ検証** | `transact` 操作後 | スキーマ変更と影響を受けた行の検証を促す |

## deploy-on-aws

AWS アーキテクチャとサービスの推奨・コスト見積もり・Infrastructure as Code（CDK または CloudFormation）の生成・デプロイのガイドによって、AWS デプロイを加速するスキルをエージェントに提供します。

### ワークフロー

1. **分析** – コードベースのフレームワーク・データベース・依存関係をスキャン
2. **推奨** – AWS サービスを選択して簡潔に理由を説明
3. **見積もり** – 進める前にコスト見積もりを表示
4. **生成** – IaC コード（CDK/CloudFormation）を記述
5. **デプロイ** – ユーザーの確認を得て実行

### スキルのトリガー

| エージェントスキル | トリガーフレーズ |
| --- | --- |
| **deploy** | "deploy to AWS"、"host on AWS"、"run this on AWS"、"AWS architecture"、"estimate AWS cost"、"AWSにデプロイ"、"AWSにホスト"、"AWSで動かす"、"AWSコスト見積もり"、"インフラ生成" |

### MCP サーバー

| サーバー | 目的 |
| --- | --- |
| **awsknowledge** | AWS ドキュメント・アーキテクチャガイダンス・ベストプラクティス |
| **awspricing** | コスト見積もり向けリアルタイム AWS サービス料金 |
| **aws-iac-mcp** | CDK/CloudFormation 向け IaC ベストプラクティス |

## migration-to-aws

Terraform リソース探索・アーキテクチャマッピング・コスト見積もり・実行計画を通じて、GCP インフラを AWS へ体系的に移行することを支援します。

### ワークフロー

1. **探索** – Terraform ファイルから GCP リソースをスキャンしてインフラを抽出
2. **明確化** – コンピュートワークロードとアーキテクチャパターンを把握
3. **設計** – GCP サービスを AWS の対応サービスにマッピングして理由を説明
4. **見積もり** – 月次 AWS コストを計算して GCP と比較
5. **実行** – 移行タイムラインを計画してデプロイリスクを特定

### スキルのトリガー

| エージェントスキル | トリガーフレーズ |
| --- | --- |
| **gcp-to-aws** | "migrate GCP to AWS"、"move from GCP"、"GCP migration plan"、"GCPからAWSに移行"、"Google CloudからAWSへ"、"GCP移行計画"、"GCPインフラ評価"、"GKEからEKSに移行"、"Cloud RunからFargateに移行" |

### MCP サーバー

| サーバー | 目的 |
| --- | --- |
| **awsknowledge** | AWS ドキュメントおよびアーキテクチャガイダンス |
| **awspricing** | コスト分析向けリアルタイム AWS サービス料金 |

## 要件

- Claude Code >=2.1.29 または [Cursor >= 2.5](https://cursor.com/changelog/2-5)
- 適切な認証情報で設定された AWS CLI

## トラブルシューティング

プラグインのインストールや使用に問題がありますか？よくある解決策は[トラブルシューティングガイド](./docs/TROUBLESHOOTING.md)をご覧ください。

## コントリビューション

素晴らしいコントリビューターの皆さんに感謝します！このプロジェクトをより良くしていただきありがとうございます！

あらゆる種類のコントリビューションを歓迎します！詳細は[コントリビューターガイド](./CONTRIBUTING.md)をご確認ください。

## 開発者ガイド

ライブラリに新しいプラグインを追加したい場合は、[デザインガイドライン](./docs/DESIGN_GUIDELINES.md)と[開発ガイド](./docs/DEVELOPMENT_GUIDE.md)をご覧ください。

## メンテナー

リポジトリのレビュアー・メンテナー・管理者は、PR レビューワークフロー・マージルール・CI/CD ドキュメントを[メンテナーガイド](./docs/MAINTAINERS_GUIDE.md)で確認できます。

## 追加リソース

- [Agent Plugins for AWS 紹介ブログ](https://aws.amazon.com/blogs/developer/introducing-agent-plugins-for-aws/)
- [Cursor をプラグインで拡張する ft. AWS](https://cursor.com/blog/marketplace)
- [AWS 向け MCP サーバー](https://github.com/awslabs/mcp)
- [Claude Code プラグインドキュメント](https://code.claude.com/docs/en/plugins)
- [Cursor プラグインドキュメント](https://cursor.com/docs/plugins)

## ライセンス

このプロジェクトは Apache-2.0 ライセンスのもとで提供されています。
