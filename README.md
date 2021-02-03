# PHP x VueJS x NginxのためのDocker Kit

## 内包されているコンテナ

| コンテナ名   | 使用用途                          | バージョン                                |
| :---------- | :------------------------------ | :-------------------------------- |
| nginx-proxy | リバースプロキシ                   | 最新版（jwilder/nginx-proxy:alpine）              |
| webapp      | VueJSを用いたフロントエンドWebサーバ | 最新版（nodejs:fpm-alpine）             |
| webapi      | WebAPI用のWebサーバ               | 最新版（php:fpm-alpine）                |
| apiapp      | WebAPI用のアプリケーションサーバ     | PHP7:7-fpm-alpine）                |
| db          | データベースサーバ                  | 最新版（mariadb:latest）                |
| phpmyadmin  | データベース管理システム             | 最新版（phpmyadmin/phpmyadmin:latest）  |

## Get Started

※事前に、DBのSQLデータをインストールしてください。
※ 4., 5.はローカル環境でhttps通信ができるようにするためのツールとして`mkcert`（URL : [https://github.com/FiloSottile/mkcert](https://github.com/FiloSottile/mkcert)）を導入する方法を記載します。

1. ソースコードのコーディング規約統一化のために下記のツールを導入してください。
    - editorconfig [https://qiita.com/naru0504/items/82f09881abaf3f4dc171](https://qiita.com/naru0504/items/82f09881abaf3f4dc171)

2. Composerをお持ちじゃない場合は、下記の手順でcomposerをインストールを行ってください。  

    - MacOS/Linuxの場合

        ```sh
        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
        php composer-setup.php
        php -r "unlink('composer-setup.php');"
        sudo mv composer.phar /usr/local/bin/composer
        ```

    - WindowsOSの場合
      [公式の「Getting Started（英語）」](https://getcomposer.org/doc/00-intro.md#installation-windows)をご参照の上、インストールしてください。

3. Dockerをお持ちじゃない場合は、下記の手順でdockerをインストールを行ってください。
    - MacOS/Linuxの場合
    [公式の「Install Docker Desktop on Mac（英語）」](https://docs.docker.com/docker-for-mac/install/)よりインストーラーをダウンロードし、インストールしてください。  
      ※インストール時に、公式ページの **System requirements** でシステム要件を満たしているか確認をしてください。

    - Windows Homeの場合
    [公式の「Install Docker Desktop on Windows Home（英語）」](https://docs.docker.com/docker-for-windows/install-windows-home/)をご参照の上、インストールしてください。  
      ※参考までにWindows Homeをご利用の方は下記のURLもご覧ください。  
        [Qiita, WSL 2 対応 Docker Desktop for Windowsを使うための手順](https://qiita.com/zembutsu/items/22a5cae1d13df0d04e7b)  

      ※インストール時に、公式ページの **System requirements** でシステム要件を満たしているか確認をしてください。

    - Windows Home以外のWindows OSの場合
    [公式の「Install Docker Desktop on Windows（英語）」](https://docs.docker.com/docker-for-windows/install/)をご参照の上、インストールしてください。  
      ※インストール時に、公式ページの **System requirements** でシステム要件を満たしているか確認をしてください。

4. （初めて`mkcert`をインストールした場合）下記の手順に準じて、mkcertをインストールをしてください。
    - MacOS/Linuxの場合

        ```sh
        brew install mkcert
        ```

    - WindowsOSの場合
        1. まずは、[installing chocolatey](https://chocolatey.org/install#installing-chocolatey)を参考に`chocolatey`をインストールしてください。

            ```sh
            Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
            ```

        2. mkcertをインストールしてください。

            ```sh
            choco install mkcert
            ```

5. （初めて`mkcert`をインストールした場合）`mkcert`でローカル環境に認証局を作ってください。

    ```sh
    mkcert -install
    ```

6. `mkcert`を利用して、SSLサーバ証明書のファイルを作成してください。

    ```sh
    mkcert ＜SSL化したいドメイン＞[ ＜SSL化したいドメイン2＞ ＜SSL化したいドメイン3＞]
    ```

    ```sh:例）localhostでSSLサーバ証明書を作成する場合
    mkcert localhost "*.localhost" #開発用ドメインを別のものに設定している場合は、この書き方に準じて生成
    ```

    ※アスタリスク「*」を使用する場合は、引用符「"」で囲ってひとまとまりにする必要があります

7. 6.で生成されたファイルを`nginx-proxy/certs`直下においてください。名前は下記で統一してください

    | ファイルの種類           | 指定のファイル名   |
    | :--------------------: | :-------------: |
    | crtファイル（.crt）      | `localhost.crt` |
    | keyファイル（.key）      | `localhost.key` |

8. このGitリポジトリの内容をローカルにCloneしてください。

    ```sh
    cd data/html
    git clone <GitHubのリポジトリURL> ./app # フロントエンドのWebサーバのアプリコード
    git clone <GitHubのリポジトリURL> ./api # WebAPI用のアプリコード
    ```

9. `docker-compose.yml`の各serviceにあるenvironment内の情報およびファイル.env内の各情報を埋めてください。

10. プロジェクトディレクトリ直下で、下記のコマンドを実施してください。

    ```sh
    docker-compose up -d --build
    ```

11. 以上で開発環境構築が完了です。
    `data/html/app`、`data/html/api`の中のコードを変えると、生成されたコンテナ内のソースコードも自動的に変更されるので、基本コンテナの中に入る必要がありません。

## 参考

- [VueJS（URL：https://vuejs.org/v2/guide/）](https://vuejs.org/v2/guide/)