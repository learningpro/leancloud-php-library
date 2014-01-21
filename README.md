## PHP AVOS Cloud Library
===========================
Forked from https://github.com/apotropaic/parse.com-php-library

AVOS Cloud PHP SDK。

## 配置
=========================

checkout这个项目后，你需要在项目根路径创建一个文件名为`AVConfig.php`作为配置文件。

```
git clone https://github.com/killme2008/avoscloud-php-library.git
cd avoscloud-php-library ; touch AVConfig.php
```

## AVConfig.php示范

填写AVConfig.php配置示范如下：

```
<?php

class AVConfig{

    const APPID = '';
    const MASTERKEY = '';
    const APPKEY = '';
    const AVOSCLOUDURL = 'https://cn.avoscloud.com/1/';
}

?>
```

其中APPID就是应用Id，MasterKey为应用的Master Key，APPKEY为应用Key。这些信息都可以在应用设置的应用key菜单里找到。

你可以通过`php test.php`命令运行单元测试。


## 简单例子
=========================

例子在samples目录下，更多例子参考tests目录下的测试用例。每个例子前都需要导入SDK:

```
<?php
	include_once 'AV.php';
    ?>
```

### 对象

创建对象：

```
	$obj = new AVObject('GameScore');
	$obj->score = 1000;
	$obj->name = 'dennis zhuang';
	$save = $obj->save();
	print_r($rsave);
```

更新对象：

```
	$updateObject = new AVObject('GameScore');
	$updateObject->score = 2000;
	$return = $updateObject->update($save->objectId);
```

删除对象：

```
	$deleteObject = new AVObject('GameScore');
	$return = $deleteObject->delete($save->objectId);
```

### 查询

```
        $query = new AVQuery('GameScore');
    	$query->where('name','dennis zhuang');
		$return = $query->find();
		print_r($return);
```

各种查询条件的where方法请看AVQuery.php源码。

### 文件

上传文件：

```
	$file = new AVFile('text/plain', 'Working at AVOS Cloud is Great');
	$save = $file->save('hello.txt');
	print_r($save);
```
关联文件到某个对象上：

```
    $obj = new AVObject('GameScore');
    $obj->image = $obj->dataType('file', $save->objectId);
    $obj->save();
```

### GeoPoint（地理信息位置）

```
	 $geo = new AVGeoPoint(30.0, -20.0);
	 $obj->location = $geo->location;
	 $return = $obj->save();
```

根据地理位置查询附近的对象：

```
	$query = new AVQuery('GameScore');
	$query->whereNear('location', (new AVGeoPoint(30.0, -20.0))->location);
	$return = $query->find();
```

### 消息推送

推送消息：

```
	$push =new AVPush;
	$push->alert = 'Hello from AVOS Cloud';
	$push->channels = array('foo', 'bar');
	$return = $push->send();
```

### 用户

注册用户：

```
    $avUser = new AVUser;
    $avUser->email = 'killme2008@gmail.com';
	$user = $avUser->signup('dennis', 'password');
    print_r($user);
```

用户登陆：

```
	$loginUser = new AVUser;
	$loginUser->username = 'dennis';
	$loginUser->password = 'password';
	$returnLogin = $loginUser->login();
```

重设密码：

```
	$user->requestPasswordReset('killme2008@gmail.com');
```
