Ansible Role: oracle-download-patches
=====================================

Download patches from My Oracle Support directly to the server.

This role uses [getMOSPatch.sh](https://github.com/MarisElsins/TOOLS/blob/master/Shell/getMOSPatch.sh) script, by Maris Elsins.


Supported OS:
-------------
* RedHat
* CentOS
* OracleLinux

Requirements
------------

My Oracle Support credentials.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    # My Oracle Support Userid
    oracle_download_patches_mos_user:

    # My Oracle Support Password
    oracle_download_patches_mos_pass:

    # target directory path for the downloaded patches
    oracle_download_patches_stage_directory: /tmp

    # whether or not create 'oracle_download_patches_stage_directory' directory
    oracle_download_patches_create_stage_directory: true

    # list of patches to download
    oracle_download_patches_list: []

    # for example:
    #oracle_download_patches_list:
    #  - 28689165
    #  - 6880880

    # regular expression to filter the filename
    # Typically this can be used if the same patch is available for multiple releases of software and you know which one you need.
    #  I.e. '.121.' would be useful for Oracle Database 12c (R1)
    oracle_download_patches_regexp:

    # for example:
    #oracle_download_patches_regexp: '.112.'

    # List of platform codes used to select files to download
    #   See `vars\oracle_download_patches_platform_map` variable for a complete list of supported platforms
    oracle_download_patches_platforms:
      - "Linux x86-64"

    #  - "Linux x86"
    #  - "HP-UX PA-RISC (32-bit)"
    #  - "HP-UX PA-RISC (64-bit)"
    #  - "IBM AIX on POWER Systems (32-bit)"
    #  - "IBM AIX on POWER Systems (64-bit)"

    # getMOSPatch.sh destination directory
    oracle_download_patches_getmospatch_directory: /tmp/getMOSPatch

    # display getMOSPatch.sh output
    oracle_download_patches_display_getmospatch_output: true


Dependencies
------------

None.

Example Playbook
----------------

    - name: Download patches from My Oracle Support directly to the server
      hosts: localhost
      gather_facts: false
      become: true
      become_user: '{{ oracle_user }}'

      vars_prompt:
        - name: oracle_download_patches_mos_user
          prompt: "Oracle Support Userid"
          private: no
        - name: oracle_download_patches_mos_pass
          prompt: "Oracle Support Password"
          private: yes

      tasks:

      - import_role:
          name: oracle
        tags:
          - always

      - import_role:
          name: oracle-download-patches
        vars:
          oracle_download_patches_stage_directory: /path/to/stage/directory/patches
          oracle_download_patches_list:
            - 28689165
        tags:
          - oracle_download_patches

      - import_role:
          name: oracle-download-patches
        vars:
          oracle_download_patches_stage_directory: /path/to/stage/directory/opatch
          oracle_download_patches_list:
            - 6880880
          oracle_download_patches_regexp: '.112.'  # download 11.2 opatch only
        tags:
          - oracle_download_patches
     

Inside `vars/main.yml` or `group_vars/..` or `host_vars/..`:


    #---------------------------------------------------
    # overrides role 'oracle-download-patches' variables
    #---------------------------------------------------

    # target directory path for the downloaded patches
    oracle_download_patches_stage_directory: /path/to/stage/directory/patches

    # ... etc ...


License
-------

GPLv3 - GNU General Public License v3.0

Author Information
------------------

This role was created in 2018 by [Krzysztof Lewandowski](mailto:Krzysztof.Lewandowski@fastmail.fm).

