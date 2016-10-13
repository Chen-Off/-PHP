https://my.oschina.net/jsan/blog/270161/
---
INSERT INTO table(field1, field2, fieldn) SELECT 'field1', 'field2', 'fieldn' FROM DUAL WHERE NOT EXISTS(SELECT field FROM table WHERE field = ?)
