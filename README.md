# Elasticsearch cluster using Vagrant

Elasticsearch cluster that use [vagrant-aws](https://github.com/mitchellh/vagrant-aws) plugin to create multiple machine (using a configuration variable) on AWS. Also Elasticsearch is configured for production using the [cookbook-elasticsearch](https://github.com/elastic/cookbook-elasticsearch)

## How to use

```
1. clone the repo
2. Add your AWS secret_key, access_key and security-group(s) to the Vagrantfile
(optional) you can change your instances count in the Vagrantfile conf.count
3. vagrant up
4. vagrant ssh to every machine and `sudo service elasticsearch start`
(you can change the recipe file to auto restart the ES service but it is not recommended)
```


