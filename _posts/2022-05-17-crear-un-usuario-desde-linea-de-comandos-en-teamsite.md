---
title: "Crear un usuario desde línea de comandos en TeamSite"
header:
  image: /images/2022-05-17-crear-un-usuario-desde-linea-de-comandos-en-teamsite/crear-un-usuario-desde-linea-de-comandos-en-teamsite.png
  og_image: /images/2022-05-17-crear-un-usuario-desde-linea-de-comandos-en-teamsite/crear-un-usuario-desde-linea-de-comandos-en-teamsite.png
tags:
  - OpenText
  - OpenText TeamSite 
  - TeamSite
last_modified_at: 2022-05-17T20:06:36-09:00
---

Habitualmente utilizamos TeamSite en conjunción con un LDAP u OpenText Directory Service (OTDS). Sin embargo, 
en ocasiones puede sernos útil utilizar los usuarios disponibles en el sistema operativo. Por eso, en este artículo
vamos a crear un usuario desde línea de comandos en TeamSite.

## Crear el usuario en el sistema operativo

Vamos a utilizar el sistema operativo como fuente de nuestros usuarios, en lugar de utilizar 
un LDAP u  OpenText Directory Services (OTDS). Esto implica que tendremos que crear el usuario en el sistema operativo (Linux). 

Utilizaremos `adduser` para crear el usuario y `passwd` para asignar la contraseña al nuevo usuario

```shell
# adduser joaquin
# passwd joaquin
```

## Crear el usuario en TeamSite

A continuación, nos movemos a la carpeta donde se encuentran los **Command Line Tools (CLT)** de TeamSite, e.g. `/usr/Interwoven/TeamSite/bin` 

```shell
# cd /usr/Interwoven/TeamSite/bin
```

Despues, creamos  el usuario con ayuda del comando `iwuseradm`:

```shell
# ./iwuseradm add-user joaquin
```

Más tarde, definimos nuestras preferencias para ese usuario:

```shell
# ./iwuseradm set-master joaquin true
# ./iwuser -modify -user joaquin -ga iwEveryone
# ./iwuseradm set-preferred-ui joaquin estudio
# ./iwuseradm set-email joaquin joaquin@joaquinonsoft.com
```

Donde: 

   - `iwuseradm set-master <user> true|false` define el valor del atributo `master` para el usuario `<user>`
   - `iwuser -modify -user <user> -ga <group>` añade el usuario `<user>` al grupo `<group>`
   - `iwuseradm set-preferred-ui <user> ccpro|ccpro_only|ccstd|ccstd_only|estudio|none` define la interface de usuario para el usuario `<user>`
   - `iwuseradm set-email <user> <mail>` especifíca el correo electrónico `<mail>` para el usuario `<user>`

Finalmente,  podemos comprobar que nuestro usuario se ha creado y  esta correctamente configurado:

```shell
# ./iwuseradm list-users -t
tsadmin ""      ""      "tsadmin@opentext.com"  14#7    TRUE
tseditor        ""      ""      "tseditor@opentext.com" 14#7    FALSE
tsreviewer      ""      ""      "tsreviewer@opentext.com"       14#7    FALSE
demouser        ""      ""      "demouser@opentext.com" 14#7    FALSE
tsservice       ""      ""      "tsservice@opentext.com"        14#7    TRUE
joaquin ""      ""      "joaquin@joaquinonsoft.com"      210#7   TRUE
```

## Gestión de usarios en el interface de usuario de TeamSite

Desde el menú `Go to` de TeamSite podemos acceder a `Administration`. Una vez que en hemos accedido a la interface de administración 
tenemos que hacer clic sobre el menú `User Management` y podremos  ver los usuarios dados de alta en el sistema.
 
![Editar un usuario en TeamSite](/images/2022-05-17-crear-un-usuario-desde-linea-de-comandos-en-teamsite/editar-un-usuario-en-teamsite.png)

