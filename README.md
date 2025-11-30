# Ingress Auth with Tilt

Tilt を利用して、フォーム認証機能を持つ Web アプリケーションとバックエンドの Nginx を Kubernetes 上にデプロイし、Ingress 経由での認証を実現するサンプルです。

## 概要

このプロジェクトは、[ts-form-auth-webapp](https://github.com/showchan33/ts-form-auth-webapp) をベースにしたフォーム認証アプリケーションを利用します。Tilt がファイルの変更を監視し、コンテナイメージのビルド、コンテナレジストリへのプッシュ、Kubernetes クラスタへのデプロイを自動的に行います。

NGINX Ingress Controller を介して、認証済みユーザーのみがバックエンドの Nginx コンテンツにアクセスできるように制御します。

## アーキテクチャ

-   **ts-form-auth-webapp**: フォーム認証を提供する Express ベースの Web アプリケーション。
-   **backend**: 認証後にアクセス可能となる Nginx コンテナ。
-   **NGINX Ingress Controller**: リクエストのルーティングと認証チェックを担います。
-   **Tilt**: 開発環境の自動化ツール。
-   **Helm**: Kubernetes マニフェストの管理ツール。

## 必要なもの

-   Tilt
-   Kubernetes クラスター
    -   NGINX Ingress Controller がインストール済みであること
-   コンテナレジストリ (例: localhost:5000)
-   Docker

## セットアップ

1.  **リポジトリのクローン**

    `--recurse-submodules` オプションを付けて、リポジトリをクローンします。

    ```bash
    git clone --recurse-submodules https://github.com/showchan33/ingress-auth-with-tilt.git
    cd ingress-auth-with-tilt
    ```

2.  **.env ファイルの作成**

    `.env.sample` をコピーして `.env` ファイルを作成し、環境に合わせて以下の変数を設定します。

    ```bash
    cp .env.sample .env
    ```

    -   `REGISTRY_HOST`: コンテナイメージをプッシュするレジストリのホスト名 (デフォルト: `localhost:5000`)
    -   `K8S_CONTEXT`: Tilt が使用する Kubernetes のコンテキスト (オプション)
    -   `K8S_NAMESPACE`: デプロイ先の Kubernetes 名前空間 (デフォルト: `default`)

3.  **values.yaml ファイルの作成**

    `helm-chart/values.yaml.sample` をコピーして `helm-chart/values.yaml` を作成し、必要に応じて値を設定します。

    ```bash
    cp helm-chart/values.yaml.sample helm-chart/values.yaml
    ```

    -   `authSignIn.hostname`: 認証画面にアクセスするためのホスト名
    -   `authSignIn.port`: 認証画面にアクセスするためのポート番号
    -   `authWebApp.sessionMaxAgeSecond`: セッションの有効期間

## 使い方

プロジェクトのルートディレクトリで以下のコマンドを実行すると、Tilt が起動し、アプリケーションが Kubernetes クラスタにデプロイされます。

```bash
tilt up
```

Tilt の UI が Web ブラウザで開き、デプロイの状況を確認できます。

デプロイ完了後、`http://[authSignIn.hostname]:[authSignIn.port]`にアクセスすると、ログイン画面が表示されます。

-   **ユーザーID**: `user1`
-   **パスワード**: `pass1`

でログインできます。

## 技術スタック

-   TypeScript
-   Node.js / Express
-   Tilt
-   Kubernetes
-   Helm
-   Docker