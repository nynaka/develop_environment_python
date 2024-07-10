GitLab CI/CD
===

```{warning}
作成途中です。
```

## 事前準備

- Docker のインストール
- [GitLab Runner のインストール](https://gitlab-docs.creationline.com/runner/install/)

    - Debian / Ubuntu

        ```bash
        curl -L \
            "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" \
            | sudo bash
        sudo apt update
        sudo apt install -y gitlab-runner
        systemctl start gitlab-runner
        systemctl enable gitlab-runner
        ```

- サーバ証明書の期限が切れていた場合

    - 自己認証局署名鍵・証明書作成

        ```bash
        openssl req \
            -new \
            -out ca.crt \
            -keyout ca.key \
            -x509 \
            -days 3650 \
            -newkey rsa:2048 \
            -nodes \
            -subj "/CN=PrivateCA"
        ```

    - サーバ証明書作成

        - CSR

            ```bash
            openssl req \
                -new \
                -out example.jp.csr \
                -keyout example.jp.key \
                -newkey rsa:2048 \
                -nodes \
                -subj "/CN=example.jp"
            ```

        - Subject Alternative Name の準備

            ここでは `san.txt` とします。

            ```text
            subjectAltName = DNS:example.jp
            ```

        - CSR へ署名

            ```bash
            openssl x509 \
                -req \
                -days 3650 \
                -in example.jp.csr \
                -out example.jp.crt \
                -CA ca.crt \
                -CAkey ca.key \
                -CAcreateserial \
                -extfile san.txt
            ```

            Subject Alternative Name の設定方法は CSR を作る時に指定する方法もあるようですが、openssl.cnf の影響か、うまくいきませんでした。

        - 証明書の確認

            ```bash
            openssl x509 -text -in server.crt -noout
            ```
            ```text
            ・・・
                    X509v3 extensions:
                        X509v3 Subject Alternative Name:
                            DNS:example.jp
                        X509v3 Subject Key Identifier:
                            93:93:36:75:41:B1:4B:6B:B2:E7:B1:1F:B3:75:93:A4:15:1C:C3:0E
                        X509v3 Authority Key Identifier:
                            FF:DA:C7:E4:2C:8A:43:16:B9:EC:D6:22:B5:8F:42:5B:6E:F5:B8:8E
            ・・・
            ```

            `X509v3 Subject Alternative Name` が設定されているかどうかが重要です。

        - GitLab の公開鍵証明書を信頼する証明書扱いにする

            ```bash
            sudo mkdir -p /usr/local/share/ca-certificates/
            sudo cp example.jp.crt /usr/local/share/ca-certificates/
            sudo update-ca-certificates
            ```



- プロジェクトごと

    - Runner の設定 (ブラウザ)

        `プロジェクトサイドメニュー` ⇒ `Settings` ⇒ `CI/CD` ⇒ `Runners` で Runnerを設定します。

    - gitlab-runner の登録

        ```bash
        sudo gitlab-runner register \
            --url https://gitlab.local \
            --token glrt-UcLNaQy3MzSpuxcWJcG8
        ```

        - Runner の検証に失敗する場合

            1. GitLab の公開鍵証明書の取得

                ```bash
                openssl s_client \
                    -showcerts \
                    -connect gitlab.local:443 < /dev/null 2> /dev/null \
                    | openssl x509 -outform PEM > gitlab.local.crt
                ```

            2. GitLab の公開鍵証明書を gitlab-runner の証明書ディレクトリに配置する

                ```bash
                sudo mkdir -p /usr/local/share/ca-certificates/
                sudo cp gitlab.local.crt /usr/local/share/ca-certificates/
                sudo update-ca-certificates
                ```


## 最小構成

- リポジトリのルートディレクトリに `.gitlab-ci.yml` を作成して push する。

    ```
    build-job:
      # ジョブのステージ
      stage: build
      # 実行する処理をscript句に配列形式で定義
      script:
        - echo "GitLab CI/CD"
    ```

