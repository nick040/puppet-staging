# Puppet Stagin

[![Puppet Forge](http://img.shields.io/puppetforge/v/puppet/staging.svg)](https://forge.puppetlabs.com/puppet/staging)
[![Puppet Forge downloads](https://img.shields.io/puppetforge/dt/puppet/staging.svg)](https://forge.puppetlabs.com/puppet/staging)
[![Puppet Forge score](https://img.shields.io/puppetforge/f/puppet/staging.svg)](https://forge.puppetlabs.com/puppet/staging)
[![Build Status](https://travis-ci.org/voxpupuli/puppet-staging.png)](https://travis-ci.org/voxpupuli/puppet-staging)

Manages staging directory, along with download/extraction of compressed files.

WARNING: Version 0.2.0 no longer uses hiera functions. The same behavior should be available in Puppet 3.0.

NOTE: Version 1.0.0 will be the last feature release. New functionality such as checksum will be implemented in a type/provider module [puppet-archive](https://www.github.com/voxpupuli/puppet-archive).

## Usage

Specify a different default staging path (must be declared before using resource):
```puppet
class { 'staging':
  path  => '/var/staging',
  owner => 'puppet',
  group => 'puppet',
}
```

Staging files from various sources:
```puppet
staging::file { 'sample':
  source => 'puppet://modules/staging/sample',
}

staging::file { 'apache-tomcat-6.0.35':
  source => 'http://apache.cs.utah.edu/tomcat/tomcat-6/v6.0.35/bin/apache-tomcat-6.0.35.tar.gz',
}
```

Staging and extracting files:
```puppet
staging::file { 'sample.tar.gz':
  source => 'puppet:///modules/staging/sample.tar.gz'
}

staging::extract { 'sample.tar.gz':
  target  => '/tmp/staging',
  creates => '/tmp/staging/sample',
  require => Staging::File['sample.tar.gz'],
}
```

Deploying a file (combining staging and extract):
```puppet
staging::deploy { 'sample.tar.gz':
  source => 'puppet:///modules/staging/sample.tar.gz',
  target => '/usr/local',
}
```

Staging files currently support the following source:

* http(s)://
* puppet://
* ftp://
* s3:// (requires aws cli to be installed and configured.)
* local (though this doesn't serve any real purpose.)

## Author

Primarily authored by Nan Liu

## Contributors

* Adrien Thebo
* gizero
* Harald Skoglund
* Hunter Haugen
* Justin Clayton
* Owen Jacobson
* Reid Vandewiele
