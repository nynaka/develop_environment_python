GitLab Runner
===

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
        sudo systemctl start gitlab-runner
        sudo systemctl enable gitlab-runner
        ```

- サーバ証明書の期限が切れていた場合

    - 自己認証局署名鍵・証明書作成

        ```bash
        openssl req \
            -new \
            -out privateca.crt \
            -keyout privateca.key \
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
                -out dev.gitlab.local.csr \
                -keyout dev.gitlab.local.key \
                -newkey rsa:2048 \
                -nodes \
                -subj "/CN=dev.gitlab.local"
            ```

            CN で設定している `dev.gitlab.local` は、GitLab で設定している `external_url` に設定した FQDN を設定します。

        - Subject Alternative Name の準備

            ここでは `san.txt` とします。

            ```text
            subjectAltName = DNS:dev.gitlab.local, IP.1:127.0.0.1, IP.2:192.168.255.1
            ```

        - CSR へ署名

            ```bash
            openssl x509 \
                -req \
                -days 3650 \
                -in dev.gitlab.local.csr \
                -out dev.gitlab.local.crt \
                -CA privateca.crt \
                -CAkey privateca.key \
                -CAcreateserial \
                -extfile san.txt
            ```

            Subject Alternative Name の設定方法は CSR を作る時に指定する方法もあるようですが、openssl.cnf の影響か、うまくいきませんでした。

        - 証明書の確認

            ```bash
            openssl x509 -text -in dev.gitlab.local.crt -noout
            ```
            ```text
            ・・・
            X509v3 extensions:
                X509v3 Subject Alternative Name:
                    DNS:dev.gitlab.local, IP Address:127.0.0.1, IP Address:192.168.255.1
                X509v3 Subject Key Identifier:
                    7F:39:4F:39:D3:BA:16:AB:5E:B0:A4:1B:E4:FD:30:AF:F3:9B:7B:95
                X509v3 Authority Key Identifier:
                    86:7D:5E:08:A4:00:A7:CA:3D:32:5F:7D:2F:9D:39:37:F0:52:CE:27
            ・・・
            ```

            `X509v3 Subject Alternative Name` が設定されているかどうかが重要です。

        - GitLab の公開鍵証明書を信頼する証明書扱いにする

            ```bash
            sudo mkdir -p /usr/local/share/ca-certificates/
            sudo cp privateca.crt /usr/local/share/ca-certificates/
            sudo update-ca-certificates
            ```



- プロジェクトごと

    - Runner の設定 (ブラウザ)

        `プロジェクトサイドメニュー` ⇒ `Settings` ⇒ `CI/CD` ⇒ `Runners` の `New project runner` ボタンで Runner を設定します。  
        ボタン右の 3 点メニューの `Show runner installation and registration instructions` を選択すると `Command to register runner` にインストール方法が提示されます。

    - gitlab-runner の登録

        ```bash
        sudo gitlab-runner register \
            --url https://dev.gitlab.local/ \
            --registration-token GR1348941uodZbxSxQJDcyDej1ftU
        ```

        - Runner に Docker を設定したら dev.gitlab.local に接続できない

            /etc/gitlab-runner/config.toml で `[runners.docker]` に FQDN と IP アドレスの対応を追記します。

            **設定例**

            ```diff
            --- config.toml.origin  2025-02-22 07:31:07.050702268 +0900
            +++ config.toml 2025-02-22 07:31:33.490694958 +0900
            @@ -27,5 +27,6 @@
                 oom_kill_disable = false
                 disable_cache = false
                 volumes = ["/cache"]
            +    extra_hosts = ["dev.gitlab.local:192.168.255.1"]
                 shm_size = 0
                 network_mtu = 0
            ```

            GitLab と Runner を同じホストで実行すると `127.0.0.1` を使いたくなりますが、`127.0.0.1` を設定した場合、Runner で用意したコンテナの 127.0.0.1 に接続しようとするため GitLab には接続できません。

        - Runner の検証が証明書理由で失敗する場合

            1. GitLab の公開鍵証明書の取得

                ```bash
                openssl s_client \
                    -showcerts \
                    -connect dev.gitlab.local:443 < /dev/null 2> /dev/null \
                    | openssl x509 -outform PEM > gitlab.local.crt
                ```

            2. GitLab の公開鍵証明書を gitlab-runner の証明書ディレクトリに配置する

                ```bash
                sudo mkdir -p /usr/local/share/ca-certificates/
                sudo cp dev.gitlab.local.crt /usr/local/share/ca-certificates/
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

