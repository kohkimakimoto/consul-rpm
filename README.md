# RPM Spec for Consul

This repository is a fork of [tomhillable/consul-rpm](https://github.com/tomhillable/consul-rpm).
But it has been heavily modified.

## Usage

Just running vagrant up.

```
vagrant up
```

see the `build` directory

```
cd build
```

## Supportting platform.

* centos5
* centos6

## directory structure

* Binary: `/usr/bin/consul`
* ConfigFile: `/etc/consul.json`
* ConfigDir: `/etc/consul.d/`
* LogDir: `/var/log/consul/`
* Sysconfig: `/etc/sysconfig/consul`
* WebUI: `/usr/share/consul/`

## built rpm packages

https://bintray.com/kohkimakimoto/rpm/consul/view
