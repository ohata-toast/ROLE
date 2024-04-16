## Application Service > ROLE > リリースノート

### 2024. 04. 23.
#### 機能追加
* [RESTful API]ロールリスト照会、ロール単件照会APIが拡張されました。
  * 関連関係ロールリストにロールタグリストが追加されました。
    * POST /role/v3.0/appkeys/{appKey}/roles/search:ロールリスト照会
        * 詳細については、マニュアルを参照: [リンク](https://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-v3-guide/#searchRoles)
    * GET /role/v3.0/appkeys/{appKey}/roles/{roleId}:ロール単件照会
        * 詳細については、マニュアルを参照: [リンク](https://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-v3-guide/#getRole)

#### バグ修正
* [RESTful API]ロール作成、ロール修正APIリクエスト時にroleApplyPolicyCode(ロール使用有無)項目を反映しないエラーが修正されました。
* [RESTful API]ロール作成、ロール修正APIリクエスト時にconditions(ロール条件属性)の一部有効性検証が失敗するエラーが修正されました。

### 2024. 02. 27.
#### 機能追加
* ABAC(属性ベースのアクセス制御)機能が追加されました。
  * [Console]新しいデザインが適用され、条件属性機能が追加されました。
      * ロールに条件属性を追加できます。
      * 「条件属性」タブが追加されました。
      * 「管理」タブが追加されました。
  * [RESTful API] Public API v3がリリースされました。
      * RBAC(ロールベースのアクセス制御(RBAC)とロールに対するABAC(属性ベースのアクセス制御)を提供します。
      * 特定のロールを未使用にしたり、条件を設定することで、多様で細かなユーザーアクセス制御が可能です。
  * [SDK] 2.0.0でリリースされました。
      * RESTful APIで新たに提供されるv3に合わせて適用しました。

#### サポート終了
* [Console] Excelアップロード/Excelダウンロード機能をサポートしません。
* [RESTful API] Public API v1でUserにRoleを付与する際の有効期間設定機能をサポートしません。
	* この機能は、ABAC(属性ベースのアクセス制御)を通じて提供します。
* [SDK] 1.xバージョンでUserにRoleを付与する際、有効期間設定機能をサポートしません。
	* この機能はABAC(属性ベースのアクセス制御)を通じて提供します。

### 2023. 09. 26.
#### 機能修正
* Role作成時、Role idの命名ルールが変更されました。
    * Role idの最大文字数が従来の32文字から128文字に増えました。
    * 従来は`_`と`-`のみ許可されていた特殊文字が`.`と`:`も許可されるようになりました。

### 2019. 11. 26.
#### 機能追加
* UserにRoleを付与する際、有効期間を設定できます。
    * Userに付与されたRoleは設定した有効期間内のみ有効です。逆に設定しない場合、付与された権限は常に有効です。
    * 有効期間は「日」単位で設定できます。
    * 有効期間の開始日は、現在の時点以降に設定することができ、設定した日から付与された権限が有効になります。
    * 有効期間は開始日のみ設定できます。この場合、付与された権限は設定した日付から常に有効です。
* roleに表示順序、tagを設定できます。
    * role リストを検索する際、表示順で並べ替えを行います。
    * 複数のtagを設定可能で、検索キーワードとして使用できます。

### 2019. 06. 25.
#### 機能追加
* [Console] Resource PathのTrailing Slash設定をできます。
    * Non Identical Path設定時、"/admin"と"/admin/"は異なるパスとして認識します。
    * Identical Path設定時、"/admin"と"/admin/"は同じパスとして認識します。

### 2018. 02. 22.
#### 機能追加
* [Console] Resource項目のpathでantPathPatternをサポートします。 
    * "/admin/**"設定時、adminの下のresource pathでauthorizationチェックをサポートできます。
* [Console] Role項目のうち、RoleNameとRoleGroupが追加され、管理しやすくなりました。
    * RoleName : Roleに意味のある名前を付与して管理できます。
    * RoleGroup : グループを指定し、グループ別検索で管理できます。
* [Console] ResourceのResource IDの長さが64文字に増加しました。
* [RESTful API] Role項目にRoleName、RoleGroupが追加され、Role関連APIが拡張されました。
    * 詳細については、マニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#3-role)
* [SDK] 1.1.7にリリースされました。
    * セキュリティ強化のためにcommons-colllection 3.2.2 を適用しました。
    

### 2017. 08. 24.
#### 機能追加
* [RESTful API] 各コンポーネントのリストを照会できるAPIを追加しました。
	* GET /role/v1.0/appkeys/{appKey}/roles : roleリストの照会
		* 詳細については、マニュアル参考: [リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#3-role)
	* GET /role/v1.0/appkeys/{appKey}/resources : resourceリストの照会
		* 詳細についてはマニュアルを参考：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#4-resource)
	* GET /role/v1.0/appkeys/{appKey}/scopes : scopeリストの照会
		* 詳細については、マニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#2-scope)
	* GET /role/v1.0/appkeys/{appKey}/operations : operationリストの照会
		* 詳細については、マニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#5-operation)

#### 機能改善/変更
* [Console] Resource nameにハングルを入力できます。 '/'文字を除くすべての文字を入力できます。
* [Console] Resource、Role、Role、User、Scopeの入力、修正時、フィールド有効性検査が失敗した時に表示されるメッセージが修正されました。
	* Resourceのpriority, descriptionの有効性検査時、4XX、5XXエラーではなくフレーズを表示するように改善されました。
		* priority有効性検査失敗文言：「Priorityは数字(-32768 ~ 32767)のみ入力可能です。」
		* description有効性検査失敗文言：「無効な形式のdescriptionです。」
	* Scope、Role、Userのdescriptionが128文字を超える場合、4XX、5XXエラーではなく、フレーズを表示するように改善されました。
		* description有効性検査失敗文言：「無効な形式のdescriptionです。」
	* User修正画面で存在しないRoleやScopeを入力した場合、11001、13001エラーではなくフレーズを表示するように改善されました。
		* 存在しないScope入力時のフレーズ：「Scope IDが見つかりません。」
		* 存在しないRole入力時のフレーズ：「Role IDが見つかりません。」
* [Console] Resource検索画面のOperationフィールドにオートコンプリート機能が追加されました。
* [Console] Migration機能の誤用を防止するために、画面に注意文言が追加されました。
	* 注意文言：「※注意：現在のプロジェクトのResource、Role、Operationを選択したプロジェクトにコピーを進めます。選択したプロジェクトの既存のResource、Role、Operationは削除されます。」
* [RESTful API] API制約事項が変更されました。
	* GET /role/v1.0/appkeys/{appKey}/resources/hierarchy APIがuserやroleを引数に指定しなくても、全ての結果を返すように変更されました。
		* 詳細についてはマニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#4-resource)

#### バグ修正
* [Console] Resourceの修正画面でサブリソースを持つリソースのnameを変更する時、5XXエラーが発生するバグを修正しました。
	* 正常に変更されたUi Pathが下位Resourceにも反映されます。 
* [Console] Resource検索画面で、存在しないユーザーで検索すると、すべてのリソースが検索されるエラーを修正しました。
	* Resource Treeに表示されるResourceがありません。
* [Console] Resource修正時に重複したnameで修正をリクエストした後、キャンセルすると、適用された値に見えるエラーを修正しました。
	* 以前の状態で表示されます。
* [Console] Role検索画面でdescriptionをキーワードで検索すると、Total値が正常に変更されないエラーを修正しました。
	* Total値が検索された件数値に反映されます。
* [Console] User画面で検索された状態でRoleを追加または削除すると、検索されたリスト画面に反映されないエラーを修正しました。
	* Userリスト画面にすぐ反映されて表示されます。
* [Console] User修正画面でTitlが「Userの追加」から「Userの修正」に修正されました。
 

### 2017. 07. 20.
#### バグ修正 
* [Console]すでに使用中のResource、Role、Scopeの名前で登録/修正時に反映されず、失敗警告ウィンドウが表示される問題を修正しました。 
	
### 2017. 05. 25.
#### バグ修正
* [Console] Resourceタブの[Excelアップロード]機能が動作しない問題を修正 

### 2017. 04. 20.
#### バグ修正
* userId(key)のvalue値に'.', '@'が含まれる場合、エラーが返されるバグを修正

### 2016. 12. 22.
#### 機能改善/変更
* バルクUserリスト照会APIを追加
* Scopeと関連した関連関係照会APIを追加

#### バグ修正
* Role関連関係が正しく動作しない問題を修正しました。
* User検索時、ScopeID ALLが検索されない問題を修正
* 無効なresource treeがある場合、hierarchy treeが異常に返されるバグを修正

### 2016. 11. 24.
#### 機能改善/変更
* Client SDK及びサーバーのCacheを削除する機能を追加

#### バグ修正
* Resource Pathを修正する時'/'で始まらない間違ったPathに修正できないように修正
	* 間違ったPathがある場合、Excelを利用したデータ入力ができない場合がある

### 2016. 10. 20.
#### 機能改善/変更
* Userリストの照会時、関連関係にあるRoleを持つユーザーも返すオプションを追加
	* 詳細については、マニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#1-user)

### 2016. 09. 29.
#### 機能改善/変更
* Userに新規Roleを付与する際、既に登録されているRoleのうちScopeが同じRoleを削除するAPIを追加
* RoleにUserを追加するAPIでUserがない場合、Userを生成するオプションを追加
	* 詳細については、マニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#3-role)

### 2016. 08. 18.
#### 機能改善/変更
* 使いやすさが落ちるため、Polling APIのサポートを終了しました。
* Role商品を利用するProject間のデータをMigrationする機能を追加
    * 詳細については、マニュアルを参照：[リンク](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/console-guide/#_3)

#### バグ修正
* Roleを削除すると、他のRoleの関連情報が削除されるバグを修正
* 権限チェックが断続的に失敗するバグを修正
