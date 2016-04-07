#### 按主键查询
```
$entity = \DoctrineExt::find('Sentrybox', $id);
$entity->getAreaId()->getName();
```

#### 非主键查询
```
$entity = \DoctrineExt::getRepository('EmParts')->findOneBy(array('name' => '门'));
$entity->getAreaId()->getName();
```

#### 更新
```
$entity = \DoctrineExt::find('Sentrybox', $id);
$entity->setName('新名字');
\DoctrineExt::flush();
```

#### 插入
```
$entity = new ORM\Sentrybox();
$entity->setName('插入新部位');
$entity->setAreaId(\DoctrineExt::find('EmArea', 97));
\DoctrineExt::persist($entity);
\DoctrineExt::flush();
```
##### 可用链调用
`\DoctrineExt::persist($entity)->flush();`

#### 删除
1. 常规调用
```
$entity = \DoctrineExt::find('Sentrybox', $id);
\DoctrineExt::remove($entity);
\DoctrineExt::flush();
```

2. 链调用
`\DoctrineExt::remove(\DoctrineExt::find('EmParts', 140))->flush();`

#### 属性设置器
```
$en = new ORM\Sentrybox();
\DoctrineExt::setAttributes($en, array('communityId' => 12343, 'name' => '就是这样'))
	->flush(); // 不用再 persist() 了
```

#### 事务
1. 传统手动调用
```
\DoctrineExt::beginTransaction();
try {
    $entity = new ORM\Sentrybox();
    $entity->setColumns(array('corp_id' => 272, 'area_id' => 97, 'name' => '呵呵', 'standard' => '标准'));
    \DoctrineExt::persist($entity)->flush()->commit();
} catch (Exception $ex) {
    \DoctrineExt::rollback();
}
```

2. 回调函数自动事务
```
\DoctrineExt::transactional(function() {
    $entity = new ORM\Sentrybox();
    $entity->setColumns(array('corp_id' => 272, 'area_id' => 97, 'name' => '呵呵', 'standard' => '标准'));
    \DoctrineExt::persist($entity);
});
```

#### 面向数据库的查询生成器 SQL语法
```
\DoctrineExt::createQueryBuilder() 与 \DoctrineExt::createDatabaseQueryBuilder() 一样
$query = \DoctrineExt::createQueryBuilder()->select('name')->from('om_s_sentrybox'); // 用表字段和表名
$query->execute()->fetchAll()
```

#### 面向实体类的查询生成器 DQL语法 只想取出部分属性的话，必须加 partial 关键字
```
$query = \DoctrineExt::createEntityQueryBuilder()->select('s')->from('\Fookii\Sentrybox\ORM\Sentrybox', 's'); // 用实体类及其属性
$query->getQuery()->getArrayResult();
```

#### DQL 查询
```
$query = \DoctrineExt::createQuery('select a from \Fookii\Sentrybox\ORM\Sentrybox a');
$query->getArrayResult();
```

#### 取结果集总项数 包括分页查询
```
$query = \DoctrineExt::createQuery('select a from \Fookii\Sentrybox\ORM\Sentrybox a')->setFirstResult(0)->setMaxResults(2);
\DoctrineExt::getCount($query);
```

#### IN 语句查询
```
$query = \DoctrineExt::createQueryBuilder('e');
$where = $query->expr()->in('e.postId', $postId); //$postId可以是id串，也可以是id数组
$query->andWhere($where);
```