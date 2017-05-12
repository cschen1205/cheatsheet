
# Install the required package

Run the following commands

* sudo yum install gcc-c++ patch readline readline-devel zlib zlib-devel
* sudo yum install libyaml-devel libffi-devel openssl-devel make
* sudo yum install bzip2 autoconf automake libtool bison iconv-devel sqlite-devel

# Install RVM

Run the following commands

* curl -sSL https://rvm.io/mpapis.asc | gpg --import -
* curl -L get.rvm.io | bash -s stable
* source ~/.profile
* rvm reload

# Verify Dependencies

Run the following commands

* rvm requirements run

# Install Ruby 2.2

Run the following commands

* rvm install 2.2.4
