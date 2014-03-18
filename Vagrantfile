Vagrant.configure("2") do |config|

  config.vm.define :ubuntu do |default_config|
    default_config.vm.hostname = "ubuntu"

    default_config.vm.box = "Official Ubuntu 12.04 daily amd64"
    default_config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"

    default_config.vm.network :private_network, ip: "33.33.33.130"

    default_config.vm.provision :shell,
      inline: <<SCRIPT
mkdir -p /var/chef/cache

cd /tmp
mkdir -p /etc/apt/sources.list.d
apt-get update && apt-get -y install libc6 debconf curl git-core

# Pre-import the rightscale public key
curl http://s3.amazonaws.com/rightscale_key_pub/rightscale_key.pub | apt-key add -

# Add rightscale sources
cat > /etc/apt/sources.list.d/rightscale_extra.sources.list <<EOF
deb http://rightlink-integration.test.rightscale.com/adhoc/ivory/v6.0/apt/ precise main
deb-src http://rightlink-integration.test.rightscale.com/adhoc/ivory/v6.0/apt/ precise main
EOF

apt-get update
apt-get -y --force-yes install rightlink-cloud-none
SCRIPT

    # This goes in a RightScript
    default_config.vm.provision :shell,
      inline: <<SCRIPT
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

sh -c "echo deb http://get.docker.io/ubuntu docker main\ > /etc/apt/ sources.list.d/docker.list"
apt-get update

curl -s https://get.docker.io/ubuntu/ | sh
SCRIPT
  end
  
end
