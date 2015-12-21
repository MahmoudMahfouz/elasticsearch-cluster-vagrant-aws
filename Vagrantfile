# -*- mode: ruby -*-
# vi: set ft=ruby :

conf = {
  count: 3,
  env: 'production'
}
Vagrant.configure(2) do |config|
  conf[:count].times do |i|
    name = "elasticsearch-#{i+1}"
    region = "us-east-1"
    access_key = ## add you access key here
    secret_key = ## add your secret key here
    availability_zone = "us-east-1c"
    cluster_name = "ibg-#{conf[:env]}-cluster"

    config.vm.define name do |cfg|
      cfg.vm.synced_folder '.', '/vagrant', :disabled => true
      cfg.vm.box = "ubuntu_aws"
      cfg.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
      cfg.vm.provider :aws do |aws, override|
        aws.access_key_id = access_key
        aws.secret_access_key = secret_key
        aws.keypair_name = 'vagrant'

        aws.ami = "ami-d05e75b8" #Ubuntu 14.10 with ssd support
        aws.region = region
        aws.availability_zone = availability_zone
        aws.instance_type = "m3.medium"

        aws.tags = { Name: name, discovery_tag: cluster_name }
        aws.security_groups = ## add your security group here

        override.vm.box = name
        override.vm.hostname = name
        override.ssh.username = "ubuntu"
        override.ssh.private_key_path = '~/.ssh/vagrant.pem'
      end

      cfg.vm.provision 'chef_solo' do |chef|
        chef.add_recipe 'java'
        chef.add_recipe 'elasticsearch::ibg'
        chef.json = {
          java: {
            jdk_version: '8'
          },
          elasticsearch: {
            config: {
              'cluster.name' => cluster_name,
              'node.name' => name,
              'indicies.memory.index_buffer_size' => '40%',
              'bootstrap.mlockall' => true,
              'cloud.aws.access_key' => access_key,
              'cloud.aws.secret_key' => secret_key,
              'discovery.type' => "ec2",
              'discovery.ec2.tag.discovery_tag' => cluster_name,
              'discovery.ec2.host_type' => "public_ip",
              'discovery.ec2.ping_timeout' => "30s",
              'discovery.ec2.availability_zones' => availability_zone,
              'cloud.aws.region' => region,
              'discovery.zen.ping.multicast.enabled' => false,
              'network.host' => '_ec2:privateIp_',
              # 'network.bind_host' => ['_ec2:privateIp_', '_ec2:publicIp_'],
              # 'network.publish_host' => ['_ec2:privateIp_', '_ec2:publicIp_'],
            },
            version: '2.0.0',
          }
        }
      end

    end
  end
end
