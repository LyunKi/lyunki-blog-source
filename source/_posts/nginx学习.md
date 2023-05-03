---
title: Nginx学习
---

记录一些常用的 Nginx 知识 <!-- more -->

## Nginx 不同网站如何维护

在 Nginx 中，通常可以将不同网站的配置和静态文件存放在不同的目录下，这样可以使得 Nginx 配置更加清晰和易于维护。以下是一些常见的最佳实践：

1. 将不同网站的配置文件存放在 `/etc/nginx/conf.d/` 目录下。可以为每个网站创建一个单独的配置文件，例如 `example.com.conf`、`blog.example.com.conf` 等，这样可以使得每个网站的配置更加独立和清晰。

2. 将不同网站的静态文件存放在 `/var/www/` 目录下。可以为每个网站创建一个单独的目录，例如 `/var/www/example.com/`、`/var/www/blog.example.com/` 等，将该网站的静态文件和资源放置在该目录下。

3. 在配置文件中使用 `include` 指令，将不同网站的配置文件和静态文件进行分离。例如，在 `/etc/nginx/nginx.conf` 文件中，可以使用以下指令引入所有的网站配置文件：

   ```
   http {
       # ...
       include /etc/nginx/conf.d/*.conf;
   }
   ```

   在每个网站的配置文件中，可以使用 `root` 指令设置该网站的静态文件目录，例如：

   ```
   server {
       listen 80;
       server_name example.com;
       root /var/www/example.com;

       # ...
   }
   ```

   这样可以将每个网站的静态文件和资源与其配置文件进行分离，并使得配置文件更加简洁和易于维护。

总之，最佳实践是将不同网站的配置文件和静态文件进行分离，以提高配置的可读性和可维护性。可以根据具体情况选择存放目录和文件命名方式，但应保持清晰和规范。