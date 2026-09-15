# Every Vagrantfile starts with this version declaration
Vagrant.configure("2") do |config|

  # The base image (box) — what OS your VM runs
  config.vm.box = "eurolinux-vagrant/centos-stream-9"

  # YMB requirement: every VM gets a hostname for asset inventory
  config.vm.hostname = "ymb-zone-b-dev"

  # Private network — only your host machine can reach this IP
  config.vm.network "private_network", ip: "192.168.56.13"

  # Hardware resources — match Zone B server specs
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 2
  end

  # Provisioning — runs automatically on first vagrant up
  config.vm.provision "shell", inline: <<-SHELL
    yum install httpd wget unzip zip vim -y
    systemctl start httpd
    systemctl enable httpd

    # Download and deploy the YMB portfolio page
    mkdir -p /tmp/studentportfolio && cd /tmp/studentportfolio
    wget https://github.com/.../main.zip -O main.zip
    unzip -o main.zip && cp -r Studentportfoliotemplate-main/* /var/www/html/

    # Create status file with provisioning date
    date > /var/www/html/status.txt

    systemctl restart httpd
  SHELL

end