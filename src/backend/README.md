# 毕昇后端代码

* Dockerfile 使用 poetry 进行 Python 依赖管理

# 2025-07-30 新增is_local字段，用于标识消息是否本地存储

```
ALTER TABLE `bisheng`.`chatmessage` 
ADD COLUMN `is_local` tinyint(1) UNSIGNED NULL DEFAULT 0 FIRST;
```
