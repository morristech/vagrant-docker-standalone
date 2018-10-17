$prepare_docker_engine_script = <<SCRIPT
apt-get update;
apt-get install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update && apt-get install -y docker-ce=18.03.0~ce-0~ubuntu;
service docker stop;
rm -rf /etc/docker/key.json
echo '{ "hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"] }' | tee -a /etc/docker/daemon.json
service docker start;
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.define "docker-standalone" do |config|
    config.vm.box = "deviantony/ubuntu-14.04-docker"
    config.vm.hostname = "docker-standalone"
    config.vm.network "private_network", ip: "10.0.8.10"
    config.vm.provision "shell", inline: $prepare_docker_engine_script
    config.vm.synced_folder ".", "/vagrant", disabled: true
  end
end

