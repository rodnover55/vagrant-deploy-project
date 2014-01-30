# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
PROJECT_DESCRIPTION = "DE.json"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  project_config = JSON.parse(File.read(PROJECT_DESCRIPTION))
  config.vm.hostname = project_config['project']

  config.vm.box = project_config['project']

  config.vm.box_url = project_config['box']

  config.vm.network :private_network, ip: project_config['ip']

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = 'cookbooks'
    chef.add_recipe 'deploy-project'
    chef.json = {
        "deploy-project" => {
        }
    }
  end
end