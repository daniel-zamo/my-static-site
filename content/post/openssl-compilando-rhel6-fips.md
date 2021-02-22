+++ 
title = "Compilando Openssl con soporte fips"
date = 2021-02-21T01:24:42+01:00
draft = false
author = "Daniel Oscar Zamo"
+++

## Compilación sobre RHEL 6: openssl + fips

En este artículo se compila openssl-1.0.2u con soporte del modulo openssl-fips-2.0.16 en ***sobre un RHEL Server release 6.3 (Santiago)***. Debería ser valido para sus derivados (CentOS, Nethserver, Scientific Linux, etc. En la misma versión).

### Compilación de openssl + fips

> Condiciones cumplidas
>
> - Distribución: RHEL 6
> - Versión de openssl fuente: openssl-1.0.2u ( fichero ya ubicado en `/usr/local/src/openssl-1.0.2u.tar.gz`)
> - Versión modulo fips: openssl-fips-2.0.16 ( fichero ya ubicado en `/usr/local/src/openssl-fips-2.0.16.tar.gz`)
> - Directorio donde se instalara: `/usr/local/openssl-1.0.2u`

#### Notas previa (RHEL 6: openssl + fips)

> El proceso que se indica a continuación se ha realizado sobre las VM indicadas anteriormente en la tabla (RHEL 6). Estas compilaciones e instalaciones fueron validadas con el desarrollador y revisadas su correcto funcionamiento durante al menos una semana posterior a la compilación e instalación efectiva. 

### Compilación

A continuación se captura la sesión de trabajo para compilar e instalar openssl.

Requerimientos cumplidos: El código fuente a compilar ya se encuentra en `/usr/local/src`

```
cd /usr/local/src/
mkdir -p /usr/local/openssl-1.0.2u
# Compilar e Instalar fips
tar xzf openssl-fips-2.0.16.tar.gz
cd openssl-fips-2.0.16
./config  --openssldir=/usr/local/openssl-1.0.2u/fips-2.0.16
make 
make install
#####################################
# Compilar openssl con el modulo fips
cd /usr/local/src/
tar xzf openssl-1.0.2u.tar.gz
cd openssl-1.0.2u
./config fips --with-fipsdir=/usr/local/openssl-1.0.2u/fips-2.0.16 \ 
  --openssldir=/usr/local/openssl-1.0.2u
make depend        
make
# En este punto se podría de realizar el apartado "Verificar"
```

### Instalar 

```
# (Si "Verificar" no da error (Item: Verificar (Opcional - recomendado)), entonces ejecutar el "make install" (Warning/Advertencia: revisar el PATH, el "make install" debe ser realizado en el mismo PATH donde se hizo la compilación (Comando 'make' del Item: Compilación (anterior comentado)))
# cd /usr/local/src/openssl-1.0.2u # recordar de ejecutar en el correcto PATH donde se compilo
make install                       # El openssl se instalara en /usr/local/openssl-1.0.2u
#####################################
mv -v /usr/local/ssl{,.$(date +%Y%m%d)}          # Hago backup del ssl actual
ln -s /usr/local/openssl-1.0.2u /usr/local/ssl  # Creo link al recien instalado
```

Si todo a ido bien el openssl queda instalado y funcional. Basta con ejecutar `openssl version -a`. La salida de este comando será similar a la mostrada [aquí](#m_-2091644685050541818_).

### Verificar (Opcional - recomendado - leer antes)

```
my_path_build=$(pwd)    # Almaceno el path donde compile y estoy posicionado
cd apps                 # No realizo aun el "make install". Primero lo pruebo
./openssl version -a    # Debería indicar la versión y con soporte de modulo fips
cd ${my_path_build}
```

![img][openssl.fips]

[openssl.fips]:/images/openssl.fips.png "Comando openssl version -a"

## Compilación sobre RHEL 5: openssl 1.1.1j con PERL 5.30.2

Para poder compilar la versión 1.1.1(X) de openssl, es necesario previamente instalar una versión mas actual de PERL. La compilación/instalación a continuación es lo que se realiza.