
# RSSHUB

rsshub.tczhong.com



# Wechat

in raspberrypi /home/tczhong/Documents/wechatrss


```
sudo docker run -d \
  --name wewe-rss \
  -p 4000:4000 \
  -e DATABASE_TYPE=sqlite \
  -e AUTH_CODE=123567 \
  -v $(pwd)/data:/app/data \
  cooderl/wewe-rss-sqlite:latest
```

https://wechatrss.tczhong.com/feeds/MP_WXS_3086720555.atom

https://wechatrss.tczhong.com/dash

