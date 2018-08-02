---
layout: post
title:  "Mysql Notes"
date:   2018-08-02 11:15:00 +0800
featured-img: mysql
categories: mysql
---

# Mysql Notes

用navicat等客户端连接mysql8时，可能会遇到`[Authentication plugin 'caching_sha2_password' cannot be loaded]`的问题。可以用命令行登录mysql，并执行以下命令：

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'newrootpassword';
```

## VARCHAR

![varchar](/assets/img/posts/mysql_notes/varchar.PNG)

## CHAR

![char](/assets/img/posts/mysql_notes/char.PNG)

```mysql
CREATE TABLE `char_test` (
  `char_col` char(10) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

```mysql
INSERT INTO char_test (char_col)
VALUES
	('string1'),
	(' string2'),
	('string3 ');
```

```mysql
SELECT
	CONCAT("'", char_col, "'")
FROM
	char_test;
```

![](/assets/img/posts/mysql_notes/output.PNG)

```mysql
CREATE TABLE `varchar_test` (
	`char_col` VARCHAR (10) DEFAULT NULL
) ENGINE = INNODB DEFAULT CHARSET = utf8;
```

```mysql
INSERT INTO varchar_test (char_col)
VALUES
	('string1'),
	(' string2'),
	('string3 ');
```

```mysql
SELECT
	CONCAT("'", char_col, "'")
FROM
	varchar_test;
```

![](/assets/img/posts/mysql_notes/output8.PNG)

![enum](/assets/img/posts/mysql_notes/enum.PNG)

```mysql
CREATE TABLE `enum_test` (
	`e` enum ('fish', 'apple', 'dog') NOT NULL
) ENGINE = INNODB DEFAULT CHARSET = utf8;
```

```mysql
INSERT INTO enum_test (e)
VALUES
	('fish'),
	('dog'),
	('apple');
```

![](/assets/img/posts/mysql_notes/enum1.PNG)

```mysql
SELECT
	e + 0
FROM
	enum_test;
```

![](/assets/img/posts/mysql_notes/output4.PNG)

![](/assets/img/posts/mysql_notes/enum2.PNG)

```mysql
SELECT
	e
FROM
	enum_test
ORDER BY
	e
```

![](/assets/img/posts/mysql_notes/output2.PNG)

![](/assets/img/posts/mysql_notes/enum3.PNG)

```mysql
SELECT
	e
FROM
	enum_test
ORDER BY
	FIELD(e, 'apple', 'dog', 'fish')
```

![](/assets/img/posts/mysql_notes/output3.PNG)

![](/assets/img/posts/mysql_notes/sum.PNG)

```mysql
CREATE TABLE msg_per_hr (
	hr DATETIME NOT NULL,
	cnt INT UNSIGNED NOT NULL,
	PRIMARY KEY (hr)
);
```

![](/assets/img/posts/mysql_notes/sum1.PNG)

```mysql
SELECT
	SUM(cnt)
FROM
	msg_per_hr
WHERE
	hr BETWEEN CONCAT(LEFT(NOW(), 14), '00:00') - INTERVAL 23 HOUR
AND CONCAT(LEFT(NOW(), 14), '00:00') - INTERVAL 1 HOUR
```

![](/assets/img/posts/mysql_notes/sum2.PNG)

![](/assets/img/posts/mysql_notes/output9.PNG)

![](/assets/img/posts/mysql_notes/output10.PNG)

![](/assets/img/posts/mysql_notes/sum3.PNG)

![](/assets/img/posts/mysql_notes/sum4.PNG)

```mysql
CREATE TABLE tbl_shelf_new LIKE tbl_shelf
```

```mysql
INSERT INTO tbl_shelf_new SELECT
	ID,
	CREATE_TIME,
	MODIFY_TIME,
	NAME,
	NO,
	STATUS,
	POINT_NO,
	TYPE,
	REMARAK
FROM
	tbl_shelf
```

```mysql
RENAME TABLE tbl_shelf TO tbl_shelf_old,
 tbl_shelf_new TO tbl_shelf
```

![](/assets/img/posts/mysql_notes/counter.PNG)

```mysql
CREATE TABLE `hit_counter` (
	slot TINYINT UNSIGNED NOT NULL AUTO_INCREMENT,
	cnt INT UNSIGNED NOT NULL DEFAULT '0',
	PRIMARY KEY (`slot`)
) ENGINE = INNODB DEFAULT CHARSET = utf8;
```

```mysql
CREATE PROCEDURE pro()
BEGIN
DECLARE i INT;
SET i = 0;
WHILE i < 100 DO
INSERT INTO `hit_counter` (`cnt`) VALUES ('0');
SET i = i + 1;
END WHILE;
END;
```

```mysql
CALL pro()
```

```mysql
SELECT
	SUM(cnt)
FROM
	hit_counter
```

![](/assets/img/posts/mysql_notes/output11.PNG)

![](/assets/img/posts/mysql_notes/counter1.PNG)

```mysql
CREATE TABLE daily_hit_counter (
	day date NOT NULL,
	slot TINYINT UNSIGNED NOT NULL,
	cnt INT UNSIGNED NOT NULL,
	PRIMARY KEY (DAY, slot)
) ENGINE = INNODB;
```

![](/assets/img/posts/mysql_notes/counter2.PNG)

```mysql
INSERT INTO daily_hit_counter (DAY, slot, cnt)
VALUES
	(CURRENT_DATE, RAND() * 100, 1) ON DUPLICATE KEY UPDATE cnt = cnt + 1;
```

![](/assets/img/posts/mysql_notes/output12.PNG)

![](/assets/img/posts/mysql_notes/counter3.PNG)

```mysql
UPDATE daily_hit_counter AS c
INNER JOIN (
	SELECT
		day,
		SUM(cnt) AS cnt,
		MIN(slot) AS mslot
	FROM
		daily_hit_counter
	GROUP BY
		day
) AS x USING (DAY)
SET c.cnt =
IF (c.slot = x.mslot, x.cnt, 0),
 c.slot =
IF (c.slot = x.mslot, 0, c.slot);
```

![](/assets/img/posts/mysql_notes/output13.PNG)

```mysql
DELETE
FROM
	daily_hit_counter
WHERE
	slot <> 0
AND cnt = 0;
```

![](/assets/img/posts/mysql_notes/output14.PNG)

## 加快ALTER TABLE操作的速度

![](/assets/img/posts/mysql_notes/output15.PNG)

```mysql
ALTER TABLE hit_counter ALTER COLUMN cnt
SET DEFAULT 5
```

