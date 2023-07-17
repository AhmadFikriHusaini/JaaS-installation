# Juju-as-a-Service
* Install MaaS
```
sudo snap install maas-test-db
sudo snap install maas --channel=3.3/stable
sudo maas init region+rack --maas-url http://MAAS_IP_ADDRESS:5240/MAAS --database-uri maas-test-db:///
sudo maas createadmin --username aslab --password aslab --email aslab@aslab.com
sudo maas apikey --username aslab > ~/apikey
```
* generate **SSH-key** untuk remote access
```
ssh-keygen -t rsa
```
* install juju
```
sudo snap install juju --classic
```
* membuat file **cloud.yaml**
```
clouds:
  mymaas:
    type: maas
    auth-types: [oauth1]
    endpoint: http://MAAS_IP_ADDRESS:5240/MAAS
```
* membuat file **cred.yaml**
```
credentials:
  mymaas:
    aslab:
      auth-type: oauth1
      maas-oauth: API_KEY
```
* menambahkan Cloud (MaaS) service kedalam juju
```
juju add-cloud --client -f cloud.yaml mymaas
juju add-credential --client -f cred.yaml mymaas
```
* instalasi *juju-controller* ke MaaS
siapkan 1 machine yang sudah commissioning dalam MaaS Cloud kemudian berikan tags `juju`
```
juju bootstrap --bootstrap-series=jammy --constraints tags=juju mymaas mymaas-controller
```
* buat model baru pada Juju
```
juju add-model laboratory
```
* masuk ke dashboard JaaS
```
juju dashboard
```
* menambahkan machine ke juju models dengan tags `machine`
```
juju add-machine --constraints "tags=machine" --series jammy
```
* deploying apps ke machine yang terdaftar di juju
```
juju deploy --to 0 --series bionic wordpress
juju deploy --to 0 --series bionic mysql --channel latest/edge
juju add-relation mysql wordpress
juju expose wordpress
```
