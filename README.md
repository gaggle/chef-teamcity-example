# chef-teamcity-example
Simple example of using the [chef-teamcity cookbook] (https://github.com/alexfalkowski/chef-teamcity):

    bundle install
    librarian-chef install
    vagrant up

Visit `http://192.168.80.10:8111`, the Teamcity server is now running locally.

To speed up subsequent provisioning install the [vagrant-cachier plugin] (https://github.com/fgrehm/vagrant-cachier):

    vagrant plugin install vagrant-cachier
