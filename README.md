# LAMP環境構築用Vagrantfile

## vagrant upで構築される環境

- CentOS7
- Apache 2.4
- MySQL5.7
- PHP 5.7

## 手順
1. 任意のディレクトリにプロジェクトディレクトリを作成
```
mkdir sample_project
```

2. Vagrantfileを配置
3. vagrantを立ち上げる
```
cd sample_project
vagrant up
```
4. 完了まで待つ
** もし共有フォルダのマウントのところでエラーが出る場合はプラグインをインストールしておく **
```
vagrant plugin install vagrant-vbguest
```
5. ルートディレクトリの編集
**vagrant up時に/var/www/appが作成されています**
```
 cd /etc/httpd/conf/
 sudo vi httpd.conf
```
### httpd.confの編集内容
```
#ドキュメントルートの指定
DocumentRoot /var/www/app

#ドキュメントルートのDirectoryの指定
<Directory "/var/www/app">

#Indexで読み込まれるファイルの指定、優先順位の変更
DirectoryIndex test.php index.html
```

6. httpd.confの編集内容の確認
```
httpd -t syntax確認
#=> Syntax OK
```

7. apache再起動
```
sudo systemctl restart httpd
```

8. SeLinuxの無効化
```
#有効になっているか確認
getenfoce #=> Enforcing

#一時的に無効にする場合
sudo setenforce 0
getenfoce #=> Permissive

#永続的に無効にする場合
/etc/selinux/configを編集
ELINUX=enabled　=> ELINUX=disabled
```
9. test.phpにアクセスできているか確認
`192.168.33.10`でアクセス


