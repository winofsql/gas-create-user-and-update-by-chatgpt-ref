# gas-create-user-and-update-by-chatgpt-ref

ChatGPT にもらったのは以下です。それを使いました。
( ChatGPT は orgUnitPath を使うの忘れてましたけど )

![image](https://user-images.githubusercontent.com/1501327/223959847-60fd2950-e74c-44f5-9f8b-25ad03d26195.png)

```javascript
function createAccount() {
  var domain = 'example.com'; // 自分のドメインに変更してください
  var user = 'testuser'; // 作成するユーザーのユーザー名を指定してください
  var password = 'testpassword'; // ユーザーのパスワードを指定してください
  var recoveryEmail = 'recovery@example.com'; // 復旧用のメールアドレスを指定してください
  var givenName = 'Test'; // ユーザーの名前を指定してください
  var familyName = 'User'; // ユーザーの姓を指定してください

  var orgUnitPath = '/'; // 組織単位のパスを指定してください。デフォルトはルートになります。

  var userObject = {
    primaryEmail: user + '@' + domain,
    password: password,
    name: {
      givenName: givenName,
      familyName: familyName
    },
    recoveryEmail: recoveryEmail
  };

  try {
    var createdUser = AdminDirectory.Users.insert(userObject);
    Logger.log('User %s created with email address %s', createdUser.name.fullName, createdUser.primaryEmail);
  } catch (e) {
    Logger.log('Error creating user: %s', e);
  }
}
```

### 以下は ChatGPT の解説
このコードでは、createAccount() 関数を定義しています。この関数内で、以下のパラメータを指定しています。

- domain: 自分のドメイン名を指定します。
- user: 作成するユーザーのユーザー名を指定します。
- password: ユーザーのパスワードを指定します。
- recoveryEmail: 復旧用のメールアドレスを指定します。
- givenName: ユーザーの名前を指定します。
- familyName: ユーザーの姓を指定します。

また、orgUnitPath パラメータを使用して、ユーザーを組織単位に割り当てることができます。この例では、ルート組織に割り当てられます。

userObject 変数には、上記で指定したパラメータを設定し、AdminDirectory.Users.insert() メソッドを使用してユーザーアカウントを作成します。作成されたアカウントの詳細は createdUser 変数に保存され、Logger.log() メソッドを使用してログに出力されます。

注意点として、このコードを実行するには、**Google Workspace 管理者としてログインしている必要があります。** また、このコードは、一度だけ実行することを想定しています。



### A 列に番号(ID)、B列に氏名

```javascript
function splitName() {

  var sheet = SpreadsheetApp.getActiveSheet();

  var target = "";

  var targetRange = null;

  var targetWork = null;

  for(var i = 2; i <= 100; i++ ) {

    targetRange = sheet.getRange('B' + i);
    target = targetRange.getValue().toString();

    if ( target == "" ) {
      break;
    }

    targetWork = target.split(/[\s　]+/)

    console.log(targetWork);

    targetRange = sheet.getRange('C' + i);
    targetRange.setValue( targetWork[0] );
    targetRange = sheet.getRange('D' + i);
    targetRange.setValue( targetWork[1] );

  }  

}

function createAccount() {
  var domain = ''; // 自分のドメインに変更してください
  var password = ''; // ユーザーのパスワードを指定してください
  var recoveryEmail = ''; // 復旧用のメールアドレスを指定してください

  var user = ''; // 作成するユーザーのユーザー名を指定してください

  var orgUnitPath = '/部署/グループ等'; // 組織単位のパスを指定してください。デフォルトはルートになります。

  var sheet = SpreadsheetApp.getActiveSheet();
  var targetRange = null;

  for(var i = 2; i <= 100; i++ ) {

    targetRange = sheet.getRange('A' + i);
    user = targetRange.getValue().toString();

    if ( user == "" ) {
      break;
    }

    var userObject = {
      primaryEmail: user + '@' + domain,
      password: password,
      name: {
        givenName: sheet.getRange('D' + i).getValue().toString(),
        familyName: sheet.getRange('C' + i).getValue().toString()
      },
      recoveryEmail: recoveryEmail,
      orgUnitPath : orgUnitPath
    };

    console.log(userObject);

    try {
      var createdUser = AdminDirectory.Users.insert(userObject);
      Logger.log('User %s created with email address %s', createdUser.name.fullName);
    } catch (e) {
      Logger.log('Error creating user: %s', e);
    }


  }  


}

function updatefamilyName() {
  var domain = ''; // 自分のドメインに変更してください

  var sheet = SpreadsheetApp.getActiveSheet();
  var targetRange = null;

  for(var i = 2; i <= 100; i++ ) {

    targetRange = sheet.getRange('A' + i);
    user = targetRange.getValue().toString();

    if ( user == "" ) {
      break;
    }


    var userObject = {
      "name": {
        "familyName": user + ' ' + sheet.getRange('C' + i).getValue().toString()
      }
    };

    console.log(userObject);

    try {
      var updateUser = AdminDirectory.Users.update(userObject, user + '@' + domain);
      Logger.log('User %s created with email address %s', updateUser.name.fullName);
    } catch (e) {
      Logger.log('Error creating user: %s', e);
    }


  }  


}
```
