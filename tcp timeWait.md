```markdown

tcp wait 是刚断开的连接
tcp wait占用端口, 总端口数为net.ipv4.ip_local_port_range右值-左值, 默认3w个
通过重用连接可以减少产生, 通过回收加快增加
```