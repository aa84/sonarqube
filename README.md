>> az group create --name my_playground --location westeurope

>> ssh-keygen

>> az vm create \
--resource-group my_playground \
--name my_vm \
--public-ip-address my_vm_pip \
--image ubuntults \
--size Standard_B1ms \
--admin-username alessandro \
--generate-ssh-keys


> by default the nsg my_vmNSG is created

>> az network nsg rule create \
--resource-group my_playground \
--nsg-name my_vmNSG \
--name allow9000 \
--protocol tcp \
--priority 150 \
--destination-port-range 9000 \
--access allow


>> az vm delete --resource-group my_playground --name my_vm --yes


# Install Docker for testing

>> curl -fsSL https://get.docker.com -o get-docker.sh
>> sudo sh get-docker.sh
>> sudo usermod -aG docker alessandro
>> sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
>> sudo chmod +x /usr/local/bin/docker-compose

# make changes permanent
>> echo "vm.max_map_count=524288" | sudo tee -a /etc/sysctl.conf
>> echo "fs.file-max=131072" | sudo tee -a /etc/sysctl.conf
>> sudo  sysctl -p

note: the firt two commands of the list above can be replaced with this one:

sed -i -e '$a\vm.max_map_count=524288' -e '/vm.max_map_count=524288/d' hello.txt
sed -i -e '$a\fs.file-max=131072' -e '/fs.file-max=131072d' hello.txt

>! the advantage is that I can run those command multiple time without duplicating the entries!

# generate self-signed-certificates
docker container run -v "$(pwd)/certs:/certs" -e HOST_NAME=sonarqubeaa.westeurope.cloudapp.azure.com diamol/cert-generator
> not: they are not good for azure
https://docs.microsoft.com/en-us/azure/key-vault/certificates/certificate-scenarios#formats-of-import-we-support
https://iceburn.medium.com/application-gateway-key-vault-how-to-set-ssl-certificate-with-azure-cli-d6de2e2a40cb



https://stackoverflow.com/questions/68147520/can-t-reset-the-admin-password-after-the-first-login-to-sonarqube

https://www.blackvoid.club/grafana-8-influxdb-2-telegraf-2021-monitoring-stack/
https://medium.com/geekculture/deploying-influxdb-2-0-using-docker-6334ced65b6c
https://dzone.com/articles/how-to-get-metrics-from-a-java-application-inside
