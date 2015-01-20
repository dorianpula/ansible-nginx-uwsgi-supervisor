ansible-nginx-uwsgi-supervisor
==============================

An Ansible role to setup and manage a UWSGI app via supervisor, and served up on a NGINX web server.  The goal of this
role is to make deployment of WSGI app as a simple and sane as possible.  Additionally the role provides sane defaults
for logging and folder structure setup.

Requirements
------------

- aptitude or python-apt (required by apt tasks)
- python > 2.5 (required by ini_file tasks)

This role is designed to work against a modern Ubuntu system.  (Tested on Ubuntu 13.10 and 14.04)  It should 
theoretically work on older versions of Ubuntu or Debian based systems.

Role Variables
--------------

### Default Variables

TODO - Explanation

# Web root paths + app name + home
app_name: app
web_root_path: /srv/www
web_server_group: www-data
app_home: "{{ web_root_path }}/{{ app_name }}_webapp"

# NGINX
nginx_hostname: localhost
nginx_app_static: "{{ app_home }}/{{ app_name }}/static/"

# UWSGI
uwsgi_port: 8001
uwsgi_app_executable: "app:make_wsgi_app()"


### Main (Internal) Variables

The following variables are part of the internals of the role.  However if you really want to, you can tweak them to 
work with your setup:

- web_user: The non-root user who is allowed to control web + app servers on the target machine.
    (Default: current user)
- virtualenv_root: The common root directory of Python virtual environments associated with running the UWSGI app + 
    server.  (Default: /srv/www/virtualenvs/)
- uwsgi_venv: The virtual environment where UWSGI is installed.  (Default: virtualenv_root/uwsgi)
- app_venv: The virtual environment where the dependencies of the WSGI app is installed.  
    (Default: virtualenv_root/app_name)
- nginx_app_conf: The filename of the NGINX configuration for the app.  (Default: app_name_uwsgi_nginx.conf)
- supervisor_app_config: The filename of the supervisor configuration for the app.  (Default: app_name_supervisor.conf)
- uwsgi_config: The path to the UWSGI configurations.  (Default: /srv/www/config/uwsgi)
- uwsgi_app_ini: The filename of the UWSGI INI configuration for the app. (Default: app_name_uwsgi.ini)
- uwsgi_service_name: The name of the UWSGI setup for the app according to supervisor.  (Default: app_name_uwsgi)

Example Playbook
----------------

The simplest way to include the role in your playbook is to copy the below configuration.  Remember to modify the 
app_name, nginx_hostname and uwsgi_app_executable parameters especially.
         
    - hosts: servers
      sudo: yes
    
      roles:
          - { role: ansible-nginx-uwsgi-supervisor, 
              app_name: app, 
              nginx_hostname: app.domain.net,
              uwsgi_port: 8080, 
              uwsgi_app_executable: "app.build:make_wsgi_app()" }
              
Default File Structure
----------------------

By default the role will organize files in the following directory structure:

    /srv/www
    ├── config
    │   ├── nginx -> /etc/nginx
    │   ├── supervisor -> /etc/supervisor
    │   └── uwsgi
    ├── logs
    │   ├── nginx -> /var/log/nginx
    │   ├── supervisor -> /var/log/supervisor
    │   └── uwsgi
    ├── app_webapp
    │   ├── requirements.txt
    │   └── app
    └── virtualenvs
        ├── app
        └── uwsgi

License
-------

BSD

Author Information
------------------

Dorian Pula
- email: dorian.pula at amber-penguin.software.ca
- www: http://amber-penguin-software.ca

This role is a spin-off of the technology developed for the [Rookeries project]: 
http://rookeries.amber-penguin-software.ca/
