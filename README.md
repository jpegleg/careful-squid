# carefaul-squid
Whitelist based squid proxy.

# Example usage:
mkdir /srv

cp squid.cfg /srv/squid.cfg

# add carefulsquid category to your inventory

echo "[carefulsquid]" >> /someplace/yourinventoryfile

# add the hostnames of the squid outbound layer 4 whitelist proxy servers to build/maintain

echo "some-squid-host" >> /someplace/yourinventoryfile

echo "some-other-squid-host" >> /someplace/yourinventoryfile

echo "some-other-squid-host" >> /someplace/yourinventoryfile

# whitelist some external sources

echo "somewhitelisedplace.com" >> /srv/whitelist.txt

echo "anotherplacetowhitelist.com" >> /srv/whitelist.txt

# white list some LAN IP

echo "192.168.1.10" >> /srv/whitelist.txt

/usr/bin/ansible-playbook --user root -i /someplace/yourinventoryfile ./carefulsquid.yml
