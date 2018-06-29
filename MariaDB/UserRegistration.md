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

## Writing Data to Database
Create the `server.php` file. Now enter the following code:

	<?php
     
	// Used to track cookies
	session_start();
     
	// Initialize variables to hold values
	$username = "";    // Empty String
	$email    = "";    // Empty String
	$errors = array(); // Empty Array 
     
	// Connect to the database
	$db = mysqli_connect('localhost', 'root', '', 'registration');
     
	// Throw an error if connection to the database failed
	if (mysqli_connect_errno()) {
	  echo "failed to connect to database: " . mysqli_connect_error();
	}
     
	// REGISTER USER
	if (isset($_POST['reg_user'])) { // If the "Register User" button is pressed
	  // Receive all input values from the form
	  // Bind the values to variables
	  $username = mysqli_real_escape_string($db, $_POST['username']);
	  $email = mysqli_real_escape_string($db, $_POST['email']);
	  $password_1 = mysqli_real_escape_string($db, $_POST['password_1']);
	  $password_2 = mysqli_real_escape_string($db, $_POST['password_2']);
     
	  // Form validation: ensure that the form is filled and not left empty
	  // If it's empty, push a string onto the $error array
	  if (empty($username)) { array_push($errors, "Username is required"); }
	  if (empty($email)) { array_push($errors, "Email is required"); }
	  if (empty($password_1)) { array_push($errors, "Password is required"); }
     
	  // Compare both passwords in the form to see if they're the same
	  // If not, push a string onto the $error array
	  if ($password_1 != $password_2) {
		array_push($errors, "The two passwords do not match");
	  }
     
	  // Checking the Database
	  // Make sure there isn't a duplicate username or email in the database
	  $user_check_query = "SELECT * FROM users WHERE username='$username' OR email='$email' LIMIT 1";
	  $result = mysqli_query($db, $user_check_query);
	  $user = mysqli_fetch_assoc($result);
	  
	  // If user already exist...
	  if ($user) {
	    if ($user['username'] === $username) {
	      array_push($errors, "Username already exists");
	    }
     
      // If user email already exist...
	    if ($user['email'] === $email) {
	      array_push($errors, "Email already exists");
	    }
	  }
     
	  // Finally, register user if there are no errors in the form
	  if (count($errors) == 0) { // $error array is empty
	  	// md5() is a PHP function for encrypting strings
	  	$password = md5($password_1);
     
     	// Insert the information in the database
	  	$query = "INSERT INTO users (username, email, password) 
	  			  VALUES('$username', '$email', '$password')";
	  	mysqli_query($db, $query);
     
		// Cookies to keep user logged in	  	
	  	$_SESSION['username'] = $username;
	  	$_SESSION['success'] = "You are now logged in";
	  	// Redirect the user back to the index page
	  	header('location: index.php');
	  }
	}

## Login the User

Create a `login.php` file. We need a way for current users to login. We will create a form that will have a username and password box with a login button. Then there will be a link at the bottom of the form that an unregistered user can click to initialize the `register.php` page. Put the following code in the `login.php` file:

	<?php include('server.php') ?>
	<!DOCTYPE html>
	<html>
	<head>
	  <title>Registration system PHP and MySQL</title>
	  <link rel="stylesheet" type="text/css" href="style.css">
	</head>
	<body>
	  <div class="header">
	  	<h2>Login</h2>
	  </div>
		 
	  <form method="post" action="login.php">
	  	<?php include('errors.php'); ?>
	  	<div class="input-group">
	  		<label>Username</label>
	  		<input type="text" name="username" >
	  	</div>
	  	<div class="input-group">
	  		<label>Password</label>
	  		<input type="password" name="password">
	  	</div>
	  	<div class="input-group">
	  		<button type="submit" class="btn" name="login_user">Login</button>
	  	</div>
	  	<p>
	  		Not yet a member? <a href="register.php">Sign up</a>
	  	</p>
	  </form>
	</body>
	</html>

## Handle the Login from Server Side

The login procedure needs to be hnadled in the `server.php` file, which is why we put `<?php include('server.php') ?>` at the first line. Open the `server.php` page and add the following code after the User Registration block of codes:

	if (isset($_POST['login_user'])) {
	  $username = mysqli_real_escape_string($db, $_POST['username']);
	  $password = mysqli_real_escape_string($db, $_POST['password']);
     
	  if (empty($username)) {
	  	array_push($errors, "Username is required");
	  }
	  if (empty($password)) {
	  	array_push($errors, "Password is required");
	  }
     
	  if (count($errors) == 0) {
	  	$password = md5($password);
	  	$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
	  	$results = mysqli_query($db, $query);
	  	if (mysqli_num_rows($results) == 1) {
	  	  $_SESSION['username'] = $username;
	  	  $_SESSION['success'] = "You are now logged in";
	  	  header('location: index.php');
	  	}else {
	  		array_push($errors, "Wrong username/password combination");
	  	}
	  }
	}
     
	?>

We grab the username and password from the `login.php` form when the button `<button type="submit" class="btn" name="login_user">Login</button>` is clicked. Then we check if the username or password is entered correctly (not empty) and we push strings into the $error array if it's incorrect. Then if there's no error, we run a query to grab the matching username and password. If there are no matching username or password, we push a string into the $error array. Once logged in, the user is directed to the `index.php` page.

We will now make the `index.php` page. Create the page and put the following code in it:

	<?php 
	  session_start(); 
     
	  if (!isset($_SESSION['username'])) {
	  	$_SESSION['msg'] = "You must log in first";
	  	header('location: login.php');
	  }
	  if (isset($_GET['logout'])) {
	  	session_destroy();
	  	unset($_SESSION['username']);
	  	header("location: login.php");
	  }
	?>
	<!DOCTYPE html>
	<html>
	<head>
		<title>Home</title>
		<link rel="stylesheet" type="text/css" href="style.css">
	</head>
	<body>
     
	<div class="header">
		<h2>Home Page</h2>
	</div>
	<div class="content">
	  	<!-- notification message -->
	  	<?php if (isset($_SESSION['success'])) : ?>
	      <div class="error success" >
	      	<h3>
	          <?php 
	          	echo $_SESSION['success']; 
	          	unset($_SESSION['success']);
	          ?>
	      	</h3>
	      </div>
	  	<?php endif ?>
     
	    <!-- logged in user information -->
	    <?php  if (isset($_SESSION['username'])) : ?>
	    	<p>Welcome <strong><?php echo $_SESSION['username']; ?></strong></p>
	    	<p> <a href="index.php?logout='1'" style="color: red;">logout</a> </p>
	    <?php endif ?>
	</div>
			
	</body>
	</html>

The first statement is a php block that checks if the user is logged in. If they're not, then they are directed to the `login.php` page. If they are logged in, then they are directed to the `index.php` page with an option to logout.