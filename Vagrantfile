# from https://github.com/mitchellh/vagrant-aws

require 'yaml'
require 'ostruct'
require 'erubis'

def readconf(filename)
  #reads a yaml file in 'config/vagrant'
  pwd = File.dirname(__FILE__)
  conf_file = File.join(pwd, "config", filename)
  vconf = Erubis::Eruby.new(File.open(conf_file, File::RDONLY).read)
  vconf = vconf.result(def_host_share: pwd)
  vconf = YAML::load(vconf)
end

@conf = readconf('vagrant.yml')['default']

Vagrant.configure("2") do |config|
  config.vm.box = @conf['box']

  config.vm.provider :aws do |aws, override|
    aws.ami = @conf['aws']['ami']
    aws.region = @conf['aws']['region']
    #aws.access_key_id = @conf['aws']['access_key_id'] if @conf['aws'].key? 'access_key_id'
    #aws.secret_access_key = @conf['aws']['secret_access_key'] if @conf['aws'].key? 'secret_access_key'

    aws.keypair_name = @conf['aws']['keypair_name']
    aws.security_groups = @conf['aws']['security_groups']

    override.ssh.username = @conf['aws']['ssh']['username']
    override.ssh.private_key_path = @conf['aws']['ssh']['private_key_path']
  end
end
