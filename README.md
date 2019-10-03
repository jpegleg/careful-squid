# carefaul-squid
Whitelist based squid proxy.

# Example usage:
mkdir /srv
cp squid.cfg /srv/squid.cfg
echo "[carefulsquid]" >> /someplace/yourinventoryfile
echo "some-squid-host" >> /someplace/yourinventoryfile
echo "some-other-squid-host" >> /someplace/yourinventoryfile
echo "some-other-squid-host" >> /someplace/yourinventoryfile

/usr/bin/ansible-playbook --user root -i /someplace/yourinventoryfile ./carefulsquid.yml
