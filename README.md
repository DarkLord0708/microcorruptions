# microcorruptions

## New Orleans
* started similar to the tutorial , wanted a password
* Looking at code, found that a password was being created by ```<create_password>``` and was being checked by ```<check_password>```
* Now, ```<create_password>``` moved something from 2400 in memory to r15
* I suspected this something at 2400 might be the password, it was a 7 character string "1!0+hnI" and was being moved byte wise 8 times
  * 7+1(for null character) = 8
* I tried entering the suspected password and it worked<br><br>
All that I nuderstood was, the ```<check_password>``` was checking the input string to the password and icreasing r14 and checking if its 8 and if its 8<br>
then we got the access i.e. its checking if the entered password is equal to the stored string or not.<br><br>

## Sydney
