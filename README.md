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
     --- al
     --- bob
     --- build
     --- curtis
     --- deb
     --- marco
     --- rebecca
     --- sally
     --- tammy
     --- tanuj
     --- ursala
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
entries in the ldif files used as well.
  
See the slapd.conf file:

    ...
    #######################################################################
    # ldbm and/or bdb database definitions
    #######################################################################

    database	bdb
    suffix		"dc=example,dc=com"
    rootdn		"uid=ldapadmin,dc=example,dc=com"
    # Cleartext passwords, especially for the rootdn, should
    # be avoided.  See slappasswd(8) and slapd.conf(5) for details.
    # Use of strong authentication encouraged.
    rootpw		secret
    # rootpw		{crypt}ijFYNcSNctBYg  
    ...

=== 00 - Base - Adds the top level oraganizational units(ou) for users and groups
 
    [virtuser@clm2012 ldifs]$ ldapadd -x -D uid=ldapadmin,dc=example,dc=com -w secret -f 00-base.ldif
    adding new entry "dc=example,dc=com"

    adding new entry "ou=users,dc=example,dc=com"

    adding new entry "ou=groups,dc=example,dc=com"

 
=== 01 - Users - Add a set of users
 
    [virtuser@clm2012 ldifs]$ ldapadd -x -D uid=ldapadmin,dc=example,dc=com -w secret -f 01-users.ldif
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
 
    [virtuser@clm2012 ldifs]$ ldapadd -x -D uid=ldapadmin,dc=example,dc=com -w secret -f 02-groups.ldif
    adding new entry "cn=jazzadmins,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzdwadmins,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzprojectadmins,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzusers,ou=groups,dc=example,dc=com"

    adding new entry "cn=jazzguests,ou=groups,dc=example,dc=com"

    adding new entry "cn=ramusers,ou=groups,dc=example,dc=com"

 
## Verify your directory ##

Simple search with no filter, will dump all directory contents. This is useful but not the easiest to read, below are a few
more specific examples you can use to validate that your directory setup is valid.
 
    [virtuser@clm2012 ldifs]$ ldapsearch -x -h clm2012.admin.ws
    ...

Search for at the ou=users base for all iNetOrgPerson objects and return specific attributes, good search for validating your users are setup correctly.

    [virtuser@clm2012 ldifs]$ ldapsearch -x -h clm2012.admin.ws -b ou=users,dc=example,dc=com objectClass=InetOrgPerson uid mail cn

    # extended LDIF
    #
    # LDAPv3
    # base <ou=users,dc=example,dc=com> with scope subtree
    # filter: objectClass=InetOrgPerson
    # requesting: uid mail cn
    #

    # jazzadmin, users, example.com
    dn: uid=jazzadmin,ou=users,dc=example,dc=com
    uid:: amF6emFkbWluIA==
    cn: Jazz
    mail: jazzadmin@example.com

    # al, users, example.com
    dn: uid=al,ou=users,dc=example,dc=com
    uid: al
    cn: Al
    mail: al@jkebanking.net

    # bob, users, example.com
    dn: uid=bob,ou=users,dc=example,dc=com
    uid: bob
    cn: Bob
    mail: bob@jkebanking.net

    # build, users, example.com
    dn: uid=build,ou=users,dc=example,dc=com
    uid: build
    cn: Build
    mail: build@jkebanking.net

    # curtis, users, example.com
    dn: uid=curtis,ou=users,dc=example,dc=com
    uid: curtis
    cn: Curtis
    mail: curtis@jkebanking.net

    # deb, users, example.com
    dn: uid=deb,ou=users,dc=example,dc=com
    uid: deb
    cn: Deb
    mail: deb@jkebanking.net

    # marco, users, example.com
    dn: uid=marco,ou=users,dc=example,dc=com
    uid: marco
    cn: Marco
    mail: marco@jkebanking.net

    # rebecca, users, example.com
    dn: uid=rebecca,ou=users,dc=example,dc=com
    uid: rebecca
    cn: Rebecca
    mail: rebecca@jkebanking.net

    # sally, users, example.com
    dn: uid=sally,ou=users,dc=example,dc=com
    uid: sally
    cn: Sally
    mail: sally@jkebanking.net

    # tammy, users, example.com
    dn: uid=tammy,ou=users,dc=example,dc=com
    uid: tammy
    cn: Tammy
    mail: tammy@jkebanking.net

    # tanuj, users, example.com
    dn: uid=tanuj,ou=users,dc=example,dc=com
    uid: tanuj
    cn: Tanuj
    mail: tanuj@jkebanking.net

    # ursala, users, example.com
    dn: uid=ursala,ou=users,dc=example,dc=com
    uid: ursala
    cn: Ursala
    mail: ursala@jkebanking.net

    # ramadmin, users, example.com
    dn: uid=ramadmin,ou=users,dc=example,dc=com
    uid:: cmFtYWRtaW4g
    cn: RAM Admin
    mail: ramadmin@example.com

    # bfadmin, users, example.com
    dn: uid=bfadmin,ou=users,dc=example,dc=com
    uid:: YmZhZG1pbiA=
    cn: BF Admin
    mail: bfadmin@example.com

    # search result
    search: 2
    result: 0 Success

    # numResponses: 15
    # numEntries: 14


