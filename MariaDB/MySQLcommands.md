## MariaDB
MariaDB is currently installed and running on my personal Raspberry Pi. MariaDB is the fork of MySQL. Using MySQL commands should work for MariaDB as well. These notes are for commands. If the command line does not suit, you can use the web interface phpmyadmin.

## Logging Into the Database
SSH into my Raspberry Pi then run the following command to get into MariaDB:

    mysql -u USER -p

Where `USER` is the user account that is associated with MariaDB. The default user is `root`. It will ask for the password. After entering the password successfully, you will see:

    MariaDB [(none)]> 

You're now logged in the database. To log out of MariaDB, you can use any of the following commands:

    quit
    exit
    /q

