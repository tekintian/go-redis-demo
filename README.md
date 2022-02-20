# go-redis-demo

redis操作使用的类库为 go-redis/redis/v8


go redis demo
~~~go
   rdb := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "", // no password set
		DB:       0,                               // use default DB
	})

	err := rdb.Set(ctx, "key-after-30-sec", "hello world, redis go client!", 30*time.Second).Err()
	if err != nil {
		panic(err)
	}

	val, err := rdb.Get(ctx, "key-after-30-sec").Result()
	if err != nil {
		panic(err)
	}
	fmt.Println("key-after-30-sec", val)
	// 这里的过期时间为纳秒所以这里需要直接使用 数字 * 时间单位常量, 如: 10分钟过期,则写成  10 * time.Minute
	err = rdb.Set(ctx, "key-after-10min", "Go Redis client key-after-10min", 10*time.Minute).Err()
	if err != nil {
		fmt.Println("redis set key-after-10min error", err)
	}

	err = rdb.Set(ctx, "key-never-expired", "Go Redis client key-never-expired", 0).Err()
	if err != nil {
		fmt.Println("redis set key-never-expired error", err)
	}

	val2, err := rdb.Get(ctx, "key-after-10min").Result()
	if err == redis.Nil {
		fmt.Println("key-after-10min does not exist")
	} else if err != nil {
		panic(err)
	} else {
		fmt.Println("key-after-10min", val2)
	}
~~~
