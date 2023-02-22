# redis-cluster

## 如何配置
1. 需修改 redis.conf 启用 cluster 模式
2. 启动时需将修改的 redis.conf 挂到 container 中
3. 启动后，使用 redis-cli -h ${redis-node1-hostname} -p ${redis-node1-port} cluster info 可以看到相关信息
注意: 这时候的 redis network 需使用 host mode，否则 nodes 之间的通信会失败。
### 如何启动
1. 增加 node: 
redis-cli -h ${redis-node1-hostname} -p ${redis-node1-port} cluster meet ${redis-node2-hostname} ${redis-node2-port}
2. 分配插槽 (0-16383): 
    - redis-cli -h ${redis-node1-hostname} -p ${redis-node1-port}  cluster addslots {0..8191}
    - redis-cli -h ${redis-node1-hostname} -p ${redis-node2-port}  cluster addslots {8192..16383}
3. 加入 slave 节点到主节点:
    - 进入到 slave 节点中
    - cluster replicate ${redis-master-node-id}
    ![alt redis-node-id 示意图](node-id.png
    )
4. 当其中的某个master 节点离线后，如果没有自动将 slave 变成 master，则今入该 slave 然后使用  cluster failover takeover