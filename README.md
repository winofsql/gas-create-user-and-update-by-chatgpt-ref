# gas-create-user-and-update-by-chatgpt-ref

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
