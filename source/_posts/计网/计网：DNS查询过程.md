迭代+递归

1. 在DNS解析器缓存中查找某主机的ip地址
2. 向其本地域名服务器进行递归查找。
3. 本地域名服务器查不到，就向根域名服务器进行迭代查询。
4. 根域名服务器告诉本地域名服务器，下一步该向顶级域名服务器查找。
5. 本地域名服务器向顶级域名服务器查找。
6. 顶级域名服务器告诉本地域名服务器，下一步该向权限域名服务器查找。
7. 权限域名服务器返回目标主机的ip地址。
8. 本地域名服务器就把ip地址告诉目标主机。

