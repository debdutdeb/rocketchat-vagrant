# vi: set ft=ruby
Vagrant.configure "2" do |config|

    host_addrs = {
        "mongodb" => "10.0.0.2",
        "rocketchat" => "10.0.0.3"
    }
    
    config.vm.define "mongodb" do |mongo|
        mongo.vm.name = "mongodb"
        mongo.vm.box = "ubuntu/focal64"
        mongo.vm.provider "virtualbox" do |vm|
            vm.memory = 1024
            vm.cpus = 1
            vm.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
        end
        mongo.vm.network "private_network",
            ip: host_addrs[mongo.vm.name]
        # mongo.vm.provision "shell", inline: <<-SHELL
        #     apt update -qqq
        #     apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4 \
        #     && echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" >> /etc/apt/sources.list.d/mongodb-org-4.0.list \
        #     && apt update -qqq \
        #     && apt install -y --no-install-recommends mongodb-org || exit 1
        #     sed -iE --file /etc/mongod.conf \
        #         -e 's/^#  engine:/  engine: mmapv1/g' \
        #         -e 's/^#replication:/replication:\n  replSetName: rs01/g' || true
        #     systemctl enable --now mongod \
        #     && sleep 5s && mongo --eval "printjson(rs.initiate())"
        # SHELL
    end

    config.vm.define "rocketchat" do |rocket|
        rocket.vm.name = "rocketchat"
        rocket.vm.box = "ubuntu/focal64"
        rocket.vm.provider "virtualbox" do |vm|
            vm.memory = 1024
            vm.cpus = 1
            vm.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
        end
        rocket.vm.network "forwarded_port",
            guest: 3000, host: 3000
        rocket.vm.network "private_network",
            ip: host_addrs[rocket.vm.name]
    end

end

