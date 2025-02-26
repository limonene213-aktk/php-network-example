<p align="center"><a target="_blank"><img src="https://upload.wikimedia.org/wikipedia/commons/2/27/PHP-logo.svg" width="300" alt="php Logo"></a></p>

# Webhook、Session、Cookieなどなど
このREADMEでは、WebhookやSessionなど、PHPにおける通信関係のコード解説を行います。

## このリポジトリで扱う内容
- 1.WebhookとHTTPリクエストの基本
- 2.SessionとCookieのコード例

# 1.WebhookとHTTPリクエストの基本 

このプロジェクトでは、PHPを使ってWebhookの受信と、GETリクエスト・POSTリクエストを送信する方法を学びます。以下の3つのスクリプトを使って、それぞれの仕組みを確認します。

- **webhook.php**: Webhookを受け取り、データを表示します。
- **send_get.php**: サーバーにGETリクエストを送ります。
- **send_post.php**: サーバーにPOSTリクエストを送ります。

---

## 前提条件

1. **PHPの環境**: Xamppなどで用意してください。
2. **基本的なPHPの知識**: 変数や配列、条件分岐を理解しているとスムーズです。

---

## ファイルの内容

### 1. webhook.php

このスクリプトは、サーバーに送られたリクエスト(GETまたはPOST)を受信し、その内容を表示します。

```php
<?php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $postData = file_get_contents('php://input'); // POSTデータを受け取る
    $data = json_decode($postData, true); // JSON形式をデコード
    echo "Received POST data: ";
    print_r($data); // 受け取ったデータを表示
} elseif ($_SERVER['REQUEST_METHOD'] === 'GET') {
    echo "Received GET request with query params: ";
    print_r($_GET); // クエリパラメータを表示
} else {
    echo "Unsupported request method!"; // その他のリクエストメソッドの場合
}
?>
```

#### 動作のポイント
- GETリクエストの場合はURLに付属したクエリパラメータを表示します。
- POSTリクエストの場合は、JSON形式のデータを受け取ります。

---

### 2. send_get.php

このスクリプトは、`webhook.php`にGETリクエストを送信します。

```php
<?php
$url = 'http://<UbuntuサーバーのIPアドレス>/webhook.php?param1=value1&param2=value2';

$response = file_get_contents($url); // URLにリクエストを送る
echo "GET Response: ";
echo $response; // サーバーの応答を表示
?>
```

#### 動作のポイント
- `param1=value1&param2=value2`のようなクエリパラメータをURLに付けて送信します。
- サーバーがリクエストを受信すると、対応するレスポンスが返ってきます。

---

### 3. send_post.php

このスクリプトは、`webhook.php`にPOSTリクエストを送信します。

```php
<?php
$url = 'http://<UbuntuサーバーのIPアドレス>/webhook.php';
$data = array('key1' => 'value1', 'key2' => 'value2'); // 送信するデータ

$options = array(
    'http' => array(
        'header'  => "Content-type: application/json\r\n", // JSON形式で送信
        'method'  => 'POST',
        'content' => json_encode($data), // 配列をJSON形式に変換
    ),
);
$context  = stream_context_create($options); // コンテキストを作成
$result = file_get_contents($url, false, $context); // POSTリクエストを送る

echo "POST Response: ";
echo $result; // サーバーの応答を表示
?>
```

#### 動作のポイント
- POSTリクエストを送信する際に、データはJSON形式でサーバーに送られます。
- `stream_context_create`を使ってHTTPリクエストのオプションを指定します。

---

## 手順

1. **ファイルの配置**
   - `webhook.php`をサーバーの公開ディレクトリ（例: `/var/www/html/`）に配置します。
   - `send_get.php`と`send_post.php`は任意のクライアント側で実行します。

2. **サーバーを起動**
   - UbuntuサーバーでApacheを起動し、`webhook.php`にアクセスできるようにします。
     ```bash
     sudo service apache2 start
     ```

3. **GETリクエストのテスト**
   - クライアント側で`send_get.php`を実行します。
     ```bash
     php send_get.php
     ```

4. **POSTリクエストのテスト**
   - クライアント側で`send_post.php`を実行します。
     ```bash
     php send_post.php
     ```

---

## 出力例

### GETリクエストの出力例

- **send_get.php**
  ```
  GET Response: Received GET request with query params: Array
  (
      [param1] => value1
      [param2] => value2
  )
  ```

### POSTリクエストの出力例

- **send_post.php**
  ```
  POST Response: Received POST data: Array
  (
      [key1] => value1
      [key2] => value2
  )
  ```

---

## まとめ

この学習を通して以下のことが理解できました：
- Webhookの仕組み
- GETリクエストとPOSTリクエストの違い
- PHPを使ったHTTPリクエストの送信と受信

これを応用して、簡単なWebサービスやアプリケーションの開発を始めてみましょう！

--- 



## About this repository
©伊藤あきら