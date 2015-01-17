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

TODO - A description of the settable variables for this role should go here, including any variables that are in 
defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables 
that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as 
well.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for 
users too:
         
    - hosts: servers
      sudo: yes
    
      roles:
          - { role: ansible-nginx-uwsgi-supervisor, 
              app_name: app, 
              nginx_hostname: app.domain.net,
              uwsgi_port: 8080, 
              uwsgi_app_executable: "app.build:make_wsgi_app()" }

License
-------

BSD

Author Information
------------------

Dorian Pula
email: dorian.pula at amber-penguin.software.ca
www: http://amber-penguin-software.ca

This role is a spin-off of the technology developed for the Rookeries project: 
http://rookeries.amber-penguin-software.ca/
