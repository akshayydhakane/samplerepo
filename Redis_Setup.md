# Redis in ubuntu

## Installing and Configure Redis in ubuntu

```bash
# sudo apt update
```

```bash
# sudo apt install redis-server
```

```bash
# sudo nano /etc/redis/redis.conf
```

```bash
. . .

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd

. . .
```

And change the password for authentication so

```bash
# requirepass foobared 
```

replace foobared with your new password  

```bash
# requirepass NewPassword
```

Then save and exit

After that you have to restart redis service

```bash
# sudo systemctl restart redis
```

For check redis service start, stop, restart and status

```bash
# sudo systemctl start redis
```

```bash
# sudo systemctl status redis
```

```bash
# sudo systemctl stop redis
```

```bash
# sudo systemctl restart redis
```

Then Enter,

```bash
# redis-cli
```

OUTPUT look like this

```bash
#127.0.0.1:6379>
```

Now you Authenticate your redis password

```bash
#127.0.0.1:6379> auth your_redis_password
```

```bash
Output
OK
```
