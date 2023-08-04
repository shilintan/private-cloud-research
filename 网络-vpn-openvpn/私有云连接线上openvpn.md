# 安装

```
apt install -y openvpn
```



```
mv robot-private-cloud.ovpn /etc/openvpn/client/robot-private-cloud.conf
systemctl daemon-reload
systemctl start openvpn-client@robot-private-cloud
systemctl enable openvpn-client@robot-private-cloud
systemctl status openvpn-client@robot-private-cloud

journalctl -xe openvpn-client@robot-private-cloud
```

