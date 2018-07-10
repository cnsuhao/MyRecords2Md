---
title: java树形结构 两种算法
date: 2012-01-05 19:05:35
categories: "开发"
tags:
	- java

---

转载：http://www.javaeye.com/topic/602979



最近看到一个有意思的树形结构，为每个节点添加了lft和rgt两个属性。这样查找该节点的子节点、查找该节点所有父节点，就不用去递归查询，只需要用between、and语句就可以实现。下面以创建一个栏目树为例，以下是我的理解。

一般来讲，我们创建栏目树的时候，大多只需要一个外键parentid来区分该节点属于哪个父节点。数据库的设计如下图：

这样一来，

1.查找该节点的所有子节点，则需要采用sql的递归语句：select \* from tableName connect by prior id=sj\_parent\_id start with id=1(oracle 写法，mysql目前不支持，如果mysql想查找树形，可以利用存储过程).

2.查找该节点的父节点的sql递归语句：select \* from tableName connect by prior sj\_parent\_id =id start with id=1

如果数据量过大或者层次太多，那么这样操作是会影响性能的。

“任何树形结构都可以用二叉树来表示”。其实我们创建的栏目树就是一个简型的二叉树。根据数据结构里面二叉树的遍历，再稍微修改下，将数据库设计如下图所示：

，这样我们查找该节点的所有子节点，则只需要查找id在lft和rgt之间的所有节点即可。

1.查找该节点的所有子节点的Sql语句为：

select \* from tb\_subject s,tb\_subject t where s.lft between t.lft and t.rgt and t.id=1

2.查找该节点的所有父节点的sql语句为：

select s.\* from tb\_subject s,tb\_subject t where s.lft<t.lft and (s.rgt-s.lft)>1 and s.rgt>t.rgt and t.id=1

下面来详细讲解下，怎么用java来实现这种算法。

1. 新增节点

新增节点比较简单，基本步骤为

A． 查找当前插入节点的父节点的lft值

B． 将树形中所有lft和rgt节点大于父节点左值的节点都+2

C． 将父节点左值+1，左值+2分别作为当前节点的lft和rgt

因为项目中采用的是struts2+hibernate3.2+spring2.5的框架，代码如下：

**public boolean** onSave(Object entity, Serializable id, Object\[\] state,

String\[\] propertyNames, Type\[\] types) \{

**if** (entity **instanceof** HibernateTree) \{

HibernateTree tree = (HibernateTree) entity;

Long parentId = tree.getParentId();

String beanName = tree.getClass().getName();

Session session = getSession();

FlushMode model = session.getFlushMode();

session.setFlushMode(FlushMode.*MANUAL*);

Integer myPosition = **new** Integer(0);

//查找父节点的左值

**if** (parentId != **null**) \{

String hql = "selectb.lft from " + beanName

\+ " b whereb.id=:pid";

myPosition = (Integer)session.createQuery(hql).setLong("pid",

parentId).uniqueResult();

\}

//将树形结构中所有大于父节点左值的右节点+2

String hql1 = "update" + beanName

\+ " b setb.rgt = b.rgt + 2 WHERE b.rgt > :myPosition";

//将树形结构中所有大于父节点左值的左节点+2

String hql2 = "update" + beanName

\+ " b setb.lft = b.lft + 2 WHERE b.lft > :myPosition";

**if** (!StringUtils.*isBlank*(tree.getTreeCondition())) \{

hql1 += " and(" + tree.getTreeCondition() + ")";

hql2 += " and(" + tree.getTreeCondition() + ")";

\}

session.createQuery(hql1).setInteger("myPosition", myPosition)

.executeUpdate();

session.createQuery(hql2).setInteger("myPosition", myPosition)

.executeUpdate();

session.setFlushMode(model);

//定位自己的左值(父节点左值+1)和右值(父节点左值+2)

**for** (**int** i = 0; i < propertyNames.length; i++) \{

**if** (propertyNames\[i\].equals(HibernateTree.*LFT*)) \{

state\[i\] = myPosition + 1;

\}

**if** (propertyNames\[i\].equals(HibernateTree.*RGT*)) \{

state\[i\] = myPosition + 2;

\}

\}

**return true**;

\}

**return false**;

\}

2. 修改节点

修改的时候比较麻烦，具体步骤为：

在修改lft和rgt之前，当前节点的父节点id已经改变

a. 查出当前节点的左右节点（nodelft、nodergt），并nodergt-nodelft+1 = span，获取父节点的左节点parentlft

b. 将所有大于parentlft的lft(左节点)、rgt(右节点)的值+span

c. 查找当前节点的左右节点（nodelft、nodergt），并parentlft-nodelft+1 = offset

d. 将所有lft(左节点) between nodelft and nodergt的值+offset

e. 将所有大于nodergt的lft(左节点)、rgt(右节点)的值-span

Java代码如下：

