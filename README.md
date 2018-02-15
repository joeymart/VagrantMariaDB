# VagrantMariaDB
install vagrant
cd to directory with Vagrantfile

activate plugins:
`$ vagrant plugin install vagrant-reload`

`$ vagrant plugin install vagrant-vbguest`

Start
`$ vagrant up`

Get to mysql:
`$ mysql -uroot -A --host=127.0.0.1 --port=3333`

Glide Connection string:
`glide.db.url=jdbc:mysql://localhost:3333/`
