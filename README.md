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
 
## Setup ##
 
 
 
 
## Examples ##
 
 
Setting a password for a user:
 
ldappasswd -x -v -S -D cn=ldapadmin,dc=example,dc=com -w *** cn=user1,ou=users,dc=example,dc=com
 