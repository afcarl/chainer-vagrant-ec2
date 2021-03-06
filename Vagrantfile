# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

ec2_config = YAML.load_file('config.yml')

Vagrant.configure(2) do |config|
  config.vm.box = "dummy"
  config.vm.synced_folder ".", "/vagrant", type: "rsync",
                          rsync__exclude: [".git/", "config.yml"]

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ec2_config["access_key_id"]
    aws.secret_access_key = ec2_config["secret_access_key"]
    aws.keypair_name = ec2_config["keypair_name"]

    aws.block_device_mapping = [{"DeviceName" => "/dev/sda1", "Ebs.VolumeSize" => 50}]
    
    aws.associate_public_ip = true
    aws.ami = "ami-746aba14"
    aws.instance_type = "g2.2xlarge"
    aws.region = "us-west-2"
    aws.security_groups = ec2_config["security_groups"]
    aws.subnet_id = ec2_config["subnet_id"]

    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = ec2_config["private_key_path"]
  end

  config.vm.provision :fabric do |fabric|
    fabric.fabfile_path = "./fabfile.py"
    fabric.tasks = ["install_chainer"]
  end
end
