# load .env
Dotenv.load

Vagrant.configure(2) do |config|
  config.vm.define "circleci"
  config.vm.box = "geerlingguy/centos7"

  config.vm.synced_folder ENV['CCI_TARGET_PATH'], "/vagrant"
  
  config.vm.provider :virtualbox do |vb|
    vb.customize [
      "modifyvm", :id,
      "--hwvirtex", "on",
      "--nestedpaging", "on",
      "--largepages", "on",
      "--memory", "2048",           # 割り当てるRAM(お好きな値で)
      "--cpus", "2",                # 割り当てるコア数(お好きな値で)
      "--ioapic", "on",
      "--pae", "on",
      "--paravirtprovider", "kvm"
    ]
  end

  if ARGV[0] == "up" then
    config.vm.provision :shell do |shell|
      shell.inline = <<-SHELL
        sudo yum install -y yum-utils device-mapper-persistent-data lvm2 && \
        sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
        sudo yum-config-manager --enable docker-ce-edge && \
        sudo yum install -y docker-ce && \
        sudo systemctl start docker && \
        sudo curl -o /usr/local/bin/circleci https://circle-downloads.s3.amazonaws.com/releases/build_agent_wrapper/circleci && sudo chmod +x /usr/local/bin/circleci && \
        sudo /usr/local/bin/circleci update &&
        sudo sed -i.bak -e "s/docker run -it/docker run/" /usr/local/bin/circleci
      SHELL
      shell.privileged = false
    end
  end

  if ARGV[0] == "provision" then
    config.vm.provision :shell do |shell|
      shell.inline = <<-SHELL
        sudo systemctl start docker && \
        cd /vagrant/ && \
        sudo /usr/local/bin/circleci config validate -c .circleci/config.yml && \
        sudo /usr/local/bin/circleci build .circleci/config.yml
      SHELL
      shell.privileged = false
    end
  end
end
