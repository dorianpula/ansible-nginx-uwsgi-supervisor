Ansible nginx-uwsgi-supervisor
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

Example Playbook
----------------

The simplest way to include the role in your playbook is to copy the below configuration.  Remember to modify the 
app_name, app_nginx_hostname and app_uwsgi_executable parameters especially.
         
    - hosts: servers
      sudo: yes
      roles:
          - { role: nginx-uwsgi-supervisor,
              app_name: app,
              app_nginx_hostname: app.domain.net,
              app_uwsgi_port: 8080,
              app_uwsgi_executable: "app.build:make_wsgi_app()" }
              
The role itself is very configurable.  For exaple, if you prefer the location of the web application to refer to the 
domain name rather than the default root path,  then simply override the app_root_path variable with something like:

    app_rooth_path: "{{ web_root_path }}/{{ app_nginx_hostname }}"

A comprehensive example can be found in the [Ansible rookeries role] 
(https://bitbucket.org/dorianpula/ansible-rookeries) that uses this role as a base to deploy a Flask-based webapp.

Role Variables
--------------

There are a few crucial variables that you need to configure for your particular WSGI powered app.  They are broken up
into sections and described below:

### App Settings

- app_name:
    - The name of WSGI app to manage via this role.
    - Used as a prefix through out the role.
- app_root_path:
    - The path to the root folder of the app.
    - Default: web_root_path/app_name_webapp

### NGINX

- app_nginx_hostname:
    - The DNS hostname that the application serves.
    - Default: localhost
- app_nginx_app_static: 
    - The path to the static elements of the site (templates, CSS, images, etc.)
    - Default: app_root_path/app_name/static/

### UWSGI

- app_uwsgi_port: 
    - The port number that the WSGI runs on and accepts requests forwarded from NGINX.
    - Default: 8001
- app_uwsgi_executable:
    - The method executed by UWSGI to create a WSGI running app.  Usually a WSGI factory method or module in the app.
    - Default: app:make_wsgi_app()
- app_uwsgi_envs:
    - (Optional) A dictionary of environment variables to pass into an app.
    - Default: empty dictionary

### General Web:

These variables are not as crucial to configure.  They do give good defaults for configuring the system in a consistent,
POSIX/LSB-friendly and user-friendly manner.  See the section on Default File Structure for more details.

- web_root_path:
    - The root of the entire web app structure include configuration and logging.
    - Default: /srv/www
- web_server_group:
    - The user group responsible for starting, stopping and managing the web and app servers on the target machine. 
    - Default: www-data

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

Internal Variables
------------------

The following variables are part of the internals of the role.  However if you really want to, you can tweak them to 
work with your setup:

- web_user: 
    - The non-root user who is allowed to control web + app servers on the target machine.
    - Default: current user
- virtualenv_root_path: 
    - The common root directory of Python virtual environments associated with running the 
    UWSGI app + server.  
    - Default: /srv/www/virtualenvs/
- uwsgi_venv:
    - The virtual environment where UWSGI is installed.
    - Default: virtualenv_root/uwsgi
- app_venv: 
    - The virtual environment where the dependencies of the WSGI app is installed.  
    - Default: virtualenv_root/app_name
- nginx_app_conf: 
    - The filename of the NGINX configuration for the app.
    - Default: app_name_uwsgi_nginx.conf
- supervisor_app_config:
    - The filename of the supervisor configuration for the app.  
    - Default: app_name_supervisor.conf
- uwsgi_config_path: 
    - The path to the UWSGI configurations.
    - Default: /srv/www/config/uwsgi
- uwsgi_app_ini: 
    - The filename of the UWSGI INI configuration for the app. 
    - Default: app_name_uwsgi.ini
- uwsgi_service_name: 
    - The name of the UWSGI setup for the app according to supervisor.  
    - Default: app_name_uwsgi

License
-------

BSD

Author Information
------------------

Dorian Pula

- twitter: @dorianpula
- email: dorian.pula at amber-penguin.software.ca
- www: http://amber-penguin-software.ca

This role is a spin-off of the technology developed for the [Rookeries project] (http://rookeries.org/)


Repositories
------------

- Main: https://bitbucket.org/dorianpula/ansible-nginx-uwsgi-supervisor
    - All development and issues are worked on this repo.
- Clone: https://github.com/dorianpula/ansible-nginx-uwsgi-supervisor
    - A clone to work with Ansible Galaxy and Github
