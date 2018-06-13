## Notes: Use PHP and MySQL to Register Users
We need a database to store the user's data. So create a database called `register`:

    create database register;
    use register;

We need a table in the database. So create a table called `users` with the following rows:

    create table users (
        id int(11) not null auto_increment primary key,
        username varchar(100) not null,
        email varchar(100) not null,
        password varchar(100) not null
    );

* `id` is of type int with 11 digits. It'll be a unique primary key and automatically increment on successive user registrations.
* `username` is of type varchar with 100 characters.
* `email` is of type varchar with 100 characters.
* `password` is of type varchar with 100 characters.

## Registering Users
Create a php page to contain a form for users to enter their registration information. So create the page `register.php` and put the following code in it:

    <?php include('server.php') ?>
    <!DOCTYPE html>
    <html>
    <head>
    <title>Registration system PHP and MySQL</title>
    <link rel="stylesheet" type="text/css" href="style.css">
    </head>
    <body>
        <div class="header">
            <h2>Register</h2>
        </div>
     
    <form method="post" action="register.php">
        <?php include('errors.php'); ?>
        <div class="input-group">
        <label>Username</label>
        <input type="text" name="username" value="<?php echo $username; ?>">
        </div>
        <div class="input-group">
        <label>Email</label>
        <input type="email" name="email" value="<?php echo $email; ?>">
        </div>
        <div class="input-group">
        <label>Password</label>
        <input type="password" name="password_1">
        </div>
        <div class="input-group">
        <label>Confirm password</label>
        <input type="password" name="password_2">
        </div>
        <div class="input-group">
        <button type="submit" class="btn" name="reg_user">Register</button>
         </div>
        <p>
        Already a member? <a href="login.php">Sign in</a>
        </p>
    </form>
    </body>
    </html>

The form method action is `<form method="post" action="register.php">`. We basically send the information in the form back to the `register.php` page upon hitting the submit button. BUT do notice the `<?php include('server.php') ?>` line at the very top. We will write the code later for `server.php` that will write the form data onto the database. There's also the `<?php include('errors.php'); ?>` that we will use to display error messages. We have also linked a stylesheet, `<link rel="stylesheet" type="text/css" href="style.css">`, you can style it however you like. Or you can use the following style:

	* {
	  margin: 0px;
	  padding: 0px;
	}
	body {
	  font-size: 120%;
	  background: #F8F8FF;
	}
     
	.header {
	  width: 30%;
	  margin: 50px auto 0px;
	  color: white;
	  background: #5F9EA0;
	  text-align: center;
	  border: 1px solid #B0C4DE;
	  border-bottom: none;
	  border-radius: 10px 10px 0px 0px;
	  padding: 20px;
	}
	form, .content {
	  width: 30%;
	  margin: 0px auto;
	  padding: 20px;
	  border: 1px solid #B0C4DE;
	  background: white;
	  border-radius: 0px 0px 10px 10px;
	}
	.input-group {
	  margin: 10px 0px 10px 0px;
	}
	.input-group label {
	  display: block;
	  text-align: left;
	  margin: 3px;
	}
	.input-group input {
	  height: 30px;
	  width: 93%;
	  padding: 5px 10px;
	  font-size: 16px;
	  border-radius: 5px;
	  border: 1px solid gray;
	}
	.btn {
	  padding: 10px;
	  font-size: 15px;
	  color: white;
	  background: #5F9EA0;
	  border: none;
	  border-radius: 5px;
	}
	.error {
	  width: 92%; 
	  margin: 0px auto; 
	  padding: 10px; 
	  border: 1px solid #a94442; 
	  color: #a94442; 
	  background: #f2dede; 
	  border-radius: 5px; 
	  text-align: left;
	}
	.success {
	  color: #3c763d; 
	  background: #dff0d8; 
	  border: 1px solid #3c763d;
	  margin-bottom: 20px;
	}

