# PostgreSQL Monitoring with the Elastic Stack

This repository will install PostgreSQL to monitor it with the [Elastic Stack](https://www.elastic.co/products) (Elasticsearch, Logstash, Kibana, and Beats) and X-Pack. You can start from scratch and configure everything with [Vagrant and Ansible](#vagrant-and-ansible).



## Features

* Logs:
* Metrics:
* Network:
* Activity: Generate data with [SQLsmith](https://github.com/anse1/sqlsmith): `$ /opt/sqlsmith/sqlsmith --verbose --target="host=localhost port=5432 dbname=test user=test password=test"`



## Vagrant and Ansible

Do a simple `vagrant up` by using [Vagrant](https://www.vagrantup.com)'s [Ansible provisioner](https://www.vagrantup.com/docs/provisioning/ansible.html). All you need is a working [Vagrant installation](https://www.vagrantup.com/docs/installation/) (1.8.6+ but the latest version is always recommended), a [provider](https://www.vagrantup.com/docs/providers/) (tested with the latest [VirtualBox](https://www.virtualbox.org) version), and 2.5GB of RAM.

With the [Ansible playbooks](https://docs.ansible.com/ansible/playbooks.html) in the */elastic-stack/* folder you can configure the whole system step by step. Just run them in the given order inside the Vagrant box:

```
> vagrant ssh
$ ansible-playbook /elastic-stack/1_configure-postgresql.yml
$ ansible-playbook /elastic-stack/2_configure-elasticsearch.yml
$ ansible-playbook /elastic-stack/3_configure-kibana.yml
$ ansible-playbook /elastic-stack/4_configure-logstash.yml
$ ansible-playbook /elastic-stack/5_configure-beats.yml
$ ansible-playbook /elastic-stack/6_configure-dashboards.yml
```

Or if you are in a hurry, run all playbooks with `$ /elastic-stack/all.sh` at once.


## Kibana

Access Kibana at [http://localhost:5601](http://localhost:5601). You will need to login into Kibana with the default credentials — username `elastic` and password `changeme`.



## PostgreSQL

There is a `test` database, which you can access with the username `test` and password `test`.

There is also an `elastic` database used for metrics, which you can access with the username `elastic` and the password `changeme`.

SSL is disabled so that Packetbeat can monitor the traffic. If you do not want to monitor the network traffic, it is safe to enable SSL.
