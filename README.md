# carefaul-squid
Whitelist based squid proxy. 

Scenario: you have a cloud or datacenter and you want funnel all outbound traffic through a proxy that only 
allows outgoing requests from your cloud to certain locations.

You can maintain a local firewall, or a firewall cloud, and perform these functions. But you might want to let your firewall do other work and let a more dynamic solution be used. With an easy to maintain text file as the whitelist, you can
go wild with how you want to administrate it, and not be restricted to having to constantly edit firewall configs and access critical network devices just to update the whitelist.

A solution is building squid proxy hosts that receive and restrict the traffic to https and only the whitelisted outbound.

You could enforce it strictly with a NAT or iptables, or you can use user profiles, or just build in the proxy into the application.

iptables example concept:

iptables -t nat -A PREROUTING -i eth0 -s ! your_proxy_server -p tcp --dport 80 -j DNAT --to your_proxy_server:3128

iptables -t nat -A POSTROUTING -o eth0 -s local-network -d your_proxy_server -j SNAT --to iptables-box

iptables -A FORWARD -s local-network -d your_proxy_server -i eth0 -o eth0 -p tcp --dport 3128 -j ACCEPT


Within the profile concept:

http_proxy=http://user:password@proxyserver.com:3128

https_proxy=https://user:password@proxyserver.com:3128

export http_proxy

export https_proxy


Or just within the app concept:

curl https://carefuldata.com/api/specialrequests-1 --proxy your_proxy_server:3128



# Example ansible deployment usage:

# I use a /srv directory by default

mkdir /srv

# get your copy of the squid.cfg to the /srv spot

cp squid.cfg /srv/squid.cfg

# add carefulsquid category to your inventory

echo "[carefulsquid]" >> /someplace/yourinventoryfile

# add the hostnames of the squid outbound layer 4 through 7 whitelist proxy servers to build/maintain

echo "some-squid-host" >> /someplace/yourinventoryfile

echo "some-other-squid-host" >> /someplace/yourinventoryfile

echo "some-other-squid-host" >> /someplace/yourinventoryfile

# whitelist some external sources

echo "somewhitelistedplace.com" >> /srv/whitelist.txt

echo "anotherplacetowhitelist.com" >> /srv/whitelist.txt

# white list some LAN IP

echo "192.168.1.10" >> /srv/whitelist.txt

# run an ansible playbook to install squid, keep it updated, deploy the config and whitelist, enable automatic start up, and restart the service

/usr/bin/ansible-playbook --user root -i /someplace/yourinventoryfile ./carefulsquid.yml
