# Juju-as-a-Service
## MaaS installation
```
sudo snap install maas-test-db
sudo snap install maas --channel=3.3/stable
sudo maas init region+rack --maas-url http://MAAS_IP_ADDRESS:5240/MAAS --database-uri maas-test-db:///
sudo maas createadmin --username aslab --password aslab --email aslab@aslab.com
sudo maas apikey --username aslab > ~/apikey
```
```
ssh-keygen -t rsa
```
```
sudo snap install juju --classic
```
```
clouds:
  mymaas:
    type: maas
    auth-types: [oauth1]
    endpoint: http://MAAS_IP_ADDRESS:5240/MAAS
```
```
credentials:
  mymaas:
    aslab:
      auth-type: oauth1
      maas-oauth: API_KEY
```
```
juju add-cloud --client -f cloud.yaml mymaas
juju add-credential --client -f cred.yaml mymaas
```
```
juju bootstrap --bootstrap-series=jammy --constraints tags=juju mymaas mymaas-controller
```
```
juju add-model --config default-series=bionic laboratory
```
```
juju dashboard
```
```
juju add-machine --constraints tags=machine
```
```
juju deploy --to 0 wordpress
juju deploy --to 0 mysql --channel=edge
juju add-relation mysql wordpress
juju expose wordpress
```
