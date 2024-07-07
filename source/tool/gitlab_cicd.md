GitLab CI/CD
===

```{warning}
作成途中です。
```

## 事前準備

- 全体
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

                ではなく、

                ```bash
                sudo mkdir -p /etc/gitlab-runner/certs
                sudo cp gitlab.local.crt /etc/gitlab-runner/certs/
                sudo systemctl restart gitlab-runner
                ```

                らしい

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

