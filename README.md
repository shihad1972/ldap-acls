ldap-acls
=========

Setup some ACL's on the LDAP directory

Requirements
------------

 A working installation of openldap is required for this role.

Role Variables
--------------

  admin_dn: rdn of the admin user of the directory. Defaults to manager
  admin_rdn: rdn of the admins container in the directory. Defaults to admins
  domain_dn: base dn of the directory. Should be in the format, e.g. dc=example,dc=com.
  admin_password: Password for the admin user of the directory
  admin_members: list of DN's of admin users to add to the admin group. If this is not defined, none of the above are required.
  ldap_db_number: Number of the cn=config style database to add the acl's to.
  ldap_dbtype: Type of database the directory uses, e.g. hdb or mdb. Defaults to hdb
  ldap_acls: List of acls to apply to the directory. This will _remove_ all previous ACL's. They should be numbered.
  admin_group: Name of the group of admins. Defaults to diradmins


Dependencies
------------

 There are no dependencies

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      vars:
        ldap_db_number: 1
        ldap_dbtype: mdb
        domain_dn: "dc=example,dc=com"
        ldap_acls:
            - "{0}to attrs=userPassword,shadowLastChange,userPKCS12 by self write by anonymous auth by * none"
            - "{1}to dn.subtree=\"{{ domain_dn }}\" by group/groupOfNames/member.exact=\"cn={{ admin_group }},ou={{ admin_rdn }},{{ domain_dn }}\" write by users read"

      roles:
         - { role: ldap-acls }

License
-------

GPLv3

Author Information
------------------

Iain M Conochie <iain@thargoid.co.uk>