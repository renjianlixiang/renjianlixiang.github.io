---
title: Redis工具类
date: 2023/07/22 10:24:00
updated: 2023/07/22 10:24:00
tags: 
- Redis
- Redis工具类
- Spring Boot
categories: 
- Spring Boot
- Redis工具类
---

Spring Boot项目中的RedisUtils封装。

<!--more-->

## RedisCofig

Redis序列化配置文件：

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * Redis 配置
 */
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory);

        //key采用string的序列化方式
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //hash的key采用string的序列化方式
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        //value序列化方式采用jackson
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        //hash的value序列化方式采用jackson
        redisTemplate.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());

        redisTemplate.afterPropertiesSet();

        return redisTemplate;
    }

    @Bean
    public RedisTemplate<String, Object> getRedisUtils(RedisTemplate<String, Object> redisTemplate) {
        RedisUtils.setRedisTemplate(redisTemplate);
        return redisTemplate;
    }
    
}
```

## RedisUtils工具类封装

RedisUtils工具类：

```java
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.util.Map;
import java.util.concurrent.TimeUnit;

/**
 * Redis 工具类
 */
@Component
public class RedisUtils {

    public static RedisTemplate<String, Object> redisTemplate;
  

    public static void setRedisTemplate(RedisTemplate<String, Object> redisTemplate) {
        RedisUtils.redisTemplate = redisTemplate;
    }

    /**
     * 设置key对应的value的offset位的bit值
     * value为true, 设置为1; value为false, 设置为0
     * @param key
     * @param offset
     * @param value
     * @return
     */
    public static boolean setBit(String key, Long offset, Boolean value) {
        return Boolean.TRUE.equals(redisTemplate.opsForValue().setBit(key, offset, value));
    }

    /**
     * 获取key对应的value的offset位的bit值
     * @param key
     * @param offset
     * @return
     */
    public static boolean getBit(String key, Long offset) {
        return Boolean.TRUE.equals(redisTemplate.opsForValue().getBit(key, offset));
    }

    /**
     * 设置key-value,
     * 若已存在相同的key, 那么原来的key-value会被丢弃
     * @param key
     * @param value
     * @return
     */
    public static void set(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }

    /**
     * 设置key-value并设置时效时间,
     * 若已存在相同的key, 那么原来的key-value会被丢弃
     * @param key
     * @param value
     * @param expireTime
     * @param unit
     * @return
     */
    public static void setExpire(String key, Object value, Long expireTime, TimeUnit unit) {
        redisTemplate.opsForValue().set(key, value, expireTime, unit);
    }

    /**
     * 设置key-value,
     * 若不存在key时, 向redis中添加key-value, 返回true/false, 若存在, 则不做任何操作, 返回false
     * @param key
     * @param value
     * @return
     */
    public static boolean setIfAbsent(String key, Object value) {
        return Boolean.TRUE.equals(redisTemplate.opsForValue().setIfAbsent(key, value));
    }

    /**
     * 设置key-value并设置时效时间,
     * 若不存在key时, 向redis中添加key-value, 返回true/false, 若存在，则不做任何操作, 返回false
     * @param key
     * @param value
     * @param expireTime
     * @param unit
     * @return
     */
    public static boolean setIfAbsent(String key, Object value, Long expireTime, TimeUnit unit) {
        return Boolean.TRUE.equals(redisTemplate.opsForValue().setIfAbsent(key, value, expireTime, unit));
    }

    /**
     * 设置key-value, 并返回旧的value,
     * 若不存在key, 返回的旧值为null
     * @param key
     * @param newValue
     * @return
     */
    public static Object getAndSet(String key, Object newValue) {
        return redisTemplate.opsForValue().getAndSet(key, newValue);
    }

    /**
     * 批量设置key-value,
     * 若存在相同的key, 则原来的key-value会被丢弃。
     * @param map
     * @return
     */
    public static void multiSet(Map<String, Object> map) {
        redisTemplate.opsForValue().multiSet(map);
    }

    /**
     * 判断是否存在key-value
     * @param key
     * @return
     */
    public static boolean exists(final String key) {
        return Boolean.TRUE.equals(redisTemplate.hasKey(key));
    }

    /**
     * 获取key-value
     * @param key
     * @return
     */
    public static Object get(String key) {
        return redisTemplate.opsForValue().get(key);
    }



    /**
     * 向key对应的hash中设置hashKey-value,
     * 同一个hash里面, 若已存在相同的hashKey, 那么此操作将丢弃原来的hashKey-value, 使用新的hashKey-value
     * @param key
     * @param hashKey
     * @param value
     * @return
     */
    public static void hashPut(String key, Object hashKey, Object value) {
        redisTemplate.opsForHash().put(key, hashKey, value);
    }


    /**
     * 向key对应的hash中批量设置hashKey-value,
     * 同一个hash里面，若已存在相同的hashKey, 那么此操作将丢弃原来的hashKey-value, 而使用新的hashKey-value
     * @param key
     * @param maps
     * @return
     */
    public static void hashPutAll(String key, Map<Object, Object> maps) {
        redisTemplate.opsForHash().putAll(key, maps);
    }

    /**
     * 向key对应的hash中设置hashKey-value,
     * 当key对应的hash中, 不存在hashKey时, 才向key对应的hash中设置hashKey-value, 返回true/false, 否则, 不进行任何操作, 返回false
     * @param key
     * @param hashKey
     * @param value
     * @return
     */
    public static boolean hashPutIfAbsent(String key, Object hashKey, Object value) {
        return redisTemplate.opsForHash().putIfAbsent(key, hashKey, value);
    }

    /**
     * 获取到key对应的hash
     * 若中不存在对应的key, 则返回一个没有任何entry的空的Map
     * @param key
     * @return
     */
    public static Map<Object, Object> hGetAll(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * 获取key对应的hash里面的hashKey-value,
     * 若不存在对应的key, 则返回null, 若key对应的hash中不存在对应的hashKey, 也会返回null
     * @param key
     * @param hashKey
     * @return
     */
    public static Object hashGet(String key, Object hashKey) {
        return redisTemplate.opsForHash().get(key, hashKey);
    }
}
```