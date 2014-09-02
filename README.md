This is a demo on how to setup consul + registrator + consul-haproxy + a nodejs application server to get a multihost loadbalancer using consul for service discovery.


vagrant up

Consol 1:
vagrant ssh lb
cd /cluster/lb
fig up

Consol 2:
vagrant ssh app
cd /cluster/app
fig up -d
fig scale app=3


TODO:
Use $PRIVATE_IP and $JOIN_IP instead of hardcoded IP's in fig.yml's
...