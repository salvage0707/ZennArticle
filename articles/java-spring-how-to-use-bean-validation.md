---
title: "Spring のBean Validaiton 〜びーん ばりでーしょん〜" 
emoji: "🎺"
type: "tech" 
topics: ["Java","spring","Validation"]
published: true
---
# Bean Validation

Spring Frameworkを使用し、REST通信を基本としたサンプルを記載する。

## びーん ばりでーしょんって？
クライアントから送信されたRequestBodyの値をチェックする。
チェック内容は何種類かある。

- Null
- 空文字
- 文字数
- パターン(正規表現)
- 最大サイズ
- 最小サイズ

などなど...

## とりあえずサンプル

コントローラでただバリデーションを実行するだけのサンプル。

```java:SampleContorller.java
@RestController
@RequestMapping("validation")
public class SampleController {

  // バリデーションするだけ
  @PostMapping
  public SampleResource validation(
        @RequestBody @Validated SampleResource resource) {
    return resource;
  }
}
```
```
@Getter
@Setter
public class SampleResource {
  @NotNull
  @Size(min = 5, max = 30)
  @Pattern(regexp = "[a-zA-Z0-9]*")
  private String message;
}
```

|アノテーション|説明|
|---|---|
|@NotNull|Nullを禁止|
|@Size|{min}以上{max}以下であることを確認する|
|@Pattern|指定パターンであることを確認する|

## more びーん ばりでーしょん
### ネストしたクラスのバリデーション
フィールドに`@Valid`を指定する。

```java:SampleContorller.java
上のサンプルコントローラ一緒
```

```
@Getter
@Setter
public class SampleResource {
  @Valid
  @NotNull
  private SampleNestResource nestResource;
}
```

@Validでネストしたクラスのバリデーションを実行している。`nestResource`が送信されてこない場合はフィールドにNullがバインドされてしまい、`SampleNestResource`に指定したバリデーションが実行されないため、@NotNullで必須にしている。

```
@Getter
@Setter
public class SampleNestResource {
  @NotNull
  @Size(min = 5, max = 30)
  @Pattern(regexp = "[a-zA-Z0-9]*")
  private String message;
}
```

### Collection内のチェック
`List`などの`Collection`をフィールドに指定した場合、`Collection`ないのクラスのバリデーション指定とフィールドに対するバリデーションの指定が異なる。

```java:SampleContorller.java
上のサンプルコントローラ一緒
```

```
@Getter
@Setter
public class SampleResource {
  
  @Size(min = 2)
  private List<@NotNull String> strList;
}
```

@Sizeでフィールドの`List`要素が2以上かを確認
@NotNullで`List`内の`String`がNullではないことを確認

### バリデーションエラーのハンドリング

ハンドリングを行わない場合は`MethodArgumentNotValidException `が発生する。
`RestController`ではBindingResultにエラーが入らないため、Errorsでエラー内容を取得する。

```java:SampleContorller.java
@RestController
@RequestMapping("validation")
public class SampleController {

  // バリデーションするだけ
  @PostMapping
  public SampleResource validation(
        @RequestBody @Validated SampleResource resource,
        Errors errors) {
    if (errors.hasErrors()){
      throw new RuntimeException();
    }

    return resource;
  }
}
```


