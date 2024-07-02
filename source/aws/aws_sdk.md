AWS SDK
===

## [LocalStack](https://github.com/localstack/localstack)

本物の AWS 環境は用意するのが面倒なので LocalStack で AWS エミュレータ環境を用意します。

- docker-compose.yml

    ```yaml
    version: "3.8"
    
    services:
      localstack:
        container_name: "${LOCALSTACK_DOCKER_NAME-localstack_main}"
        image: localstack/localstack
        ports:
          - "127.0.0.1:4566:4566"            # LocalStack Gateway
          - "127.0.0.1:4510-4559:4510-4559"  # external services port range
          - "127.0.0.1:53:53"                # DNS config (only required for Pro)
          - "127.0.0.1:53:53/udp"            # DNS config (only required for Pro)
          - "127.0.0.1:443:443"              # LocalStack HTTPS Gateway (only required for Pro)
        environment:
          - DEBUG=${DEBUG-}
          - PERSISTENCE=${PERSISTENCE-}
          - LAMBDA_EXECUTOR=${LAMBDA_EXECUTOR-}
          #- LOCALSTACK_API_KEY=${LOCALSTACK_API_KEY-}  # only required for Pro
          - DOCKER_HOST=unix:///var/run/docker.sock
        volumes:
          - "${LOCALSTACK_VOLUME_DIR:-./volume}:/var/lib/localstack"
          - "/var/run/docker.sock:/var/run/docker.sock"
    ```

- localstack コンテナ起動

    ```bash
    docker compose up -d --build
    ```

## AWS SDK for Python のインストール

```bash
pip install boto3
```

## 使用例

- S3

    ```python
    import boto3

    access_key = "dummy"
    secret_access_key = "dummy"

    s3_client = boto3.client(
        "s3",
        aws_access_key_id=access_key,
        aws_secret_access_key=secret_access_key,
        endpoint_url="http://localhost:4566",
    )

    response = s3_client.list_buckets()
    print(response)
    ```
