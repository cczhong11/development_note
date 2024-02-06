
dropbox的auth 流程比较麻烦。

1. https://www.dropbox.com/oauth2/authorize?client_id=[clint_id]&response_type=code&token_access_type=offline 拿到access token
2. https://api.dropboxapi.com/oauth2/token 拿到refresh token
3. 在python client里用app key ， app secret， refresh token 拿到access token