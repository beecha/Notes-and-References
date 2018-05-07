## MariaDB
MariaDB is currently installed and running on my personal Raspberry Pi. MariaDB is the fork of MySQL. Using MySQL commands should work for MariaDB as well. These notes are for commands. If the command line does not suit, you can use the web interface, phpmyadmin.

## Logging Into the Database
SSH into my Raspberry Pi then run the following command to get into MariaDB:

    mysql -u USER -p

Where `USER` is the user account that is associated with MariaDB. The default user is `root`. It will ask for the password. After entering the password successfully, you will see:

    MariaDB [(none)]> 

You're now logged in the database. To log out of MariaDB, you can use any of the following commands:

    quit
    exit
    /q

## Create a User
To create a user, run the following mysql command:

    CREATE USER 'USERNAME'@'localhost' IDENTIFIED BY 'UserPassword';

This will add a row to the user table where `USERNAME` and `UserPassword` with no privileges (add, delete, etc). You can run the following query to list the users in the Users table:

    SELECT User from mysql.user;

You will probably see something like so:

    +------------+
    | User       |
    +------------+
    | USERNAME   |
    | root       |
    +------------+


The newly created user does not have any privileges yet. We will work on that later.

## Create a Database

The following command will create a database:

    CREATE DATABASE reviews;

Typing the following command will list the databases:

    Show databases;
     
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | phpmyadmin         |
    | reviews            |
    +--------------------+

Switch to that new database you just created:

    USE reviews;

## Create a Table

We will create a table with 3 fields. Run the following command:

    CREATE TABLE user_reviews (
    id MEDIUMINT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    reviewer_name CHAR(100),
    star_rating TINYINT,
    details VARCHAR(4000)
    );

* `id` is a unique key in the table. When populating the table, this ID key will auto increment.
* `reviewer_name` is a text field limited to 100 characters.
* `star_rating` is a numeric value of 1 to 5.
* `details` is another text field limited to 4000 characters.

## Inserting and Populating the Table

We will populate the table with 2 examples by using the `INSERT INTO` command:

    INSERT INTO user_reviews (reviewer_name, star_rating, details) VALUES ('Frank', '5', 'I enjoyed my time here. I recommend it!');
     
    INSERT INTO user_reviews (reviewer_name, star_rating, details) VALUES ('Kelly', '1', 'Poor service. Had hair in my food. Yuck.');

To see the data in the table, run the following command:

    SELECT * FROM user_reviews;
     
    +----+---------------+-------------+------------------------------------------+
    | id | reviewer_name | star_rating | details                                  |
    +----+---------------+-------------+------------------------------------------+
    |  1 | Frank         |           5 | I enjoyed my time here. I recommend it!  |
    |  2 | Kelly         |           1 | Poor service. Had hair in my food. Yuck. |
    +----+---------------+-------------+------------------------------------------+

Now we will grant privilege to the new `USERNAME` that we created earlier for this `reviews` database.

    GRANT ALL PRIVILEGES ON reviews.* TO 'USERNAME'@'localhost';

## Create PHP Script

We will now create a script that will pull the information from the database and display it in a table format on a website. Create a file in your localhost directory, in my case it will be `/var/www/html/`. We will name it `showreviews.php`. Begin with the HTML declarations:

    <!DOCTYPE html>
    <html>
    <body>

Create PHP variables to bind our strings that will get us logged into the database.

    <?php
    $hostname = "localhost";
    $username = "USERNAME";
    $password = "UserPassword";
    $db = "reviews";

Bind another variable to a MySQL method, `mysqli_connect()`, which will take 4 variables earlier as arguments.

    $dbconnect=mysqli_connect($hostname, $username, $password, $db);

Use an if statement to check if you were able to access the database.

    if ($dbconnect->connect_error) {
    	die("Database connection failed: " . $dbconnect->connect_error);
    }
     
    ?>

Now we will create the table to display the data. Start off by creating the table headers.

    <table border="1" align="center">
    <tr>
    	<th>Reviewer Name</th>
    	<th>Stars</th>
    	<th>Details</th>
    </tr>

Bind a variable to run a query that will select the table, `user_reviews`, with all the columns from it. Then run a while loop to fetch the data from each row in the table.

    <?php
     
     $query = mysqli_query($dbconnect, "SELECT * FROM user_reviews");
     
     while ($row = mysqli_fetch_array($query)) {
     	echo
     	"<tr>
     		<td>{$row['reviewer_name']}</td>
     		<td<{$row['star_rating']}</td>
     		<td<{$row['details']}</td>
     	</tr>\n";
     }

And now we close the HTML tags.

    </table>
    </body>
    </html>

Navigate to your `showreviews.php` website and you will see the data shown in a table.

## Create an Entry Data Form

We will now create a website that will have a `<form>` element that will be able to enter information into a database. The existing database, `reviews`, and table, `user_reviews`, will be used. Make a website in your localhost directory, in my case it will be `/var/www/html`. And I'm going to name it `reviews.php`. Now put the following HTML in the file:

    <!DOCTYPE HTML>
    <html>
    <body>
     
    <p>Review Form</p>
     
    <form action="addreview.php" method="POST">
     
    Your Name: <input type="text" name="reviewer_name"><br><br>
     
    Star Rating
    <select name="star_rating">
    	<option value="1">1 Star</option>
    	<option value="2">2 Stars</option>
    	<option value="3">3 Stars</option>
    	<option value="4">4 Stars</option>
    	<option value="5">5 Stars</option>
    </select><br><br>
         
    Your Review<br>
    	<textarea name="details" rows="10" cols="30"></textarea><br><br>
    	<input type="submit" value="Submit" name="submit">
     
    </form>
    </body>
    </html>

The form's `method="POST"` will transfer the information inside the form to an action, `action="addreview.php"`. Now create the `addreview.php` file in your directory and add php scripts to it.

Create php variables and bind it to the strings that will get you access to the database.

    <?php
    $hostname = "localhost";
    $username = "USERNAME";
    $password = "UserPassword";
    $db = "reviews";

Bind another variable to hold the mysqli_connect() method with the 4 variables.

    $dbconnect=mysqli_connect($hostname, $username, $password, $db);

Do an if statement to check if the access to the database failed.

    if ($dbconnect->connect_error) {
    	die("Database connection failed: " . $dbconnect->connect_error);
    }

Grab the information from the HTML form and put it in php variables.

    if(isset($_POST['submit'])) {
    	$reviewer_name=$_POST['reviewer_name'];
    	$star_rating=$_POST['star_rating'];
    	$details=$_POST['details'];

Insert the information into the database table.

        $query = "INSERT INTO user_reviews (reviewer_name, star_rating, details) VALUES ('reviewer_name', 'star_rating', 'details')";

Do an if statement if it failed to insert into the database. Otherwise throw a success message if it went through.

    if (!mysql_query($dbconnect, $query)) {
    	die('An error occurred when submitting.');
    }
    else {
    	echo "Submission Success.";
    }

Close off the if statement from `$dbconnect->connect_error` and close off the php script.

    }
    ?>

