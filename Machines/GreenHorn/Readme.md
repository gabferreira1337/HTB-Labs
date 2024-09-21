1st nmap 10.10.10.

Directory enumeration feroxbuster —url http://greenhorn.htb/ -o dir_1.txt -k

FFUF = Fuzz Faster you full

Recursion  -> if you file immediately find files and folders. -recursion
-e .log
-t 100. = number of threads

Parameters

Fluff -w parameters.txt -u http://ffuf.me/cd/param/data\?FUZZ\=1

-mc 200, 204, 301, 302, 307, 401, 403, 405, 415

Filter based on the size -fs 669. = not show
Filter based on number of words -fw= 123

-ac = calibration, look for garbage, to not filter anything 

Rlwrap nc -nlvp 9001. = lstener to control reverse shell

Python3 -c ‘import pty;pty.spawn(“/bin/bash”) dummy shell

Ls -alls -al -> give permissions to everyone

Which python3 , check path and if installed

& to run in the background
python3 -m http.server 8888 -> deploy an http connection to download using wget in your machine the pdf file

Depix = depisctalized to et the password