Search at ou=groups for GroupOfNames objects returning the member attribute, good for validating group memberships.

    [virtuser@clm2012 ldifs]$ ldapsearch -x -h clm2012.admin.ws -b ou=groups,dc=example,dc=com objectClass=GroupOfNames member
    # extended LDIF
    #
    # LDAPv3
    # base <ou=groups,dc=example,dc=com> with scope subtree
    # filter: objectClass=GroupOfNames
    # requesting: member
    #

    # jazzadmins, groups, example.com
    dn: cn=jazzadmins,ou=groups,dc=example,dc=com
    member: uid=jazzadmin,ou=users,dc=example,dc=com

    # jazzdwadmins, groups, example.com
    dn: cn=jazzdwadmins,ou=groups,dc=example,dc=com
    member: uid=jazzadmin,ou=users,dc=example,dc=com

    # jazzprojectadmins, groups, example.com
    dn: cn=jazzprojectadmins,ou=groups,dc=example,dc=com
    member: uid=jazzadmin,ou=users,dc=example,dc=com

    # jazzusers, groups, example.com
    dn: cn=jazzusers,ou=groups,dc=example,dc=com
    member: uid=al,ou=users,dc=example,dc=com
    member: uid=bob,ou=users,dc=example,dc=com
    member: uid=build,ou=users,dc=example,dc=com
    member: uid=curtis,ou=users,dc=example,dc=com
    member: uid=deb,ou=users,dc=example,dc=com
    member: uid=marco,ou=users,dc=example,dc=com
    member: uid=rebecca,ou=users,dc=example,dc=com
    member: uid=sally,ou=users,dc=example,dc=com
    member: uid=tammy,ou=users,dc=example,dc=com
    member: uid=tanuj,ou=users,dc=example,dc=com
    member: uid=ursala,ou=users,dc=example,dc=com

    # jazzguests, groups, example.com
    dn: cn=jazzguests,ou=groups,dc=example,dc=com
    member: uid=jazzadmin,ou=users,dc=example,dc=com

    # ramusers, groups, example.com
    dn: cn=ramusers,ou=groups,dc=example,dc=com
    member: uid=ramadmin,ou=users,dc=example,dc=com

    # search result
    search: 2
    result: 0 Success

    # numResponses: 7
    # numEntries: 6


 
##  Other examples ##
 
OpenLDAP sample commands: http://www.zytrax.com/books/ldap/ch5/ 
 
Setting a password for a user:
 
    ldappasswd -x -v -S -D uid=ldapadmin,dc=example,dc=com -w *** cn=jazzadmin,ou=users,dc=example,dc=com
 