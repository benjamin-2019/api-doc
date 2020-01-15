# 模型

模型是对实体资源的抽象和映射，当前框架并没有在模型层与逻辑业务分层上最更多的区分，
模型处理了具体某个资源的业务逻辑以及增删改查与资源搜索

## 文件声明
```php
<?php

/**
 * 玩家层级表
 */

namespace Api\Model;

use Api\Core\Mvc\Model;             // 模型基类

class Level extends Model
{
    const TABLE_PK   = 'id';        // 声明表主键（必须）
    const TABLE_NAME = 'level';     // 声明表名称（必须）

    /**
     * @title 根据ID获取层级信息
     * @param $id
     * @return mixed
     * @author benjamin
     */
    public static function getLevelById ( $id ) {
        return self::findByPk($id);
    }

}

```

## 查询 - select
* 单条数据查询

```php
// 根据主键查询
$findById = Members::findByPk($params['id'], 'uid, username, type, name, host');

// 根据某个字段查询
$findByUsr = Members::findOne([ 'username' => $params['usr'] ], 'uid, username, name');

// 原生SQL查询
$findBySql = Members::findOneBySql('SELECT * FROM `ssc_members` WHERE uid = :uid ', ['uid' => 1]);

# 多条件查询
$findOne = Members::findOneBySearch([
    'columns' => 'username, password, name',
    'data'    => [ 'uid' => 2 ],
    'where'   => [ 'regTime <= ' . time() ]
]);
```

* 列表数据查询

```php
// 普通数据列表：data为数组，列入了要查询的字段和值，适用于简单左右相等的列表获取
$loginList = MemberSession::getDataList([
    'columns' => 'username',
    'data'    => [ 'loginIP' => $loginIp ],
]);

// 普通数据列表：处理in查询
$loginList = MemberSession::getDataList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp, 'uid' => [1, 2, 3, 4] ],
]);

// 复杂查询列表：where为数组，能处理所有查询，但依靠条件拼装
$loginList = MemberSession::getDataList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp ],
    'where'   => [ "accessTime > {$lastTime}", "username NOT LIKE 'guest_%'" ],
]);

// 带索引数据列表：声明 'arrKey', 值为索引的字段 
$loginList = MemberSession::getDataList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp ],
    'where'   => [ "accessTime > {$lastTime}", "username NOT LIKE 'guest_%'" ],
    'arrKey'  => 'username'
]);

// 分页数据列表: 通过传递：page + rows 参数，但目前底层已自动获取适配
$loginList = MemberSession::getPageList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp ],
    'where'   => [ "accessTime > {$lastTime}", "username NOT LIKE 'guest_%'" ],
    'arrKey'  => 'username'
]);

```

* 获取数据并生成缓存

```php
// 单条数据缓存
$game = Type::getOneByCache([ 'data' => [ 'id' => $gameId ], 'columns' => Type::$COLUMNS['bet'] ], 120);

// 列表数据缓存
$methods = Played::getListByCache([
    'data'   => [ 'type' => $gameId, 'name' => $name ],
    'arrKey' => 'id'
], 10);

```


## 新增 - insert
```php

// 获取参数
$usr = $this->getSafeDataByKey('usr', $this->_conf['user']['usr']);

// 构建数据
$data = [
    'username'    => $usr,
    'attemptTime' => 1,
    'lockTime'    => $this->now(false),
    'loginIp'     => $this->IP(true)
];

// 新增数据：响应：bool
$addOne = MemberAttempt::insert($data);

// 检查状态
if ( !$addOne ) {
    throw new ErrMsg('新增错误信息异常, ' . ERR_MSG);
}

// 响应结果
$this->success('新增成功', [
    'status' => $addOne,
    'data'   => $data
]);

```

## 更新 - update

```php

// 参数1：更新条件: 为SQL语句（处理任意查询条件）, 参数2：
$update = self::update("uid = {$user['uid']}", $update);
if ( !$update ) {
    throw new ErrMsg('用户密码更新失败，' . ERR_MSG);
}

// 更新条件：为数组（只能处理相等关系）
$updateStatus = MemberAttempt::update(
    // [ 'username' => $usr ],
    [ 'username' => $usr, 'type' => 1 ],
    [ 'attemptCount' => rand(1, 4), 'lockTime' => rand(0, -1) ]
);

// 更新常用操作
try {
    $update = array_merge([
        'updateTime' => date('Y-m-d H:i:s')
    ], $data);

    $res = self::update("username = '{$usr}'", $update);
    
    if ( !$res ) {
        throw new ErrMsg('用户密码更新失败，' . ERR_MSG);
    }

} catch (\Exception $e) {
    throw new ErrMsg('用户资料更新失败，' . ERR_MSG);
}

```

## 删除 - delete

```php

# 删除单条数据：根据条件（处理任意查询条件）
$deleteOne = Members::delete("`uid` = {$findByUsr['uid']}");

# 删除单条数据：根据数组（只能处理相等关系）
$deleteOne = MemberAttempt::delete([ 'username' => $usr, 'attemptCount' => rand(1, 4) ]);

```

## 读写锁 - rwLock

必须在`事务`里进行，并且`查询条件`必须带`索引`，否则会造成 `表锁`

`加锁顺序`尽量保持一致，顺序不一致会导致互相`等待锁释放`、造成 `死锁`


```php
try {

    Model::begin();
    
    # 行锁用户
    $user = Members::getUserById($uid, [ 
        'columns' => Members::$COLUMNS['auth'], 
        'rwLock' => true 
    ]);
    
    if ( empty($user) ) {
        throw new ErrMsg('当前用户信息异常，' . ERR_MSG);
    }
    
    # 扣钱操作
    do xxxx ... 
    
    Model::commit();
    
} catch (Exception $e) {
    Model::rollback();
    throw new ErrMsg($e->getMessage());
}

```

## 增减量更新 - decr|incr

这两个方法其实没有区别，区别在于传入的值的正负

```php
# 核心方法
final static function incrOrDecr ( $data, $field, $num = 1, $where = [] ) {}

# 普通增减量更新1：参数1：相等条件, 参数2：字段，参数3：字段值
MemberAppend::incr(['uid'=> $user['uid']], 'dml_real', 3)

# 普通增减量更新2：参数1：相等条件, 参数2：数组 + 字段和值
MemberAppend::incr(['uid'=> $user['uid']], ['dml_real'=>3 ])

# 多字段增减量更新: 参数1：相等条件, 参数2：数组 + 多字段和值
MemberAppend::incr(['uid'=> $user['uid']], ['dml_real'=>3, 'age'=>-20])

# 多字段多条件增减量更新
# 注册送彩金：活动统计-出现超卖问题解决
if ( $registerCoin > 0 ) {
    $regCoinAct = $this->regConf['regCoinAct'];

    # 增量更新：活动参与人数、领取额度
    Activity::incr([ 'id' => $regCoinAct['id'] ], [ 'used_amount' => $registerCoin, 'used_member' => 1 ], 0, [
        'used_member + 1 <= ' . ($regCoinAct['max_member']), 'used_amount + ' . $registerCoin . ' <= ' . ($regCoinAct['max_amount'])
    ]);

    # 如果更新条数为0、说明库存已卖完
    $rowCount = Activity::rowCount();

    # 将奖励金额重置为0：后续不再过多处理
    if ( $rowCount === 0 ) $registerCoin = 0;
}

```

## 总更新条数 - rowCount

获取每一条非查询语句执行影响条数，生命周期在当前SQL执行完，到下次SQL执行前

```php
// 如果更新条数为0、说明库存已卖完
$rowCount = Activity::rowCount();

```