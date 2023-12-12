# microcorruptions
(will keep updating this to add more levels)

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
this time there was no password in the memory but I could bypass by ```let pc = 44ae``` but this doesn't solve the problem<br>
we have to get the correct password because ```<check_password>``` changes r14 if password is wrong and we have to retain r14<br>
some googling helped me to know that MSP430 uses "little endian", so it reads two bytes at a time and reads backwards<br>
so, looking at all the comparisons in ```<check_password>```, I tried "3767715266793a66" as hexcode as password and it worked<br><br>

## Hanoi
```<test_password_valid>``` is the function that is checking the password, but I couldn't see anything there so I went to ```<login>``` function<br>
here, we get "Access granted" if "0xfs" is stroed at 2410 in memory<br>
Now, 2400 and 2410 are for storing the password to access 2410 I have to fill 16 characters of 2400 and addfs in the end to get to 2410<br>
this solves the problem and unlocks the door.<br><br>

## Cusco
I couldn't find a compare instruction to do whatever with it, so looked at the code more and running a 16 character<br>
password I found that at the end the pointer was pointing at the end of our password, i.e. the 17th character<br>
Also, there was a ```<unlock_door>``` function saved at 4446. So, I had to make the pointer point at 4446 somehow to get through<br>
So, after the 16 character password I added 4644 (because of the little endian) at the end, in hexcode, and this works.<br><br>

## Reykjavik
some encoding was being done by ```<enc>``` and finally 2400 was being called, in the debugger I used<br>
```read 2400 500``` to see all the lines of instructions it had, copied the hexcode and put it into the disassembler (had to google the disassembler part)<br>
then I could see all the instructions this was running and found, ```cmp #0x6b80 -0x24(r4)``` after entering the password r4 at 43fe in memory<br>
and password is at 43da which is 24 bytes before r4. So, basically its checking if the password is "6b80" in hex, which might be the encoded password<br>
enetring 6b80 itself would work because the encoding has been done before hand and doesn't affect the password we eneter<br>
keeping in mind the little endian, eneterd "806b" in hex and this worked<br><br>
