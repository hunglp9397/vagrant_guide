# Hướng dẫn vagrant 
 
## VD1: Tạo 1 máy ảo đơn giản
- Tạo thư mục vagrant-test
- Chạy lệnh : `vagrant init`
- Sửa Vagrantfile như sau:
```shell
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|                # Bắt đầu khai báo máy ảo
  config.vm.box = 'centos/7'                    # Sử dụng Box centos/7 tạo máy ảo
  config.ssh.insert_key = false                   

  config.vm.provider "virtualbox" do |vb|       # Máy ảo dùng nền tảng virtualbox, với các cấu hình bổ sung thêm cho provider
     vb.name = "may-ao-01"                      # đặt tên máy ảo tạo ra
     vb.cpus = 2                                # cấp 2 nhân CPU
     vb.memory = "2048"                         # cấu hình dùng 2GB bộ nhớ
  # config.vm.synced_folder ".", "/vagrant", type: "rsync",
  #    rsync__args: ["--chmod=ug=rwX,o=rxX","--verbose", "--archive", "--delete", "-z"]
  
    end                                           # hết cấu hình provider
end                                             #  hết cấu hình tạo máy ảo
```

- Sau đó khởi động máy ảo bằng lệnh : `vargrant up` 
- Kết nối tới máy ảo bằng lệnh: `vagrant ssh` (Mặc định sẽ đăng nhập với tài khoản là "vagrant")
    + Nếu muốn đăng nhập bằng tài khoản root thì chạy lệnh : `vagrant -i`
    ![v1](/img_guide/v0.png)

## VD2: Đồng bộ thư mục
- Giả sử tại máy host, tạo một file là hello.txt. Ta muốn đồng bộ file này từ máy host vào máy ảo 
- B1: Tại folder của máy host, cài plugin: `vagrant plugin install vagrant-vbguest`
- B2: Tại folder của máy host, tạo file hello.txt
- B3: Kết nối tới máy ảo, tạo folder : /data/mydata/
- B4: Sửa file Vargrantfile : thêm dòng lệnh đồng bộ sau: `config.vm.synced_folder '.', '/data/mydata/' `
- B5: Chi tiết Vargrantfile như sau:
```shell
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|                # Bắt đầu khai báo máy ảo
  config.vm.box = 'centos/7'                    # Sử dụng Box centos/7 tạo máy ảo
  config.ssh.insert_key = false                   
  config.vm.synced_folder '.', '/data/mydata', type: "rsync", rsync__args: ["--verbose", "--rsync-path='sudo rsync'", "--archive", "--delete", "-z"]
  config.vm.provider "virtualbox" do |vb|       # Máy ảo dùng nền tảng virtualbox, với các cấu hình bổ sung thêm cho provider
     vb.name = "may-ao-01"                      # đặt tên máy ảo tạo ra
     vb.cpus = 2                                # cấp 2 nhân CPU
     vb.memory = "2048"                         # cấu hình dùng 2GB bộ nhớ
  # config.vm.synced_folder ".", "/vagrant", type: "rsync",
  #    rsync__args: ["--chmod=ug=rwX,o=rxX","--verbose", "--archive", "--delete", "-z"]
  
    end                                           # hết cấu hình provider
end                                             #  hết cấu hình tạo máy ảo
```
- B6: Sau khi sửa file cấu hình, nạp lại máy ảo bằng `vagrant reload `
    + Nếu run lệnh mà lỗi thì làm như sau:
        `vagrant ssh`
        `sudo yum -y update kernel`
        `exit`
        `vagrant reload --provision`
- Kết quả: Đồng bộ thành công:
- ![1.png](/img_guide/v1.png)
