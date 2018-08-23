# -*- mode: ruby -*-
# vi: set ft=ruby :

##
# If you copy this file, dont't delete this comment.
# This Vagrantfile was created by Daniel Menezes:
#   https://github.com/danielmenezesbr/modernie-winrm
#   E-mail: danielmenezes at gmail dot com
##

##
# Version: 0.0.4
##
#

      

require 'yaml'
require_relative 'ie-box-automation-plugin.rb'

inventory = YAML.load_file('inventory.yml')


Vagrant.configure("2") do |config|

  inventory['all']['hosts'].each do |server, details|
    config.vm.define server do |srv|
      srv.vm.box = "MSEDGE_Win10"
      srv.vm.boot_timeout = 5000

      srv.vm.guest = :windows
      srv.vm.hostname = server

      srv.vm.communicator = :winrm       if provisioned?
      srv.winrm.username = "IEUser"      if provisioned?
      srv.winrm.password = "Passw0rd!"   if provisioned?
      srv.winrm.timeout = 50000          if provisioned?
      srv.winrm.retry_delay = 30         if provisioned?
      srv.winrm.retry_limit = 1000       if provisioned?

      srv.ssh.username = "IEUser"
      srv.ssh.password = "Passw0rd!"
      srv.ssh.insert_key = false
      srv.vbguest.auto_update = false

      srv.vm.box_check_update = false

      srv.vm.synced_folder ".", "/vagrant", disabled: true                     if not provisioned?
      srv.vm.synced_folder "./ExtraFolder", "c:/ExtraFolder", create: false    if provisioned?

      srv.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        vb.name = File.basename(File.dirname(__FILE__)) + "_" + server + "_" + Time.now.to_i.to_s
        vb.gui = false
        vb.memory = 2048
        vb.cpus = 2
      end

      srv.vm.provision "file", source: "./tools/7z.exe", destination: "c:/users/IEUser/7z.exe"
      srv.vm.provision "file", source: "./tools/7z.dll", destination: "c:/users/IEUser/7z.dll"
      srv.vm.provision "file", source: "./tools/tools.zip", destination: "c:/users/IEUser/tools.zip"
      srv.vm.provision "winrm", type: "ie_box_automation"
    end
  end
end
