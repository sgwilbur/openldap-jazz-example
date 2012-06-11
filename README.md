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

 The following repository has the basic files to perform a simple configuration of OpenLDAP for use in testing.

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
entries in the ldif files used.well
  
See the slapd.conf file:

    ...
    #######################################################################
    # ldbm and/or bdb database definitions
    #######################################################################

    database	bdb
    suffix		"dc=example,dc=com"
    rootdn		"cn=ldapadmin,dc=example,dc=com"
    # Cleartext passwords, especially for the rootdn, should
    # be avoided.  See slappasswd(8) and slapd.conf(5) for details.
    # Use of strong authentication encouraged.
    rootpw		secret
    # rootpw		{crypt}ijFYNcSNctBYg  
    ...

=== 00 - Base - Adds the top level oraganizational units(ou) for users and groups
 
    [virtuser@clm2012 ldifs]$ ldapadd -x -D cn=ldapadmin,dc=example,dc=com -w secret -f 00-base.ldif
    adding new entry "dc=example,dc=com"

    adding new entry "ou=users,dc=example,dc=com"

    adding new entry "ou=groups,dc=example,dc=com"

 
=== 01 - Users - Add a set of users
 
    [virtuser@clm2012 ldifs]$ ldapadd -x -D cn=ldapadmin,dc=example,dc=com -w secret -f 01-users.ldif
    adding new entry "uid=jazzadmin,ou=users,dc=example,dc=com"

    adding new entry "uid=al,ou=users,dc=example,dc=com"

    adding new entry "uid=bob,ou=users,dc=example,dc=com"

    adding new entry "uid=build,ou=users,dc=example,dc=com"

    adding new entry "uid=curtis,ou=users,dc=example,dc=com"

    adding new entry "uid=deb,ou=users,dc=example,dc=com"

    adding new entry "uid=marco,ou=users,dc=example,dc=com"

    adding new entry "uid=rebecca,ou=users,dc=example,dc=com"

    adding new entry "uid=sally,ou=users,dc=example,dc=com"

    adding new entry "uid=tammy,ou=users,dc=example,dc=com"

    adding new entry "uid=tanuj,ou=users,dc=example,dc=com"

    adding new entry "uid=ursala,ou=users,dc=example,dc=com"

    adding new entry "uid=ramadmin,ou=users,dc=example,dc=com"

    adding new entry "uid=bfadmin,ou=users,dc=example,dc=com"

 
=== 02 - Groups + Members - 
 
    [virtuser@clm2012 ldifs]$ ldapadd -x -D cn=ldapadmin,dc=example,dc=com -w secret -f 02-groups.ldif
    adding new entry "cn=jazzadmins,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzdwadmins,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzprojectadmins,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzusers,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzguests,ou=groups,dc=example,dc=com"

    adding new entry "cn=ramusers,ou=groups,dc=example,dc=com"

 
 
 
##  Other examples ##
 
OpenLDAP sample commands: http://www.zytrax.com/books/ldap/ch5/ 
 
Setting a password for a user:
 
ldappasswd -x -v -S -D cn=ldapadmin,dc=example,dc=com -w *** cn=user1,ou=users,dc=example,dc=com
 