# 鹿游器| MC服务器页端注册插件


## 说明

使用Node.js驱动程序的Minecraft网页注册系统，基于Github-hhui64的Regx发行版本开发


## 安装&启动

使用 cd 命令切换到项目根目录，执行命令 `npm install` 来安装项目的依赖包

用编辑器打开 `config/config.json` 并填写数据库等信息

执行命令 `npm run start` 启动服务端，会在控制台看到一行提示 __"Server is running on host XXXX"__

访问 `127.0.0.1:80` 就会出现页面了(阿里云，腾讯云，华为云，AWS，GCP，Digital Ocean，Linode等VPS需要提前在服务器安全组内开放对应端口)


## 功能&支持

* 兼容 AuthMe Reloaded 5.4.0
* 检测输入内容并提交后端验证
* 腾讯点击式防灌水验证码
* 发送邮件验证码验证邮箱
* 注册昵称白名单/黑名单
* 注册邮箱白名单/黑名单
* 注册IP归属地白名单/黑名单
* 相同IP间隔N分钟后才能注册
* 支持 Authme 插件的加密算法


## 反向代理
### 推荐使用Nginx服务器来进行反向代理

在 `nginx.conf` 的 `http` 中添加一个 `server` 节点，示例如下：

```
http {
  # regx 反向代理节点
  server {
      listen        80;
      server_name   www.yourdomain.com;

      location / {
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header Host  $http_host;
          proxy_set_header X-Nginx-Proxy true;
          proxy_set_header Connection "";
          proxy_pass http://127.0.0.1:80;
          proxy_intercept_errors on;
      }
      error_page   404  /404.html;
      error_page   500 502 503 504  /50x.html;
      location = /50x.html {
          root   html;
      }
  }
  # 其它 server 节点...
}
```

`listen 80;` 为监听 80 端口(即 http 协议)，如果开启 SSL (即 https 协议)那么请修改为 `listen 443;` ，并添加相应的 SSL 配置代码，具体代码就不在此贴出，一般证书提供商都会有相应的配置代码

`server_name www.yourdomain.com;` 为绑定域名，将其修改为自定义域名

`proxy_pass http://127.0.0.1:80;` 后面的 `:80` 为端口号，可根据站点的配置修改

添加完成后，保存配置文件，然后重启 Nginx

以上面为例子的话，此时访问 __www.yourdomain.com__ 即可显示MC注册页面

__注意:__ 推荐开启 SSL ，此项目支持全站 HTTPS
