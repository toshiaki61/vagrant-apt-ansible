vagrant-apt-ansible
===================

vagrant up

vagrant ssh controller

source keystonerc_admin

glance image-create --name="cirros-0.3.2-x86_64" --disk-format=qcow2 --container-format=bare --is-public=true --copy-from http://cdn.download.cirros-cloud.net/0.3.2/cirros-0.3.2-x86_64-disk.img
glance image-list
cinder create --display-name myVolume 1
cinder list

echo 'test' > test.txt
echo 'test2' > test2.txt
swift upload myfiles test.txt
swift upload myfiles test2.txt
rm -f test*
swift download myfiles
cat test2.txt

heat stack-create -f /vagrant/provisioning/roles/heat/files/test-stack.yml -P "ImageID=cirros-0.3.2-x86_64;NetID=$(nova net-list | awk '/ demo-net / { print $2 }')" testStack

heat stack-list

ceilometer meter-list
glance image-download "cirros-0.3.2-x86_64" > /dev/null
ceilometer meter-list | grep download
ceilometer statistics -m image.download -p 60

sudo -u trove trove-manage --config-file=/etc/trove/trove.conf datastore_version_update mysql mysql-5.5 mysql $(glance image-list | awk '/ cirros/ { print $2 }') mysql-server-5.5 1

trove list
#update datastores set default_version_id='e00e990c-5e64-4047-8003-9f1b9e44809c' where name='mysql';
trove --debug create db1 2 --size=2
