## Monitor
To show what is happening in the database use `monitor` command. It streams back every command processed by the Redis server

```shell
redis-cli monitor
```

If auth is enabled use: 
```shell
redis-cli -a 'your-password' monitor
```
