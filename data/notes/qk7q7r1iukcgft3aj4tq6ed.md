
# Xiaoaigpt

使用xiaoai 和gpt相结合做的。

## source code

https://github.com/cczhong11/xiaogpt.git

## set up

1. use docker
2. `sudo docker build -t tczhong24/xiaogpt .`
3. `sudo docker run tczhong24/xiaogpt --config=/app/xiaogpt.config --use_chatgpt_api --use_command`

## rerun every 20 minutes script

```bash
#!/bin/bash
CONTAINER_NAME="6639477c3c3c"

sudo docker stop $CONTAINER_NAME
sudo docker start $CONTAINER_NAME
```

add to sudo crontab