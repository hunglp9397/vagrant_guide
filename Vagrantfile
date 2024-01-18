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