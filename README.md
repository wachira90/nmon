# nmon nmonchart centos 7


## install

yum install nmon -y

OR

rpm -ivh \<rpmfile>

## checkpath install 

which sudo 

which nmon

## add to crontab with root permission all day keep

crontab -e

nmon \<parameter> -m \<path store location>

0 0 * * * /usr/bin/sudo /bin/nmon -ft -s 120 -c 720 -m /home/docker/.nmon-data

## generate html report file

/bin/bash nmonchart \<nmonfile source> \<html destination>

sudo /bin/bash   /var/www/example.com/nchart/nmonchart   /home/docker/.nmon-data/host1_201213_0000.nmon    /var/www/example.com/nchart/host1_201213_0000.html
