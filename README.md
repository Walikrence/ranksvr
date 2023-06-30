# 基于golang和redis的排行榜服务实现

计划使用redisgo和zset实现排行榜



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
zrevrange rank 0 10 
# 查看榜rank的 反向0-10排名的成员带上分数
zrevrange rank 0 10 withscores

# 删除zset
del rank
```

## 关于redigo
[redigo](https://github.com/gomodule/redigo)

安装
go.mod
```sh
module ranksvr

go 1.18

require github.com/gomodule/redigo/redis v0.0.1

```

```sh
go mod tidy
```

TODO

## 关于redis部署

使用docker运行容器redis容器和redis容器，连接同一个网络
```sh
docker run -itd --name=ranksvr -v /c/ranksvr:/root -p 8096:22 --shm-size=3096m --privileged --network=redisnetwork my_fmgame
docker run --name redis-container --network redisnetwork -d redis
```

在ranksvr容器中使用redis-cli连接redis容器
```sh
redis-cli -h redis-container
```

## 关于服务器接口设计

基于redis和 http/proto 请求的排行榜设计

支持增删改查

1. 新增/更新 玩家分数
2. 查询玩家分数
3. 查询玩家排名
4. 查询前五玩家
5. 从榜上删除玩家数据
6. 清空排行榜数据

```proto
// 新增/更新 玩家分数
message UpdatePlayerRankInfoReq
{
  int64 roleid = 1;
  int32 score  = 2;
}

message UpdatePlayerRankInfoRes
{
  int32 RetCode  = 1;
}


//玩家信息
message PlayerInfo
{
	int64 roleid   = 1;
  	int32 score    = 2;
  	int32 rank     = 3;
}


// 查询玩家分数
message QueryPlayerScoreReq
{
  int64 roleid = 1;
}
message QueryPlayerScoreRes
{
  PlayerInfo info   = 1;
  int32 RetCode  	= 2;
}

// 查询玩家排名
message QueryPlayerRankReq
{
  int64 roleid = 1;
}
message QueryPlayerRankRes
{
  PlayerInfo info   = 1;
  int32 RetCode  	= 2;
}

// 查询前五玩家
message QueryTop5RankReq
{}
message QueryTop5RankRes
{
  repeated PlayerInfo   = 1;
  int32 RetCode 		= 2;
}

// 从榜上删除玩家数据
message DeletePlayerRankReq
{
	int64 roleid = 1;
}
message DeletePlayerRankRes
{
  int32 RetCode 	= 1;
}

// 清空排行榜数据
message ClearRankInof
{}
message ClearRankInof
{
  int32 RetCode 	= 1;
}

```
## 关于服务器实现
TODO
## 关于客户端设计
TODO
## 关于测试