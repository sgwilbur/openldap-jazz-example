# Example OpenLDAP directory #

This example is primarily meant for use with Rational products and
developed to directly support use with Jazz based products. However
the general techniques and files and be adjusted to fit any similar
needs.
 
 This sample sets up a dummy directory with a few users:
  
  -dc=example,dc=com
  -- ou=groups
  --- JazzAdmins
  --- JazzProjectAdmins
  --- JazzDWAdmins
  --- JazzUsers
  --- JazzGuests
  --- RAMAdmins
  --- RAMUsers
  -- ou=users
  --- user1
  --- ramadmin
  --- bfadmin

## Contents ##

.
  |-- README.md
  |-- etc_openldap
  |   `-- slapd.conf
  `-- ldifs
    |-- 00-base.ldif
    |-- 01-users.ldif
    |-- 02-groups.ldif
    |-- 03-example-mod-jazzadmins.ldif
    |-- 04-mod-jazzadmins-02.ldif
    `-- 05-mod-ramadmin.ldif


## Setup ##
 
  In order to configure your LDAP repository you need to first configure the slapd.conf file to match your desired
  directory base. The examples are for dc=example,dc=com, if you change this be sure to update all the corresponding
  entries in the ldif files used.
 
 00 - Base - Adds the top level oraganizational units(ou) for users and groups
 
ldapadd -x -D cn=ldapadmin,dc=example,dc=com -w secret -f 00-base.ldif
 
 01 - Users - Add a set of users
 
ldapadd -x -D cn=ldapadmin,dc=example,dc=com -w *** -f 01-users.ldif
 
 02 - Groups + Members - 
 
## Examples ##
 
OpenLDAP sample commands: http://www.zytrax.com/books/ldap/ch5/ 
 
Setting a password for a user:
 
ldappasswd -x -v -S -D cn=ldapadmin,dc=example,dc=com -w *** cn=user1,ou=users,dc=example,dc=com
 