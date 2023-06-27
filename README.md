# 基于golang和redis的排行榜服务实现

计划使用redisgo和zset实现排行榜

TODO

## 关于zset

```sh
# 向榜rank 添加成员walikrence 分数为 1
zadd rank 1 walikrence
# 向榜rank 添加成员zhangsan 分数为 2
zadd rank 2 zhangsan
# 向榜rank 添加成员lisi 分数为 3
zadd rank 3 lisi
# 向榜rank 添加成员wangwu 分数为 4
zadd rank 4 wangwu
# 向榜rank 添加成员walikrence 分数为 100  （已经存在，更改分数为100）
zadd ranke 100 walikrence

# 查看榜rank 中walikrence的分数
zscore rank walikrence
# 查看榜rank中walikrence的排名
zrank rank walikrence
# 查看榜rank中walikrence的反向排名
zrevrank rank walikrence

# 查看榜rank中的成员数量
zcard rank

# 查看榜rank的 0-10排名的成员
zrange rank 0 10 
# 查看榜rank的 0-10排名的成员带上分数
zrange rank 0 10 withscores

# 查看榜rank的 反向0-10排名的成员
zrevrank rank 0 10 
# 查看榜rank的 反向0-10排名的成员带上分数
zrevrank rank 0 10 withscores

```