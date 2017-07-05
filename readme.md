# PostgreSQL Monitoring with the Elastic Stack

This repository will install PostgreSQL to monitor it with the [Elastic Stack](https://www.elastic.co/products) (Elasticsearch, Logstash, Kibana, and Beats) and X-Pack. You can start from scratch and configure everything with [Vagrant and Ansible](#vagrant-and-ansible).



## Features

* Logs: Collect the PostgreSQL log and parse it in a sensible fashion.
* Metrics: Collect the statistics PostgreSQL can provide.
* Network: Monitor the PostgreSQL network traffic and visualize its performance.
* Activity: Generate PostgreSQL data with [SQLsmith](https://github.com/anse1/sqlsmith): `$ /opt/sqlsmith/sqlsmith --verbose --target="host=localhost port=5432 dbname=test user=test password=test"`
* Injector: Generate Elasticsearch data: `$ java -jar /opt/injector.jar 100000 1000`



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

For workshops, reset the fully configured environment with `$ ansible-playbook /elastic-stack/prepare-workshop.yml`.


## Kibana

Access Kibana at [http://localhost:5601](http://localhost:5601). You will need to login into Kibana with the default credentials — username `elastic` and password `changeme`.



## PostgreSQL

`test` database to run random queries, which is also accessible from the host system: `$ psql --host=localhost --user=test --dbname=test` with the password `test`.

`elastic` database used for metrics, which is also accessible from the host system: `$ psql --host=localhost --user=elastic --dbname=elastic` with the password `changeme`.

SSL is disabled so that Packetbeat can monitor the traffic. If you do not want to monitor the network traffic, it is safe to enable SSL.
