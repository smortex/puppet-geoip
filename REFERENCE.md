# Reference
<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

**Classes**

* [`geoip`](#geoip): The geoip module installs tools and databases GeoIP resolution from MaxMind.  You may replace userid and licensekey with your subscription an
* [`geoip::config`](#geoipconfig): This class implements the configuration stage of this module. It should not be called directly.  Configuration file defined with `geoip::conf
* [`geoip::config::ge311`](#geoipconfigge311): This class is creating a GeoIP.conf configuration file for geoipupdate versions >= 3.1.1.
* [`geoip::config::lt311`](#geoipconfiglt311): This class is creating a GeoIP.conf configuration file for geoipupdate versions < 3.1.1.
* [`geoip::install`](#geoipinstall): This class implements the installation stage of the module. It should not be called directly.  This will install or remove software packages 
* [`geoip::service`](#geoipservice): This class implements the service control stage of the module. It should not be called directly.  If `geoip::manage_service` enabled, an upda
* [`geoip::systemd::service`](#geoipsystemdservice): Controling SystemD service unit for update
* [`geoip::systemd::timer`](#geoipsystemdtimer): Controll the SystemD Timer unit

## Classes

### geoip

The geoip module installs tools and databases GeoIP resolution from MaxMind.

You may replace userid and licensekey with your subscription and
add the productids you want to sync. Leaving these options on default
will allow you to sync all free available databases. With
database_directory the destination directory of the databases can be
set, protocol, proxy* and *_verification may only be needed in the
case your host needs some specific proxy settings to get to the
internet.

#### Examples

##### hiera

```puppet
geoip::config:
  userid: '999999'
  licensekey: '000000000000'
  productids:
    - GeoLite2-City
    - GeoLite2-Country
  database_directory:
  protocol:
  proxy:
  proxy_user_password:
  skip_hostname_verification:
  skip_peer_verification:
```

#### Parameters

The following parameters are available in the `geoip` class.

##### `ensure`

Data type: `Enum['present', 'absent']`

install or remove settings done by this module

Default value: 'present'

##### `packages`

Data type: `Array[String]`

the software packages containing the tools to be installed

Default value: ['mmdb-bin', 'geoipupdate']

##### `package_ensure`

Data type: `String`

which version of the packages should be ensured

Default value: 'latest'

##### `config_path`

Data type: `Stdlib::Absolutepath`

path to the configuration and license file

Default value: '/etc/GeoIP.conf'

##### `config_version`

Data type: `Enum['lt311','ge311']`

the config version to use for geoipupdate

Default value: 'lt311'

##### `config`

Data type: `Hash`

hash of configuration options

Default value: {}

##### `manage_service`

Data type: `Boolean`

whether to manage database updating service

Default value: `true`

##### `update_path`

Data type: `Stdlib::Absolutepath`

path to the geoipupdate tool, used by update service

Default value: '/usr/bin/geoipupdate'

##### `service_name`

Data type: `String`

name of the update service

Default value: 'geoip_update'

##### `update_timers`

Data type: `Array[String]`

wallclock timers when the update service should be triggered (for syntax see [systemd.time(7)#Parsing Timestamps](https://www.freedesktop.org/software/systemd/man/systemd.time.html#Parsing%20Timestamps))

Default value: []

##### `update_scatter`

Data type: `Integer`

a time window in seconds of randomized, host specific delay of the update trigger (see [systemd.timer(5)#AccuracySec](https://www.freedesktop.org/software/systemd/man/systemd.timer.html#AccuracySec=))

Default value: 1800

### geoip::config

This class implements the configuration stage of this module. It should not be called directly.

Configuration file defined with `geoip::config_path` will be created using parameter from
`geoip::config`.

### geoip::config::ge311

This class is creating a GeoIP.conf configuration file for geoipupdate
versions >= 3.1.1.

#### Parameters

The following parameters are available in the `geoip::config::ge311` class.

##### `accountid`

Data type: `String`

AccountID of your MaxMind subscription

##### `licensekey`

Data type: `String`

The license key issued by MaxMind to be used

##### `editionids`

Data type: `Array[String]`

Edition IDs (formerly known as ProductIDs) to download

Default value: ['GeoLite2-ASN','GeoLite2-City','GeoLite2-Country']

##### `database_directory`

Data type: `Optional[Stdlib::Absolutepath]`

Path where the database file to be stored

Default value: `undef`

##### `host`

Data type: `Optional[String]`

The update server to use

Default value: `undef`

##### `proxy`

Data type: `Optional[String]`

URL or IP:Port of the proxy to use when accessing the update server

Default value: `undef`

##### `proxy_user_password`

Data type: `Optional[String]`

Credentials as username:password for proxy authentication

Default value: `undef`

##### `preserve_file_times`

Data type: `Optional[Boolean]`

Whether to preserve modification times of files downloaded from the server

Default value: `undef`

##### `lock_file`

Data type: `Optional[String]`

The lock file to use

Default value: `undef`

### geoip::config::lt311

This class is creating a GeoIP.conf configuration file for geoipupdate
versions < 3.1.1.

#### Parameters

The following parameters are available in the `geoip::config::lt311` class.

##### `userid`

Data type: `String`

UserID of your MaxMind subscription

##### `licensekey`

Data type: `String`

The license key issued by MaxMind to be used

##### `productids`

Data type: `Array[String]`

Product IDs to download

Default value: ['GeoLite2-ASN','GeoLite2-City','GeoLite2-Country']

##### `database_directory`

Data type: `Optional[Stdlib::Absolutepath]`

Path where the database file to be stored

Default value: `undef`

##### `protocol`

Data type: `Optional[Enum['http','https']]`

Protocol to be used when accessing the update server

Default value: `undef`

##### `proxy`

Data type: `Optional[String]`

URL or IP:Port of the proxy to use when accessing the update server

Default value: `undef`

##### `proxy_user_password`

Data type: `Optional[String]`

Credentials as username:password for proxy authentication

Default value: `undef`

##### `skip_hostname_verification`

Data type: `Optional[Boolean]`

Whether to skip host name verification on HTTPS connections

Default value: `undef`

##### `skip_peer_verification`

Data type: `Optional[Boolean]`

Whether to skip peer verification on HTTPS connections

Default value: `undef`

### geoip::install

This class implements the installation stage of the module. It should not be called directly.

This will install or remove software packages listed in `geoip::packages`. When installing
package versions will be ensured as `geoip::package_ensure`.

### geoip::service

This class implements the service control stage of the module. It should not be called directly.

If `geoip::manage_service` enabled, an update service will be created fitting to the service
provider available on the node. Service name is configured with `geoip::service_name`.

### geoip::systemd::service

This class is creating a serivce unit for SystemD to update GeoIP databases.
The service is running the geoipupdate ones and retry as configured.

#### Parameters

The following parameters are available in the `geoip::systemd::service` class.

##### `restart`

Data type: `String`

update service retry behaviour

Default value: 'on-failure'

##### `restart_sec`

Data type: `String`

time to wait before retry

Default value: '5min'

### geoip::systemd::timer

This class will create a SystemD timer unit triggering the update service on
each wallclock timer.
