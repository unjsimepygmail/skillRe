1.如何使用redis：传感器写库之前，存入redis，接口直接去查redis。
2.使用kafka干啥?通过socket长连接获取的传感器数据，直接丢入kafka中，随后监听sensor topic 设置消费者组id，一次拉取
