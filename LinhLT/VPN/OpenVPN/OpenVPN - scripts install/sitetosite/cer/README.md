#README
Hướng dẫn sử dụng scripts cài đặt OpenVPN mô hình site to site dùng **certificate** trên nền tảng ubuntu 14.04
#1. Mô hình:

![](https://camo.githubusercontent.com/57b1fbe2d551e35e857474590a1d827a10d5f25b/687474703a2f2f692e696d6775722e636f6d2f737a56726c35332e6a7067)

- Site A (server):
	- ip public: 10.10.40.131
	- ip lan private: 10.10.10.0/24
- Site B (client):
	- ip public: 10.10.40.132
	- ip lan private: 10.10.20.0/24

#2. Thiết lập các thông số trên cả 2 site
- Tải file `config.cfg`, thay đổi các thông số phù hợp với mô hình của bạn.
- Chú ý, các bạn phải tải file này về ở cả 2 site và phải cấu hình trên từng site. Nội dung cấu hình trên các site là hoàn toàn khác nhau.

```sh
cd /root/
wget https://raw.githubusercontent.com/lethanhlinh247/networking-team/master/LinhLT/VPN/OpenVPN/OpenVPN%20-%20scripts%20install/sitetosite/cer/config.cfg
```
- Ví dụ trên site A (server) tôi cấu hình như thế này: 
```sh
#########
ip_public_local=10.10.40.131
ip_public_remote=10.10.40.132
port=1194
ip_tunnel_local=10.0.0.1
ip_tunnel_remote=10.0.0.2
dir_key=/etc/openvpn/easy-rsa
net_lan_remote=10.10.20.0
mask_lan_remote=255.255.255.0
user=root       #user for login ssh on site remote.
dir_log=/var/log/openvpn
dir_openvpn=/etc/openvpn
```

- Và trên site B (client) tôi cấu hình như thế này:
```sh
#########
ip_public_local=10.10.40.132
ip_public_remote=10.10.40.131
port=1194
ip_tunnel_local=10.0.0.2
ip_tunnel_remote=10.0.0.1
dir_key=/etc/openvpn/easy-rsa
net_lan_remote=10.10.10.0
mask_lan_remote=255.255.255.0
user=root       #user for login ssh on site remote.
dir_log=/var/log/openvpn
dir_openvpn=/etc/openvpn
```

#3. Chạy trên site B, site client.
- Tải file `client.sh` về, phân quyền cho phép thực thi file và chạy file.
- Lưu ý, file `config.cfg` phải nằm cùng thư mục với file `client.sh`.

```sh
cd /root/
wget https://raw.githubusercontent.com/lethanhlinh247/networking-team/master/LinhLT/VPN/OpenVPN/OpenVPN%20-%20scripts%20install/sitetosite/cer/client.sh
chmod u+x client.sh
./client.sh
```

#4. Trên site A, site server.
- Tải file `server.sh` về,  phần quyền cho phép thực thi file và chạy file.
- Lưu ý, trong quá trình chạy scripts, sẽ có yêu cầu thông báo nhập mật khẩu ssh để truy cập đến site B từ site A, phục vụ cho mục đích chép file key vpn từ site A sang site B. Nếu các bạn đã cấu hình ssh trên site A và site B bằng cách sử dụng file key thì không cần phải nhập mật khẩu.

```sh
cd /root/
wget https://raw.githubusercontent.com/lethanhlinh247/networking-team/master/LinhLT/VPN/OpenVPN/OpenVPN%20-%20scripts%20install/sitetosite/cer/server.sh
chmod u+x server.sh
./server.sh
```

#5. Trên site B.
Tiến hành khởi động lại dịch vụ openvpn
```sh
service openvpn restart
```

#6. Tận hưởng kết quả :)).

