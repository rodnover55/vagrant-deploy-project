# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
PROJECT_DESCRIPTION = "../dev.json"
PROJECT_PATH = '/var/www/'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  project_config = JSON.parse(File.read(PROJECT_DESCRIPTION))
  config.vm.hostname = project_config['deploy-project']['project']

  config.vm.box = project_config['deploy-project']['box']

  config.vm.box_url = project_config['deploy-project']['box_url']

  config.vm.network :private_network, ip: project_config['deploy-project']['ip']

  unless project_config['deploy-project']['memory'].nil?
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", project_config['deploy-project']['memory']]
    end
  end

  project_config['deploy-project']['source'] ||= '../'
  project_config['deploy-project']['path'] ||= PROJECT_PATH + project_config['deploy-project']['project']
  config.vm.synced_folder project_config['deploy-project']['source'], project_config['deploy-project']['path']

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = 'cookbooks'
    chef.add_recipe 'deploy-project::develop'
    chef.json = project_config
  end
end