# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
PROJECT_DESCRIPTION = "DE.json"
PROJECT_PATH = '/var/www/'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  project_config = JSON.parse(File.read(PROJECT_DESCRIPTION))
  config.vm.hostname = project_config['project']

  config.vm.box = project_config['project']

  config.vm.box_url = project_config['box_url']

  config.vm.network :private_network, ip: project_config['ip']
  project_config['source'] ||= '../'
  project_config['path'] ||= PROJECT_PATH + project_config['project']
  config.vm.synced_folder project_config['source'], project_config['path']

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = 'cookbooks'
    chef.add_recipe 'deploy-project::develop'
    chef.json = {
        "deploy-project" => project_config
    }
  end
end