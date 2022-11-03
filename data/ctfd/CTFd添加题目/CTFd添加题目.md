# Misc，Crypto类型的题目

![截图](img/f1962ca6ac28f509c95e8acefc11dff7.png)

![截图](img/34732d11dd5593b029420f74dd6ec509.png)

![截图](img/444c60c07274365695ecdc795e35f3f3.png)

# Web动态flag

![截图](img/4c4a6e04869564cfb7adfe4a5c379d0f.png)

### 配置docker-compose.yml

```sh
version: "2"
services:

  web:
    build: .
    restart: always
    environment:
      - FLAG=flag{This_is_s0_simpl3}
```

![截图](img/9fa7009e478070a1e07e9d0afc29c5ab.png)

### 配置Dockerfile

```sh
FROM ctftraining/base_image_nginx_mysql_php_73

LABEL Author="jchan <jchan-l@qq.com>"

COPY ./files /tmp/
RUN cp -rf /tmp/html /var/www/ \
    && cp -f /tmp/flag.sh /flag.sh \
    && chown -R www-data:www-data /var/www/html \
```

![截图](img/0bd6221c7b3ebd81aedea88fd7d633aa.png)

### flag.sh、docker-compose.yml和index.php里面的flag要一样才能实现动态flag

```sh
sed -i "s/flag{This_is_s0_simpl3}/$FLAG/" /var/www/html/index.php
export FLAG=not_flag
FLAG=not_flag

rm -f /flag.sh
```

**注意：flag在哪个文件，flag.sh第一行的路径就要写flag这个文件的路径**

## 构建容器

```sh
docker build -t whereisflag .
# -t 镜像名字 
```

![截图](img/d2b14f0c42dd127f9747a2aea4f61cad.png)

## 登录Dockerhub

```sh
docker login
```

![截图](img/04cc95daebe4c72d28f830afda753799.png)

## 推送镜像到Dockerhub

![截图](img/241e4bd064a709ee3cb8215e5f6551b5.png)

![截图](img/c0fc8454e649522e3e3489cbb80c59c5.png)

## 配置CTFd

![截图](img/1163709a5d72f087ed7f0ed9b8108f03.png)

![截图](img/f4cd971eb526288ecc7f1400b3bb7575.png)

#### 不需要填写flag

![截图](img/09fb0ea46968d1d432f8058490401361.png)
