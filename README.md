win_mssqlserver Project
=========

This project provides playbooks to help with administering MS SQL Server

Requirements
------------

This project contains these playbooks:
1. site.yml - This playbook is used to install MS SQL Server on Windows.  It calls the role (role_win_mssqlserver)

2. mssql_dbcreate.yml - This playbook is used to create a DB in MS SQL Server on Windows.  It calls the role (role_win_mssqldb)

3. rhel_mssqlinstall.yml - This playbook is used to install MS SQL Server on RHEL.  It calls the RHEL system role (microsoft.sql.server)

4. win_test_playbook.yml - This playbook is just used to test win_command and win_shell on Windows.

win_mssqlserver Variables
--------------
# Variables needed for MS SQL Server on Windows
# hostname or IP can be entered from AAP survey
    survey_vm_hostname    
# VARS for connecting to Windows VM via winrm
    ansible_user: '{{ windowsuser }}'   
    ansible_password: '{{ windowspassword }}'  
    ansible_connection: winrm
    ansible_port: 5985
    ansible_winrm_transport: ntlm
    ansible_winrm_server_cert_validation: ignore
    win_dom_user: '{{ domainuser }}'   #windows AD domain admin user
    win_dom_pw: '{{ domainpassword }}'
# DB users for MS SQL Server 
    db_agent_user: dbagentuser
    db_engine_user: dbengineuser
    db_pw: eP@ssw0rd!157#
# MAXDOP - setting for max degree of parallelism 
    db_cpu_for_maxdop: 2
# SA password
    db_sapwd: Ansible01
# URL to download MS SQL SERVER 2019 install file
    inst_url: 'https://go.microsoft.com/fwlink/p/?linkid=866662'
# Path where MS SQL SERVER 2019 install file will be placed and used for install
    inst_file: 'C:\Temp\SQL2019-SSEI-Dev.exe'
# DB install type 
    inst_db_type: SQLServer
# file used to install MS SQL Server from command line
    inst_config_file: 'C:\Temp\SQLServer.ini'
# directories used in SQL Server install
    dir_sqlbackups: 'C:\SQLBackups'
    dir_sqldata: 'C:\SQLData'
    dir_mssql: 'C:\MSSQL'
    dir_sqllogs: 'C:\SQLLogs'
    dir_tempdb: 'C:\TempDB'
    dir_temp: 'C:\Temp'
# name of DBTools DB
    var_db_name: DBATools
# local DBA group created on windows server
    dba_local_group: 'DBA Local Admins'
# file used to create DB from command line 
    inst_dbcreate_file: 'C:\Temp\DBCreate.sql'
# array containing directories to be created for SQL Server install
    db_dirs:
      - '{{ dir_sqlbackups }}'
      - '{{ dir_sqldata }}'
      - '{{ dir_mssql }}'
      - '{{ dir_sqllogs }}'
      - '{{ dir_tempdb }}'
      - '{{ dir_temp }}'
# array containing DB users
    db_admins:
      - '{{ db_agent_user }}'
      - '{{ db_engine_user }}'

# Variables needed for MS SQL Server on RHEL

# accept license agreements
    mssql_accept_microsoft_odbc_driver_17_for_sql_server_eula: true
    mssql_accept_microsoft_cli_utilities_for_sql_server_eula: true
    mssql_accept_microsoft_sql_server_standard_eula: true
# set MQ SQL Server version
    mssql_version: 2019
# SA password
    mssql_password: "p@55w0rD"
# select the mssql server edition you want to install
    mssql_edition: Developer

Dependencies
------------

site.yml - role_win_mssqlserver
mssql_dbcreate.yml - role_win_mssqldb
rhel_mssqlinstall.yml - microsoft.sql.server

win_mssqlserver Project Playbook
----------------
- name: Project win_mssqlserver
  hosts: '{{ survey_vm_hostname }}'
  gather_facts: no

  vars:
    <list all required vars>

tasks:
  - name: Setup MS SQL Server on "{{ survey_vm_hostname }}"
    include_role:
      name: role_win_mssqlserver
      apply:
        tags:
          - win_mssqlserver
    tags:
      - always

License
-------

GPL

Author Information
------------------

Norman Owens
Norman.Owens@redhat.com