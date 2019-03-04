# cloudformation-sso-app-template

### 前提条件

- [openam-template](https://github.com/ot-nemoto/cloudformation-openam-template)で環境構築済みであること
  - このStackで作成したVPC上にデモ環境を構築することを前提としています
  - このStackのStack名を `openam` 以外で作成している場合、パラメータ `OpenAMStackName` にStack名を指定（StackのOutputを利用しているため）
- **aws-cli** がインストールされ、プロファイルの設定がされていること
- HostedZoneに **ドメイン** が登録されていること
- 対象のRegionに **キーペア** が作成されていること
- 実行するIAMユーザに十分な権限があること

### 構築手順

- 環境変数を設定
  - 作成済み（作成した）キーペアのキー名
  - Route53に登録済みのHostedZone名
  - OpenAMのURI
  - OpenAMのAdminユーザのユーザ名
  - OpenAMのAdminユーザのパスワード
  - OpenAMユーザがログインする際のAWSロールのARN
  - OpenAMユーザのロールの信頼されたエンティティのARN
  - onloginのURLを指定
  - Adminユーザでログインし DEVELOPERS > API Credentials で登録したクライアントID
  - Adminユーザでログインし DEVELOPERS > API Credentials で登録したシークレットID
  - Adminユーザでログインし USERS > Roles > Role のURLから確認できるロールID
  - Adminユーザでログインし APPS > Company Apps > App のURLから確認できるアプリID

```sh
# 例)
KEY_NAME=openam-key
HOSTED_ZONE_NAME=example.com.
OPENAM_URL=http://opneam.example.com/openam
OPENAM_ADMIN_USER=amAdmin
OPENAM_ADMIN_PASS=password
OPENAM_AWS_ROLE_ARN=arn:aws:iam::<Account Id>:role/<Role Name>
OPENAM_AWS_ID_PROVIDER_ARN=arn:aws:iam::<Account Id>:saml-provider/<Provider Name>
ONELOGIN_URI=https://<OneLogin Domain>/onlgoin.com
ONELOGIN_CLIENT_ID=0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
ONELOGIN_CLIENT_SECRET=0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
ONELOGIN_ROLE_ID=12345
ONELOGIN_APP_ID=67890
```

- 環境構築

```sh
aws cloudformation create-stack \
  --stack-name sso-app \
  --parameters ParameterKey=KeyName,ParameterValue=${KEY_NAME} \
               ParameterKey=HostedZoneName,ParameterValue=${HOSTED_ZONE_NAME} \
               ParameterKey=OpenAMUri,ParameterValue=${OPENAM_URI} \
               ParameterKey=OpenAMAdminUser,ParameterValue=${OPENAM_ADMIN_USER} \
               ParameterKey=OpenAMAdminPass,ParameterValue=${OPENAM_ADMIN_PASS} \
               ParameterKey=OpenAMAwsRoleArn,ParameterValue=${OPENAM_AWS_ROLE_ARN} \
               ParameterKey=OpenAMAwsIDProviderArn,ParameterValue=${OPENAM_AWS_ID_PROVIDER_ARN} \
               ParameterKey=OneloginUri,ParameterValue=${ONELOGIN_URI} \
               ParameterKey=OneloginClientId,ParameterValue=${ONELOGIN_CLIENT_ID} \
               ParameterKey=OneloginClientSecret,ParameterValue=${ONELOGIN_CLIENT_SECRET} \
               ParameterKey=OneloginRoleId,ParameterValue=${ONELOGIN_ROLE_ID} \
               ParameterKey=OneloginAppId,ParameterValue=${ONELOGIN_APP_ID} \
  --template-body file://sso-app-template.yml
```

### Parameters

|Name|Type|Default|Required|
|--|--|--|--|
|ProjectName|String|sso-app|*No*|
|DeveloperCIDR|String|0.0.0.0/0|*No*|
|InstanceType|String|t2.micro|*No*|
|KeyName|AWS::EC2::KeyPair::KeyName|-|*Yes*|
|HostedZoneName|String|-|*Yes*|
|OpenAMStackName|String|openam|*No*|
|OpenAMUri|String||*No*|
|OpenAMAdminUser|String||*No*|
|OpenAMAdminPass|String||*No*|
|OpenAMAwsRoleArn|String||*No*|
|OpenAMAwsIDProviderArn|String||*No*|
|OneloginUri|String||*No*|
|OneloginClientId|String||*No*|
|OneloginClientSecret|String||*No*|
|OneloginRoleId|String||*No*|
|OneloginAppId|String||*No*|
