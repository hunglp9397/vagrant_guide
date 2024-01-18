# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|                # Bắt đầu khai báo máy ảo
  config.vm.box = 'centos/7'                    # Sử dụng Box centos/7 tạo máy ảo
  config.ssh.insert_key = false     
  config.vm.network "private_network", ip: "192.168.10.55"   # Lập IP cho máy ảo             
  config.vm.synced_folder '.', '/var/www/public/', type: "rsync", rsync__args: ["--verbose", "--rsync-path='sudo rsync'", "--archive", "--delete", "-z"]
  config.vm.provider "virtualbox" do |vb|       # Máy ảo dùng nền tảng virtualbox, với các cấu hình bổ sung thêm cho provider
     vb.name = "may-ao-01"                      # đặt tên máy ảo tạo ra
     vb.cpus = 2                                # cấp 2 nhân CPU
     vb.memory = "2048"                         # cấu hình dùng 2GB bộ nhớ
  # config.vm.synced_folder ".", "/vagrant", type: "rsync",
  #    rsync__args: ["--chmod=ug=rwX,o=rxX","--verbose", "--archive", "--delete", "-z"]
  
    end                                           # hết cấu hình provider

    # Chạy các lệnh cài đặt
config.vm.provision "shell", inline: <<-SHELL
    # cài đặt Apache, PHP
    yum update -y
    yum install httpd php -y
    systemctl start httpd
    systemctl enable httpd

    # Tat SELinux cua CentOS
    setenforce 0
    sed -i --follow-symlinks 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux


    # Đổi root password thành 123 và cho phép login SSH qua root
    echo "123" | passwd --stdin root
    sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    systemctl reload sshd

    # Tạo file cấu hình vhost lưu vào /etc/httpd/conf.d/vhost.conf để Apache nạp
    echo '<VirtualHost *:80>
      DocumentRoot /var/www/public
      AllowEncodedSlashes On

      <Directory /var/www/public>
        Options +Indexes +FollowSymLinks
        DirectoryIndex index.php index.html
        Order allow,deny
        Allow from all
        AllowOverride All
        </Directory>
    </VirtualHost>' > /etc/httpd/conf.d/vhost.conf
    systemctl start httpd
SHELL
end  