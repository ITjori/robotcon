## Task 2
**( robot controller )**

This web application is designed to control a robot remotely. It consists of three main files:

	1.	HTML (index.html) – User interface with control buttons
 
	2.	PHP (update.php) – Handles user commands and stores them in a database
 
	3.	PHP (view.php) – Retrieves the latest command for execution
 
i use XAMPP as a local server (Apache, MySQL, PHP) and Visual Studio Code for development.

***Step 1: Frontend - HTML and JavaScript***
	•	The index.html file provides an interface for controlling the robot.
	•	It has buttons for moving the robot: Forward (F), Backward (B), Left (L), Right (R), Stop (S).
	•	When a button is clicked, the sendDirection() function is called.

**-JavaScript : Sending Commands-**

*function sendDirection(direction) {
    fetch("update.php", {
        method: "POST",
        headers: { "Content-Type": "application/x-www-form-urlencoded" },
        body: "direction=" + direction
    }).then(response => response.text()).then(data => {
        console.log(data);
    });
}*

	•	This function sends an HTTP POST request to update.php, passing the selected direction.
	•	The response is logged in the console for debugging.

***Step 2: Backend - PHP (update.php)***
	•	This file processes user input and updates the MySQL database.
	•	Steps:
	1.	Establish a database connection (robot_control database).
	2.	Validate the received direction (F, B, L, R, S).
	3.	Store the valid command in the directions table.

**-Key Code Snippet-**

*if ($_SERVER["REQUEST_METHOD"] == "POST" && isset($_POST["direction"])) {
    $direction = $_POST["direction"];
    if (in_array($direction, ['F', 'B', 'L', 'R', 'S'])) {
        $sql = "INSERT INTO directions (direction) VALUES ('$direction')";
        $conn->query($sql);
    }
}*

	•	Only valid commands are inserted into the database.
	•	The response confirms if the update was successful.

***Step 3: Retrieving the Latest Command (view.php)***
	•	This file fetches the last recorded movement command.
	•	The robot’s controller can use this data to execute the last command.

**-Key Code Snippet-**

*$sql = "SELECT direction FROM directions ORDER BY id DESC LIMIT 1";
$result = $conn->query($sql);
if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    echo "Last Direction: " . $row["direction"];
}*

	•	Queries the latest direction from the database and displays it.

# Final Overview
	1.	User clicks a button (e.g., “Forward”) → JavaScript sends the command to update.php.
	2.	update.php stores the command in the MySQL database.
	3.	The robot checks view.php for the latest command.

