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


trove --debug create trusty-mysql 2 --size=2
wget http://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.img
wget http://cloud-images.ubuntu.com/saucy/current/saucy-server-cloudimg-amd64-disk1.img
wget http://cloud-images.ubuntu.com/trusty/current/trusty-server-cloudimg-amd64-disk1.img
qemu-img convert -O qcow2 precise-server-cloudimg-amd64-disk1.img precise-mysql.qcow2
qemu-img convert -O qcow2 saucy-server-cloudimg-amd64-disk1.img saucy-mysql.qcow2
qemu-img convert -O qcow2 trusty-server-cloudimg-amd64-disk1.img trusty-mysql.qcow2
glance image-create --name="precise" --disk-format=qcow2 --container-format=bare --is-public=true < precise-mysql.qcow2
glance image-create --name="saucy" --disk-format=qcow2 --container-format=bare --is-public=true < saucy-mysql.qcow2
glance image-create --name="trusty" --disk-format=qcow2 --container-format=bare --is-public=true < trusty-mysql.qcow2
