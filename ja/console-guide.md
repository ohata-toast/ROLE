## Application Service > ROLE > コンソール利用ガイド

## 掲示板の例

小さな掲示板を作る状況でロールベースのリソースアクセスコントロールを構成する例でコンソールの使い方を説明します。
`/board/v1.0/{boardId}` APIを呼び出すと投稿を返すAPIがあり、このAPIは認証された会員だけが呼び出すことができるとします。
まず、認証された会員というロールを作る必要があります。

> curlを使用した例で"\{Appkey}"と"\{SecretKey}"値は実際のプロジェクト内で有効にしたROLEサービスのアプリケーションキーと秘密鍵に置き換える必要があります。

### 1) ロールの作成

![role_1.1.png](http://static.toastoven.net/prod_role/role_1.1.png)
<center>[図1.1]ロールタブに移動します。</center>

![role_1.2.png](http://static.toastoven.net/prod_role/role_1.2.png)
<center>[図1.2]ロールを追加します。</center>

### 2)オペレーションの作成

ロールを作成したら、オペレーションを作成する必要があります。

![role_2.1.png](http://static.toastoven.net/prod_role/role_2.1.png)
<center>[図2.1]オペレーションタブに移動します。</center>

![role_2.2.png](http://static.toastoven.net/prod_role/role_2.2.png)
<center>[図2.2]オペレーションを追加します。</center>

### 3)リソースの作成

次は`/board/v1.0/{boardId}`をリソースとして登録してみます。
`board`, `v1.0`, `{boardId}`に分けて順番に登録する必要があります。

![role_3.1.png](http://static.toastoven.net/prod_role/role_3.1.png)
<center>[図3.1]リソースタブに移動します。</center>

![role_3.2.png](http://static.toastoven.net/prod_role/role_3.2.png)
<center>[図3.2]リソースを追加したい親ノードをクリックし、上部の<strong>追加</strong>ボタンをクリックします。</center>

![role_3.3.png](http://static.toastoven.net/prod_role/role_3.3.png)
<center>[図3.3]リソース #1 `board`を追加します。</center>

![role_3.4.png](http://static.toastoven.net/prod_role/role_3.4.png)
<center>[図3.4]リソース #2 `v1.0`を追加します。</center>

![role_3.5.png](http://static.toastoven.net/prod_role/role_3.5.png)
<center>[図3.5]リソース #3 `{boardId}`を追加します。</center>

### 4)ロール-リソース関係の作成

リソースまで登録したら、ロールがオペレーションを実行できるリソースを指定するため、`ロール-リソース`関係を設定する必要があります。

![role_4.1.png](http://static.toastoven.net/prod_role/role_4.1.png)
<center>[図4.1]ユーザー-リソース関係を追加します。</center>

![role_4.2.png](http://static.toastoven.net/prod_role/role_4.2.png)
<center>[図4.2]ユーザー-リソース関係を追加した後の様子です。</center>

### 5)条件属性の作成

作成したロールに特定の条件のみオペレーション実行権限を付与するため、 `ロール-条件属性`関係を設定する必要があります。
条件属性は、条件属性にあらかじめ追加したロールのみ使用できます。条件属性を作成/修正する時、先に作成しておいたロールを条件属性に追加します。

![role_5.1.png](http://static.toastoven.net/prod_role/role_5.1.png)
<center>[図5.1]条件属性タブに移動します。</center>

![role_5.2.png](http://static.toastoven.net/prod_role/role_5.2.png)
<center>[図5.2]条件属性追加画面です。</center>

![role_5.3.png](http://static.toastoven.net/prod_role/role_5.3.png)
<center>[図5.3]条件属性にロールを追加します。</center>

### 6)ユーザーの作成

最後に掲示板APIを使うユーザーを追加して、アクセス制御を設定するために`MEMBER`ロールと`instance.name`条件属性を設定します。

![role_6.1.png](http://static.toastoven.net/prod_role/role_6.1.png)
<center>[図6.1]ユーザータブに移動します。</center>

![role_6.2.png](http://static.toastoven.net/prod_role/role_6.2.png)
<center>[図6.2]ユーザー追加画面です。</center>

![role_6.3.png](http://static.toastoven.net/prod_role/role_6.3.png)
<center>[図6.3]ロールを追加した様子です。</center>

![role_6.4.png](http://static.toastoven.net/prod_role/role_6.4.png)
<center>[図6.4]追加したロール修正モーダルです。</center>

![role_6.5.png](http://static.toastoven.net/prod_role/role_6.5.png)
<center>[図6.5]ロール修正モーダルで条件属性を追加します。</center>

![role_6.6.png](http://static.toastoven.net/prod_role/role_6.6.png)
<center>[図6.6]条件属性を追加した様子です。</center>

![role_6.7.png](http://static.toastoven.net/prod_role/role_6.7.png)
<center>[図6.7]ユーザーのロールに条件属性まで追加した様子です。</center>

### 7)権限チェック

`userId`のHeaderの`'uuid'`に値が渡されたとします。
`12345678-1234-5678-1234-567812345678`ユーザーが`/board/v1.0/1` APIを呼び出した時、権限をチェックすると下記のようになります。

#### [RESTful API呼び出しの場合]

```shell
curl -X POST -H "Content-Type: application/json" -d '{
    "resources": [
        {
            "attributes": {
                "attributeId": "instance.name",
                "attributeValue": "GPU"
            },
            "authRequestId": "",
            "operationId": "GET",
            "resourceId": "",
            "resourcePath": "/board/v1.0/1",
            "scopeId": "ALL"
        }
    ]
}' "https://role.api.nhncloudservice.com/role/v3.0/appkeys/{Appkey}/users/12345678-1234-5678-1234-567812345678/authorizations/resources"
```

レスポンス例) アクセス権限がある場合、そのリソース内部に`permission: true` でレスポンスが来ます。

```json
{
  "authorizations" : [
    {
      "attributes" : [
        {
          "attributeId" : "instance.name",
          "attributeValue" : "GPU"
        }
      ],
      "operationId" : "GET",
      "permission" : true,
      "resourceId" : "{boardId}",
      "resourcePath" : "/board/v1.0/1",
      "scopeId" : "ALL"
    }
  ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : ""
  }
}
```

## マイグレーション

ROLEサービスを使用する他のプロジェクトがある場合、データ移行機能を利用して便利にデータを同期させることができます。
データ同期の対象は`リソース`、`ロール`、`オペレーション`であり、`スコープ`と`ユーザー`は同期されません。

管理タブのマイグレーションメニュー領域でデータを移行するプロジェクトもしくはアプリキーを入力して進めることができます。

![role_8.1.png](http://static.toastoven.net/prod_role/role_8.1.png)
<center>[図8.1]管理タブに移動します。</center>

![role_8.2.png](http://static.toastoven.net/prod_role/role_8.2.png)
<center>[図8.2]マイグレーションメニューです。</center>

データを移行するプロジェクトを選択するか、直接アプリケーションキーを入力できます。

![role_8.3.png](http://static.toastoven.net/prod_role/role_8.3.png)
<center>[図8.3]プロジェクト選択ドロップダウンです。</center>

![role_8.4.png](http://static.toastoven.net/prod_role/role_8.4.png)
<center>[図8.4]アプリケーションキーを入力する様子です。</center>

![role_8.5.png](http://static.toastoven.net/prod_role/role_8.5.png)
<center>[図8.5] <strong>確認</strong> ボタンクリックした時に表示される確認モーダルです。</center>

## サーバー設定

![role_8.1.png](http://static.toastoven.net/prod_role/role_8.1.png)
<center>[図9.1]管理タブに移動します。</center>

![role_9.2.png](http://static.toastoven.net/prod_role/role_9.2.png)
<center>[図9.2]サーバー設定領域です。</center>

### クライアントSDKキャッシュ設定

クライアントSDKキャッシュの設定を行うことができます。
設定できる属性は`TTL`、`ID別サイズ`、`Path別サイズ`、`Tree別サイズ`があります。

### Resource Path Trailing Slash Match

リソースパスの最後の `'/'` に対して設定できます。
`Non Identical Path`に設定すると、`'/board/v1.0/{boardId}'`と `'/board/v1.0/{boardId}/'`は異なるパスになります。
しかし、`Identical Path` に設定した場合、 `'/board/v1.0/{boardId}'` と `'/board/v1.0/{boardId}/'` は同じパスになります。

## キャッシュの削除

クライアントSDKとサーバーのキャッシュのため、変更されたリソースの権限チェック結果がすぐに反映されない場合があります。
その場合、[管理]タブの**キャッシュの削除**ボタンを使って明示的にキャッシュを削除することで問題を解決することができます。

![role_8.1.png](http://static.toastoven.net/prod_role/role_8.1.png)
<center>[図10.1]管理タブに移動します。</center>

![role_10.2.png](http://static.toastoven.net/prod_role/role_10.2.png)
<center>[図10.2]キャッシュの削除ボタンです。</center>

![role_10.3.png](http://static.toastoven.net/prod_role/role_10.3.png)
<center>[図10.3]ボタンをクリックした後に開く削除確認モーダルです。</center>
