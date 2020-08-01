## 分布式锁 ##
解决分布式系统下，多个请求修改同一个参数锁造成的问题。
## 分布式锁的实现 ##
- **Memcached**：利用 Memcached 的 `add` 命令。此命令是原子性操作，只有在 `key` 不存在的情况下，才能 `add` 成功，也就意味着线程得到了锁。
- **Redis**：和 Memcached 的方式类似，利用 Redis 的 `setnx` 命令。此命令同样是原子性操作，只有在 `key` 不存在的情况下，才能 `set` 成功。
- **Zookeeper**：利用 Zookeeper 的顺序临时节点，来实现分布式锁和等待队列。Zookeeper 设计的初衷，就是为了实现分布式锁服务的。
- **Chubby**：Google 公司实现的粗粒度分布式锁服务，底层利用了 Paxos 一致性算法。

## Redis实现分布式锁 ##
### 加锁 ###
使用SETNX可以实现一个简单的分布式锁：
SETNX命令是[SET if Not eXists]，如果key不存在，才会设置，使用方法:
```
redis> exists lock_key
0
redis> setnx lock_key 'client_a'
1
redis> setnx lock_key 'client_a'
0
redis> setnx lock_key 'client_b'
0
redis> get lock_key
client_a
```
但是SETNX有一个缺陷，不能设置过期时间，如果加锁的客户端宕机了，就会导致无法解锁，造成死锁。
可以使用EXPIRE命令设置过期时间。
但是这种方法不具备原子性，如果客户端还没设置过期时间就宕机了，同样造成死锁。

Redis 2.6.12 更新了SET命令，可以通过带上NX,EX命令原子执行加锁操作。
- EX second:设置key的过期时间，单位为秒
- NX :当key不存在时，进行set操作，等同于setnx
代码：
```
SET lock_key unique_value NX EX lock_time
```

### 解锁 ###
用DEL删除锁：
```
DEL lock_key
```
但是这种方式存在缺陷：
假设 A 获取了锁，过期时间 30s，此时 35s 之后，锁已经自动释放了，A 去释放锁，但是此时可能 B 获取了锁。A 就会释放 B 的锁。

这就需要加锁时给锁设置一个唯一值unique_value，解锁时，验证unique_value是否与加锁的一致才能解锁。
```
value = get lock_key
if(value == unique_value){
	DEL lock_key
}else{

}
```
上述代码不具备原子性，无法保证程序原子执行。
需要使用lua脚本执行。
```
EVAL script numkeys key [key ...] arg [arg ...]
```
`numkeys` 参数用于键名参数，即后面 `key` 数组的个数。
`key [key ...]` 代表需要在脚本中用到的所有 Redis key，在 Lua 脚本使用使用数组的方式访问 key，类似如下 `KEYS[1]` ， `KEYS[2]`。注意 Lua 数组起始位置与 Java 不同，Lua 数组是从 1 开始。
命令最后，是一些附加参数，可以用来当做 Redis Key 值存储的 Value 值，使用方式如 `KEYS` 变量一样，类似如下：`ARGV[1]` 、 `ARGV[2]` 。
```
if redis.call("get", KEYS[1]) == ARGV[1] then 
	return redis.call("del",KEYS[1])
else
	return 0
end
```

## Redisson ##
Redisson 是一个企业级的开源 Redis Client，也提供了分布式锁的支持。
```
Config config = new Config();
config.useClusterServers()
.addNodeAddress("redis://192.168.31.101:7001")
.addNodeAddress("redis://192.168.31.101:7002")
.addNodeAddress("redis://192.168.31.101:7003")
.addNodeAddress("redis://192.168.31.102:7001")
.addNodeAddress("redis://192.168.31.102:7002")
.addNodeAddress("redis://192.168.31.102:7003");

RedissonClient redisson = Redisson.create(config);


RLock lock = redisson.getLock("anyLock");
lock.lock();
lock.unlock();
```
我们只需要通过它的 API 中的 Lock 和 Unlock 即可完成分布式锁，他帮我们考虑了很多细节：
- Redisson 所有指令都通过 Lua 脚本执行，Redis 支持 Lua 脚本原子性执行。
- Redisson 设置一个 Key 的默认过期时间为 30s，如果某个客户端持有一个锁超过了 30s 怎么办？Redisson 中有一个 Watchdog 的概念，翻译过来就是看门狗，它会在你获取锁之后，每隔 10s 帮你把 Key 的超时时间设为 30s。这样的话，就算一直持有锁也不会出现 Key 过期了，其他线程获取到锁的问题了。
- Redisson 的“看门狗”逻辑保证了没有死锁发生。（如果机器宕机了，看门狗也就没了。此时就不会延长 Key 的过期时间，到了 30s 之后就会自动过期了，其他线程可以获取到锁）

另外，Redisson 还提供了对 Redlock 算法的支持，它的用法也很简单：
```
RedissonClient redisson = Redisson.create(config);
RLock lock1 = redisson.getFairLock("lock1");
RLock lock2 = redisson.getFairLock("lock2");
RLock lock3 = redisson.getFairLock("lock3");
RedissonRedLock multiLock = new RedissonRedLock(lock1, lock2, lock3);
multiLock.lock();
multiLock.unlock();
```

参考文章：
[https://www.jianshu.com/p/a1ebab8ce78a](https://www.jianshu.com/p/a1ebab8ce78a "什么是分布式锁")
[http://dockone.io/article/10456](http://dockone.io/article/10456 "分布式锁用Redis还是Zookeeper？")
手撸分布式锁