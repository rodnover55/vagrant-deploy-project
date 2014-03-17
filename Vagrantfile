# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
PROJECT_DESCRIPTION = "DE.json"
PROJECT_PATH = '/var/www/'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  project_config = JSON.parse(File.read(PROJECT_DESCRIPTION))
  config.vm.hostname = project_config['deploy-project']['project']

  config.vm.box = project_config['deploy-project']['box'] || 'develop'

  config.vm.box_url = project_config['deploy-project']['box_url'] || ''

  config.vm.network :private_network, ip: project_config['deploy-project']['ip'] || '10.2.2.15'

  unless project_config['deploy-project']['memory'].nil?
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", project_config['deploy-project']['memory']]
    end
  end

  config.vm.synced_folder project_config['deploy-project']['source'], project_config['deploy-project']['path']

  unless project_config['deploy-project']['ip'].nil?
    project_config.merge({
        'mysql' => {
            'bind_address' => project_config['deploy-project']['ip']
        }
    })
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ['cookbooks', 'site-cookbooks']
    chef.roles_path = ['roles']

    chef.run_list = project_config['run_list']
    chef.run_list << 'recipe[deploy-project::develop]'

    chef.json = project_config
  end
end