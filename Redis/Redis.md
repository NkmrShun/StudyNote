# Redis #
## 什么是Redis ##
Redis是一个开源的，基于内存的数据结构存储，可用作于数据库、缓存、消息中间件。
- Redis是基于内存，支持多种数据结构
- Redis常用作于缓存

Redis由C语言编写

Redis的存储是以key-value的形式的。Redis中的key一定是字符串，value可以是string、list、hash、set、sortset这几种常用的。

Redis并没有直接使用这些数据结构来实现key-value数据库，而是基于这些数据结构创建了一个对象系统
- 简单来说：Redis使用对象来表示数据库中的键和值。每次我们在Redis数据库中新创建一个键值对时，至少会创建出两个对象。一个是键对象，一个是值对象。