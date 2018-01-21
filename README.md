# kodi

This role will install Kodi media player and configure it. This role only work
on Ubuntu for the moment.

The role will do the following tasks:

- Install Software Properties Common
- Add Kodi PPA
- Install Kodi
- Configure the sources used by Kodi
- Configure Kodi
- Configure the database informations if needed.

## Requirements

Ansible 2.4 required

If you want to store Kodi library in a MySQL/Mariadb database, you must install
and configure it following the Kodi Wiki page: http://kodi.wiki/view/MySQL/Setting_up_MySQL

## Role Variables

### System settings

The role will drop all the configurations in the directory /home/{{ kodi_sysuser }}/.kodi:

```yaml
kodi_sysuser: htpc
kodi_sysgroup: htpc
```

### GUI Settings

Theses settings are configured in the file guisettings.xml.

If you want to enable and configure the kodi webserver :

```yaml
kodi_webserver_enable: True

# Kodi webserver configuration
kodi_webserver_username: kodi
kodi_webserver_password: kodi
kodi_webserver_port: 8080
```

If you want to activate upnp and zereconf :

```yaml
kodi_zeroconf_enable: true
kodi_upnpserver_enable: true

```

If you want to activate scan of the video or music library on startup :

```yaml
kodi_videolibrary_updateonstartup: true
kodi_musiclibrary_updateonstartup: true
```

If you want to specify a audio device as passthroughdevice, you must determine
which interface you want to use :

```
$ aplay -L
iec958:CARD=MID,DEV=0
    HDA Intel MID, ALC887 Digital
    IEC958 (S/PDIF) Digital Audio Output
```

and add the following variable :

```yaml
kodi_audiooutput_passthroughdevice: ALSA:iec958:CARD=MID,DEV=0
```

If you want to install addons from unknown sources in Kodi :

```yaml
kodi_addons_unknownsource_enable: true
```

Samba configuration :

```yaml
kodi_smb_winserver: ""
kodi_smb_workgroup: "WORKGROUP"
```
### MySQL settings

By default, Kodi stores the library informations in a SQL lite database. But if
you want to share this database between multiple Kodi installation, you must use
a MySQL/Mariadb database.

```yaml
kodi_usemysql: false

kodi_mysql:
  host: localhost
  port: 3306
  user: kodi
  pass: changeme
```

### Sources configuration

You can configure kodi sources in advance but you still must use Kodi interface
to finish the configuration.

Available Settings:

```yaml
kodi_sources_video: []
kodi_sources_music: []
kodi_sources_programs: []
kodi_sources_files: []
kodi_sources_pictures: []
```

Example :

```yaml
kodi_sources_video:
  - name: Movies
    path: /media/movies
    allowsharing: true
  - name: Tv Shows
    path: smb://192.168.1.1/tvshow
    allowsharing: false
```

Dependencies
------------

None.

Example Playbook
----------------

Here an example of a Playbook:

```yaml
- hosts: kodi
  become: true

  vars:
    kodi_sources_video:
      - name: Movies
        path: /media/movies
        allowsharing: true
      - name: Tv Shows
        path: smb://192.168.1.1/tvshow
        allowsharing: false

  roles:
     - role: Synehan.kodi
```

License
-------

BSD