**public void** updateParent(HibernateTree tree, HibernateTreepreParent,

HibernateTree curParent) \{

**if** (preParent != **null** && preParent !=**null**

&& !preParent.equals(curParent)) \{

String beanName = tree.getClass().getName();

// 获得节点位置

String hql = "selectb.lft,b.rgt from " + beanName

\+ " b whereb.id=:id";

Object\[\] position = (Object\[\]) **super**.createQuery(hql).setLong(

"id", tree.getId()).uniqueResult();

System.*out*.println(hql+"| id= "+tree.getId());

**int** nodeLft = ((Number) position\[0\]).intValue();

**int** nodeRgt = ((Number) position\[1\]).intValue();

**int** span = nodeRgt - nodeLft + 1;

// 获得当前父节点左位置

hql = "selectb.lft from " + beanName + " b where b.id=:id";

**int** parentLft = ((Number) **super**.createQuery(hql).setLong("id",

curParent.getId()).uniqueResult()).intValue();

System.*out*.println(hql+"| id= "+curParent.getId());

// 先空出位置

String hql1 = "update" + beanName + " b set b.rgt = b.rgt + "

\+ span + "WHERE b.rgt > :parentLft";

String hql2 = "update" + beanName + " b set b.lft = b.lft + "

\+ span + "WHERE b.lft > :parentLft";

**if** (!StringUtils.*isBlank*(tree.getTreeCondition())) \{

hql1 += " and(" + tree.getTreeCondition() + ")";

hql2 += " and(" + tree.getTreeCondition() + ")";

\}

**super**.createQuery(hql1).setInteger("parentLft", parentLft)

.executeUpdate();

**super**.createQuery(hql2).setInteger("parentLft", parentLft)

.executeUpdate();

System.*out*.println(hql1+"|parentLft = "+parentLft);

System.*out*.println(hql2+"|parentLft = "+parentLft);

// 再调整自己

hql = "selectb.lft,b.rgt from " + beanName + " b where b.id=:id";

position = (Object\[\]) **super**.createQuery(hql).setLong("id",

tree.getId()).uniqueResult();

System.*out*.println(hql+"| id= "+tree.getId());

nodeLft = ((Number) position\[0\]).intValue();

nodeRgt = ((Number) position\[1\]).intValue();

**int** offset = parentLft - nodeLft + 1;

hql = "update"

\+ beanName

\+ " b setb.lft=b.lft+:offset, b.rgt=b.rgt+:offset WHERE b.lft between :nodeLft and:nodeRgt";

**if** (!StringUtils.*isBlank*(tree.getTreeCondition())) \{

hql += " and(" + tree.getTreeCondition() + ")";

\}

**super**.createQuery(hql).setParameter("offset", offset)

.setParameter("nodeLft",nodeLft).setParameter("nodeRgt",

nodeRgt).executeUpdate();

System.*out*.println(hql+"|offset = "+offset+" | nodelft = "+nodeLft+" | nodergt = "+ nodeRgt);

// 最后删除（清空位置）

hql1 = "update" + beanName + " b set b.rgt = b.rgt - " + span

\+ " WHEREb.rgt > :nodeRgt";

hql2 = "update" + beanName + " b set b.lft = b.lft - " + span

\+ " WHEREb.lft > :nodeRgt";

**if** (tree.getTreeCondition() !=  **null**) \{

hql1 += " and(" + tree.getTreeCondition() + ")";

hql2 += " and(" + tree.getTreeCondition() + ")";

\}

**super**.createQuery(hql1).setParameter("nodeRgt", nodeRgt)

.executeUpdate();

**super**.createQuery(hql2).setParameter("nodeRgt", nodeRgt)

.executeUpdate();

System.*out*.println(hql1+"|nodeRgt = "+nodeRgt);

System.*out*.println(hql2+"|nodeRgt = "+nodeRgt);

\}

\}

3. 删除节点

删除节点也比较简单，具体步骤为：

A． 查找要删除节点的lft值

B． 将所有lft和rgt大于删除节点lft值的都-2

Java代码如下：

**public void** onDelete(Object entity, Serializable id, Object\[\] state,

String\[\] propertyNames, Type\[\] types) \{

**if** (entity **instanceof** HibernateTree) \{

HibernateTree tree = (HibernateTree) entity;

String beanName = tree.getClass().getName();

Session session = getSession();

FlushMode model = session.getFlushMode();

session.setFlushMode(FlushMode.*MANUAL*);

//查找要删除的节点的左值

String hql = "selectb.lft from " + beanName + " b where b.id=:id";

Integer myPosition = (Integer)session.createQuery(hql).setLong(

"id", tree.getId()).uniqueResult();

//将所有大于删除节点左值的rgt都-2

String hql1 = "update" + beanName

\+ " b setb.rgt = b.rgt - 2 WHERE b.rgt > :myPosition";

//将所有大于删除节点左值的lft都-2

String hql2 = "update" + beanName

\+ " b setb.lft = b.lft - 2 WHERE b.lft > :myPosition";

**if** (tree.getTreeCondition() !=  **null**) \{

hql1 += " and(" + tree.getTreeCondition() + ")";

hql2 += " and (" + tree.getTreeCondition() + ")";

\}

session.createQuery(hql1).setInteger("myPosition", myPosition)

.executeUpdate();

session.createQuery(hql2).setInteger("myPosition", myPosition)

.executeUpdate();

session.setFlushMode(model);

\}

\}

