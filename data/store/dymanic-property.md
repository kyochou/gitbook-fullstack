
# 动态属性
## 传统 DB 的 EAV 方法
EAV 是 Entity-Attribute-Value 的缩写. 它的核心思想是把原本按列保存的数据转换成按行保存. 核心通常是 entity, attribute, value 三张表.

```sql

CREATE TABLE eav_entities (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    entity_name VARCHAR(100) NOT NULL,
    PRIMARY KEY(id)
);

INSERT INTO `eav_entities`
    (`id`, `entity_name`)
VALUES
    (1,'普拉多 3.5L TX-L'),
    (2,'帕杰罗 3.0L 豪华版');

CREATE TABLE eav_attributes (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    attribute_name VARCHAR(100) NOT NULL,
    PRIMARY KEY(id)
);

INSERT INTO `eav_attributes`
    (`id`, `attribute_name`)
VALUES
    (1,'长度'),
    (2,'宽度'),
    (3,'高度'),
    (4,'GPS 导航系统'),
    (5,'倒车影像'),
    (6,'上坡辅助'),
    (7,'陡坡缓降');

CREATE TABLE eav_values (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    entity_id INT UNSIGNED NOT NULL,
    attribute_id INT UNSIGNED NOT NULL,
    attribute_value VARCHAR(100) NOT NULL,
    PRIMARY KEY(id)
);

INSERT INTO `eav_values`
    (`id`, `entity_id`, `attribute_id`, `attribute_value`)
VALUES
    (1,1,1,'4780'),
    (2,1,2,'1885'),
    (3,1,3,'1890'),
    (4,1,6,'有'),
    (5,1,7,'有'),
    (6,2,1,'4900'),
    (7,2,2,'1875'),
    (8,2,3,'1900'),
    (9,2,4,'有'),
    (10,2,5,'有');
```

## 使用 JSON 类型
MySQL5.7 开始已经支持 JSON 数据类型. 使用 JSON 来保存动态属性更方便.

## Refs
[关系数据库的 EAV 模型 VS JSON](https://mp.weixin.qq.com/s/jYt2HDqnIn2bgs5V1GG6QQ)

