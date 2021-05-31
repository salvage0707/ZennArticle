---
title: "REST通信には、げきつよRestTemplateを使うべし" 
emoji: "🔢"
type: "tech" 
topics: ["Java","api","spring","microservices","RestTemplate"]
published: true
---
# RestTemplate

## RestTemplateって？

RestTemplateは、REST API(Web API)を呼び出すためのメソッドを提供するクラス。
Spring Frameworkが提供するHTTPクライアント（HttpClientをラップしている）。

まとめると、、、REST通信が簡単にできる便利部品。

DTOからJson形式のリクエストに変換する処理や、Json形式のレスポンスをDTOにバインドする処理をしてくれる。
データ形式はJson以外にXmlやFormなど様々対応していて、カスタマイズも可能!!

### DTO => Json

```java:DTO
@Getter // (1)
@Setter // (1)
public class SomeRequestResource {
    private String message; // (2)
}
```
(1) ライブラリの`lombock`を使用してGetter, Setterを自動生成
(2) Json変換を後に反映されるフィールド

```json:Json
{
  "messsage": "test message"
}
```
DTOのフィールド名と値がJson形式に変換される

### Json => DTO

```json:Json
{
  "messsage": "test message"
}
```


```java:DTO
@Getter
@Setter
public class SomeRequestResource {
    private String message; // (1)
}
```
(1) Jsonの値がバインドされるフィールド



## `RestTemplate`の使い方


### 「きほん」のき

#### とりあえずインジェクション

```java
@Service
public class XxxxServiceImpl implements XxxxService {

    @Autowired
    RestTemplate restTemplate;

    // ...

}
```

今回のメイン`RestTemplate`をインジェクション。この`RestTemplate`の中にREST通信に使うパーツがいっぱい詰まってる。

#### GET送信してみる

```java
@Getter
@Setter
public class TestResponseResource {
    private String id;      // (1)
    private String message; // (1)
}
```
(1) レスポンスをバインドするフィールド


```java
@Service
public class XxxxServiceImpl implements XxxxService {

    @Autowired
    RestTemplate restTemplate;

    public static final String URL = "http://com.example.rest/test";

    public TestResponseResource getTestResponse() {
      // (1)
      return restTemplate.getForObject(URL, TestResponseResource.class);
    }

}
```
(1) GET送信する。
(1) `getForObject`の引数

|引数順|型|説明|
|---|---|---|
|1|`String`|送信先のURL|
|2|`Class<T>`|送信先から返却された`ResponseBody`をバインドするクラス|


#### POST送信してみる

```java
@Getter
@Setter
public class TestRequestResource {
    private String message;  // (1)
}
```
(1) 送信する値

```java
@Getter
@Setter
public class TestResponseResource {
    private String id;      // (1)
    private String message; // (1)
}
```
(1) レスポンスをバインドするフィールド

```java
@Service
public class XxxxServiceImpl implements XxxxService {

    @Autowired
    RestTemplate restTemplate;

    public static final String URL = "http://com.example.rest/test";

    public TestResponseResource getTestResponse() {
      // (1)
      TestRequestResource request = new TestRequestResource();
      request.setMessage("test message");

      // (2)
      return restTemplate.postForObject(URL,request,TestResponseResource.class);
    }

}
```
(1) 送信データを設定する
(2) POST送信
(2) `postForObject`の引数

|引数順|型|説明|
|---|---|---|
|1|`String`|送信先のURL|
|2|`Object`|送信する値|
|3|`Class<T>`|送信先から返却された`ResponseBody`をバインドするクラス|

### 「きほん」のほ

#### 送信URLを動的に変更する

REST通信の場合、URLからResourceの`id`情報を取得する場合がある。
例）　`http://com.example.rest/book/1`

`RestTemplate`の場合、複数パターンで実装が可能。

##### 順番に`PathParameter`つめつめパターン

```java
@Getter
@Setter
public class BookResponseResource {
    private String id;
    private String message;
}
```

```java
@Service
public class XxxxServiceImpl implements XxxxService {

    @Autowired
    RestTemplate restTemplate;

    // (1)
    public static final String URL = "http://com.example.rest/book/{id}";

    public TestResponseResource getTestResponse() {
      // (2)
      return restTemplate.getForObject(URL, BookResponseResource.class, "1");
    }

}
```
(1) `{id}`の箇所にパラメータがバインドされる
(2) 引数の最後にパラメータにバインドしたい文字列を渡す。可変長引数で取るため、複数指定可能。

##### 明示的につめつめパターン

```java
@Getter
@Setter
public class BookResponseResource {
    private String id;
    private String message;
}
```

```java
@Service
public class XxxxServiceImpl implements XxxxService {

    @Autowired
    RestTemplate restTemplate;

    // (1)
    public static final String URL = "http://com.example.rest/book/{id}";

    public BookResponseResource getTestResponse() {
      // (2)
      Map<String, String> params = new HashMap<String, String>();
      params.put("id", "1");
      // (3)
      return restTemplate.getForObject(URL, BookResponseResource.class, params);
    }
}
```
(1) `{id}`の箇所にパラメータがバインドされる
(2) パラメータにバインドする値を設定
(2) 引数の最後にパラメータにバインドしたい値の`Map<String, String>`を渡す。

#### 送信するHeader情報を変更する
送信するデータ形式をXMLなどの変更した場合や、文字コードを変更した場合、特殊なHeaderを送信したい場合など臨機応変に変更可能。

#### `Content-Type`を変更
##### `Content-Type`のみを変更する場合

```java
public BookResponseResource getTestResponse(BookRequestResource request) {
    RequestEntity<BookRequestResource> requestEntity = 
        RequestEntity
          .post(new URI(URL))
          .contentType(MediaType.APPLICATION_XML) // (1)
          .body(request);

    return restTemplate.getForObject(requestEntity, BookResponseResource.class);
}
```
(1) `Content-Type`を変更

##### charsetも変更する場合

```java
public BookResponseResource getTestResponse(BookRequestResource request) {
    // (1)
    Map<String, String> prop = new HashMap<String, String>();
    prop.put("charset", "shift_jis");
    // (2)
    MediaType mediaType = new MediaType(MediaType.APPLICATION_XML, param);

    RequestEntity<BookRequestResource> requestEntity = 
        RequestEntity
          .post(new URI(URL))
          .contentType(mediaType)
          .body(request);

    return restTemplate.getForObject(requestEntity, BookResponseResource.class);
}
```
(1) `MediaType`のプロパティ(`charset`)を設定
(2) プロパティを元に`MediaType`を生成

### 「きほん」のん

#### 通信先でのエラーをハンドリングする

```java
public TestResponseResource getTestResponse() {
    // (1)
    try {
        return restTemplate.getForObject(URL, TestResponseResource.class);
    }
    catch (HttpClientErrorException e) { // (1)
        logger.error("400系エラー発生");
        throw e;
    }
    catch (HttpServerErrorException e) { // (2)
        logger.error("500系エラー発生");
        throw e;
    }
}
```
(1) レスポンスのHttpStatusコードが400系の場合に発生する。
(2) レスポンスのHttpStatusコードが500系の場合に発生する。 

