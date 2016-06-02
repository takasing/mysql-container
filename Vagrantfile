# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

SHARED_DIR="/home/core/share"

$update_channel = "stable"
$image_version = "current"
$create_volume = <<SCRIPT
COUNT=`docker volume ls | grep local | wc -l`
if [ $COUNT -lt 1 ]; then
  docker run \
    -d \
    --name storage \
    -v /var/lib/mysql \
    busybox
fi
SCRIPT
$rm_mysql = <<SCRIPT
COUNT=`docker ps | grep mysql | wc -l`
if [ $COUNT -gt 0 ]; then
  docker rm -f mysql
fi
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "coreos-%s" % $update_channel
  config.vm.box_url = "http://%s.release.core-os.net/amd64-usr/%s/coreos_production_vagrant.json" % [$update_channel, $image_version]
  config.vm.define "local" do |cfg|
    cfg.ssh.insert_key = false
    cfg.vm.host_name = "local"
    cfg.vm.network :private_network, ip: "172.10.1.101"
    cfg.vm.synced_folder ".", SHARED_DIR, id: "core", :nfs => true, :mount_options => ['nolock,vers=3,udp']

    cfg.vm.provision "create volume", type: "shell", inline: $create_volume
    cfg.vm.provision "remove mysql container", type: "shell", inline: $rm_mysql

    cfg.vm.provision "mysql", type: "docker" do |d|
      d.build_image "#{SHARED_DIR}/docker/mysql", args: "-t takasing104/mysql:0.0.1"
      d.run "mysql",
        image: "takasing104/mysql:0.0.1",
        args: "-p 3306:3306 --volumes-from storage",
        auto_assign_name: true,
        daemonize: true
    end
  end
end

