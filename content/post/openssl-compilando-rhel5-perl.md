+++
title = "Openssl sobre RHEL5 - Perl"
description = "Compilar openssl - perl"
date = 2019-09-28T02:13:50Z
author = "Daniel Oscar Zamo"
+++

## Compilando openssl sobre RHEL5, con Perl actual

Este post expone como cmpilar openssl en una distribución Linux. Previamente se compilando e instala Perl.

### Supuestos iniciales

> Los siguientes son supuestos cumplidos inicialmente. Estos son:

> - Las tareas realizadas son ejecutadas sobre una distribucón de Linux basado en Red Hat Enterprise Linux 5 (RHEL 5). Pero estas mismas deberían de funcionar ajustando apropiadamente.
> - La descarga e instalación de los soft se realizara desde el `PATH=/usr/local/usr`

### Compilar e instalar Perl

``` 
cd /usr/local/src
wget https://www.cpan.org/src/5.0/perl-5.32.1.tar.gz
tar xzf perl-5.30.2.tar.gz 
cd perl-5.30.cd /usr/local/src
tar xzf perl-5.30.2.tar.gz 
cd perl-5.30.22
./Configure -Dprefix=/opt/perl/perl-5.30.2 -de
make
make test
make install
```
### Compilar e instalar Openssl

```
export PERL=/opt/perl/perl-5.30.2/bin/perl
cd /usr/local/src
wget https://www.openssl.org/source/openssl-1.1.1j.tar.gz
tar xzf openssl-1.1.1j.tar.gz 
cd openssl-1.1.1j
./config --openssldir=/usr/local/openssl-1.1.1j --prefix=/usr/local/openssl-1.1.1j
make
make test
make install
```
### Configurar
```
cd /usr/local/
echo "/usr/local/ssl/lib" > /etc/ld.so.conf.d/openssl.conf
ldconfig -v
openssl version
openssl version -a
ln -s /usr/local/openssl-1.1.1j /usr/local/ssl
which openssl
openssl version all
make
make test
make install
cd /usr/local/
ln -s /usr/local/openssl-1.1.1j /usr/local/ssl
openssl version -a
openssl 
find /usr/lib* libcrypto.so.1.1
find /usr/lib* -name 'libcrypto.so.1.1'
find /usr/local/lib* -name 'libcrypto.so.1.1'
openssl 
cd /usr/local/lib/
ls -l libssl.so.1.1
#find /usr/local/ssl -name libssl.so.1.1
ls /usr/local/ssl/lib/
#vi /etc/ld.so.conf.d/openssl.conf
ldconfig 
openssl version -a
``` 
