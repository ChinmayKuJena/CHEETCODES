# Redis Practice Documentation: TTL and Pub/Sub

This document guides you through the basic operations of **TTL (Time-to-Live)** and **Pub/Sub (Publish/Subscribe)** in Redis using the command-line interface (CLI). The operations will help you understand how to set expiration times for keys and implement a messaging system using Redis.

## Prerequisites

Before starting, ensure that you have Redis installed and running. Follow these steps to pull and run Redis in a Docker container:

### 1. Pull Redis Image:
```bash
docker pull redis
```

### 2. Run Redis Container:
```bash
docker run --name redis-container -d -p 6379:6379 redis
```

### 3. Connect to Redis Container:
```bash
docker exec -it redis-container redis-cli
```

### 4. Stop and Remove Redis Container:
```bash
docker stop redis-container
docker rm redis-container
```

## Basic Redis Commands

### Setting and Retrieving Keys:
```bash
SET key value
GET key
```

### Listing Keys:
```bash
KEYS *
```

### Deleting a Key:
```bash
DEL key
```

### Checking if a Key Exists:
```bash
EXISTS key
```

---

## Working with Numbers in Redis

### Incrementing and Decrementing Keys:
```bash
INCR counter       # Increments the value by 1
DECR counter       # Decrements the value by 1
INCRBY counter 5  # Increments by 5
DECRBY counter 2  # Decrements by 2
```

---

## Working with Strings in Redis

### Appending to Keys:
```bash
APPEND key value
```

### Getting the Length of a String:
```bash
STRLEN key
```

---

## Working with Lists in Redis

### Operations on Lists:
```bash
LPUSH list_name value    # Adds a value to the start (left) of a list
RPUSH list_name value    # Adds a value to the end (right) of a list
LPOP list_name           # Removes and returns the first element of the list
RPOP list_name           # Removes and returns the last element of the list
LRANGE list_name start stop  # Gets a range of elements from the list
```

Example:
```bash
RPUSH mylist "apple"
RPUSH mylist "banana"
LPUSH mylist "orange"
LRANGE mylist 0 2  # Returns ["orange", "apple", "banana"]
```

---

## Working with Sets in Redis

### Operations on Sets:
```bash
SADD myset value      # Adds one or more members to a set
SMEMBERS myset        # Gets all the members of a set
SISMEMBER myset value # Checks if a member exists in a set
SREM myset value      # Removes a member from a set
```

Example:
```bash
SADD myset "apple"
SADD myset "banana"
SMEMBERS myset       # Returns ["apple", "banana"]
SISMEMBER myset "apple"   # Returns (integer) 1
SREM myset "banana"       # Removes "banana"
```

---

## Working with Hashes in Redis

### Operations on Hashes:
```bash
HSET hash_name field value    # Sets a field in a hash
HGET hash_name field          # Gets the value of a field in a hash
HGETALL hash_name             # Gets all fields and values in a hash
HDEL hash_name field          # Deletes a field from a hash
```

Example:
```bash
HSET user:1 name "John"
HSET user:1 age "30"
HGETALL user:1   # Returns ["name", "John", "age", "30"]
HDEL user:1 age  # Removes the "age" field
```

---

## TTL (Time-to-Live) in Redis

TTL allows setting an expiration time for keys. When the TTL expires, the key is automatically deleted.

### Commands to Practice:

#### Setting TTL:
```bash
SETEX mykey 10 "This key will expire in 10 seconds"
```

#### Checking TTL:
```bash
TTL mykey       # Returns remaining TTL in seconds
PTTL mykey      # Returns remaining TTL in milliseconds
```

#### Setting TTL for Existing Key:
```bash
EXPIRE mykey 5  # Set TTL to 5 seconds
```

#### Removing TTL (Making the Key Persistent):
```bash
PERSIST mykey   # Removes expiration time from the key
```

### Example Scenario:

1. Set a key with an expiration time of 10 seconds:
```bash
SETEX session 10 "user-session-data"
```

2. Check the TTL of the key:
```bash
TTL session
```

3. Wait a few seconds, then check the TTL again:
```bash
TTL session
```

4. After the TTL expires, try to get the value of the key:
```bash
GET session
```

5. Remove the TTL and make the key persistent:
```bash
PERSIST session
```

6. Check the TTL again:
```bash
TTL session
```

---

## Pub/Sub in Redis

Redis supports a **Publish/Subscribe (Pub/Sub)** messaging pattern where publishers send messages to channels, and subscribers listen to those channels. Redis handles the delivery of messages to all subscribers.

### Pub/Sub Commands:

#### Subscribing to a Channel:
```bash
SUBSCRIBE channel1
```

#### Publishing to a Channel:
```bash
PUBLISH channel1 "Hello, Redis!"
```

#### Unsubscribing from a Channel:
```bash
UNSUBSCRIBE channel1
```

### Example Scenario:

1. **Subscriber** (in one terminal window):
```bash
SUBSCRIBE news
```
This command waits for messages to be published to the `news` channel. You will see:
```bash
1) "subscribe"
2) "news"
3) (integer) 1
```

2. **Publisher** (in another terminal window):
```bash
PUBLISH news "Breaking News: Redis is awesome!"
```
The message will be sent to the `news` channel, and in the subscriber terminal, you will see:
```bash
1) "message"
2) "news"
3) "Breaking News: Redis is awesome!"
```

3. Publish another message:
```bash
PUBLISH news "More updates: Pub/Sub is easy to use!"
```
The subscriber terminal will receive:
```bash
1) "message"
2) "news"
3) "More updates: Pub/Sub is easy to use!"
```

4. **Unsubscribe** from the channel:
```bash
UNSUBSCRIBE news
```
The subscriber will stop receiving messages and will see:
```bash
1) "unsubscribe"
2) "news"
3) (integer) 0
```

---

## Conclusion

This document introduced key Redis features, including TTL for automatic key expiration and Pub/Sub for message passing. Practicing these commands will help you get familiar with Redis, and they are useful for building fast, scalable systems for caching, message queues, and more.
