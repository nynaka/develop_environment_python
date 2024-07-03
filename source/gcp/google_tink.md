[Google Tink](https://developers.google.com/tink?hl=ja)
===

## インストール

```bash
pip install tink
```

## 使い方

### 暗号鍵 (32バイト乱数) を自分で用意する場合

```python
import tink
from tink import aead
import os

# Tinkの初期化
aead.register()

# 32バイトの乱数を生成
# これを対象鍵として使います
random_key = os.urandom(32)

# KeysetHandleを手動で作成したキーで初期化
keyset_handle = tink.new_keyset_handle(aead.aead_key_templates.AES256_GCM)

# AEAD (Authenticated Encryption with Associated Data) primitiveを取得
aead_primitive = keyset_handle.primitive(aead.Aead)

# 暗号化するデータと関連データ
plaintext = b"this is some data to encrypt"
associated_data = b"some associated data"

# データを暗号化
ciphertext = aead_primitive.encrypt(plaintext, associated_data)
print(f"Ciphertext: {ciphertext}")

# データを復号
decrypted_text = aead_primitive.decrypt(ciphertext, associated_data)
print(f"Decrypted text: {decrypted_text}")
```

### AWS KMS 鍵を利用する場合

AWS KMS は本物ではなく LocalStack を使用する。

1. Localstack のエンドポイントの設定

    ```bash
    export ENDPOINT_URL=http://localhost:4566
    ```

2. テスト用アカウント (`testuser`) 作成

    ```bash
    aws iam create-user \
        --user-name testuser \
        --profile localstack \
        --endpoint-url $ENDPOINT_URL 
    ```

3. テスト用アカウントのアクセスキー作成

    ```bash
    aws iam create-access-key \
        --user-name testuser \
        --profile localstack \
        --endpoint-url $ENDPOINT_URL 
    ```

    `AccessKeyId` と `SecretAccessKey` を控えておきます。

4. テスト用の鍵作成

    ```bash
    aws kms create-key \
        --profile localstack \
        --endpoint-url $ENDPOINT_URL
    ```

    作成した鍵の ARN を控えておきます。

5. credentials.ini 作成

    #3 で作成した `AccessKeyId` と `SecretAccessKey` を使って、下記の書式で credentials.ini を作成します。

    ```ini
    [default]
    aws_access_key_id = AccessKeyId
    aws_secret_access_key = SecretAccessKey
    ```

6. サンプルコード

    https://github.com/tink-crypto/tink-py/tree/main/tink/integration/awskms のテストコードを参考にしています。

    ```python
    import boto3
    import tink
    from tink import _kms_clients
    from tink import aead
    from tink.integration import awskms
    from tink.integration.awskms import _aws_kms_client
    from tink.testing import helper

    CREDENTIAL_PATH = "./credentials.ini"
    KEY_URI = ("aws-kms://arn:aws:kms:ap-northeast-1:000000000000:key/"
            "840af982-b09f-4ee7-896b-83cb9285d01c")  ### #4で作った鍵のARN

    aws_access_key_id, aws_secret_access_key = _aws_kms_client._parse_config(
        CREDENTIAL_PATH
    )

    boto3_client = boto3.client(
        service_name="kms",
        aws_access_key_id=aws_access_key_id,
        aws_secret_access_key=aws_secret_access_key,
        region_name="ap-northeast-1",
        endpoint_url="http://localhost:4566"
    )
    aws_client = awskms.new_client(boto3_client=boto3_client, key_uri=KEY_URI)

    aws_aead = aws_client.get_aead(KEY_URI)

    plaintext = b'hello'
    associated_data = b'world'
    ciphertext = aws_aead.encrypt(plaintext, associated_data)
    print(ciphertext)
    print(aws_aead.decrypt(ciphertext, associated_data))

    plaintext = b'hello'
    ciphertext = aws_aead.encrypt(plaintext, b'')
    print(ciphertext)
    print(aws_aead.decrypt(ciphertext, b""))
    ```
