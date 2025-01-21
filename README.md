# Robotic-Remote-Controller
*Robotic Remote Controller* project. The project enables you to control a robot's movement through a web-based interface and store directions in a database.

---

### *Project Structure*
1. *HTML*: Frontend interface for controlling the robot.  
2. *PHP Scripts*: Backend to handle database operations for sending and retrieving directions.  
3. *Database*: Stores the directions in a MySQL table.  

---

### *Prerequisites*
1. *Software Required*:
   - XAMPP (or any Apache-MySQL-PHP stack).
   - Web browser (e.g., Chrome, Firefox).
   - Text/code editor (e.g., VS Code, Notepad++).
2. *Hardware* (optional for robot connection):
   - ESP32 or similar microcontroller.
   - Bluetooth or WiFi module for communication.

---

### *Steps to Set Up*

#### *1. Database Setup*
1. Open phpMyAdmin through your browser:  
   
   http://localhost/phpmyadmin
   
2. Create a database named *robot_control*.
3. Create a table named *directions* with the following columns:
   - *id*: INT, AUTO_INCREMENT, PRIMARY KEY.
   - *direction*: VARCHAR(20).

   *SQL Query*:  
   sql
   CREATE TABLE directions (
       id INT AUTO_INCREMENT PRIMARY KEY,
       direction VARCHAR(20) NOT NULL
   );
   

---

#### *2. PHP Backend*
1. *File 1: send_direction.php*  
   Handles inserting direction commands into the database.

   php
   <?php
   $conn = new mysqli("localhost", "root", "", "robot_control");

   if ($conn->connect_error) {
       die("Connection failed: " . $conn->connect_error);
   }

   if (isset($_GET['direction'])) {
       $direction = $_GET['direction'];
       $stmt = $conn->prepare("INSERT INTO directions (direction) VALUES (?)");
       $stmt->bind_param("s", $direction);
       $stmt->execute();
       echo "Direction sent: " . $direction;
   }
   ?>
   

2. *File 2: get_last_direction.php*  
   Retrieves the most recent direction from the database.

   php
   <?php
   $conn = new mysqli("localhost", "root", "", "robot_control");

   if ($conn->connect_error) {
       die("Connection failed: " . $conn->connect_error);
   }

   $result = $conn->query("SELECT direction FROM directions ORDER BY id DESC LIMIT 1");
   if ($row = $result->fetch_assoc()) {
       echo $row['direction'];
   } else {
       echo "No direction found.";
   }
   ?>
   

---

#### *3. HTML Frontend*
1. Save the following file as index.html inside the folder:  
   
   C:\xampp\htdocs\robot_project\
   

   html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Robotic Remote Controller</title>
       <style>
           body {
               display: flex;
               justify-content: center;
               align-items: center;
               height: 100vh;
               background-color: #f0f0f0;
               margin: 0;
               font-family: Arial, sans-serif;
           }
           .controller {
               display: flex;
               flex-direction: column;
               align-items: center;
           }
           .button {
               width: 100px;
               height: 100px;
               background-color: #007BFF;
               color: white;
               border: none;
               font-size: 20px;
               margin: 10px;
               cursor: pointer;
               border-radius: 10px;
           }
           .button:hover {
               background-color: #0056b3;
           }
           #lastDirection {
               margin-top: 20px;
               font-size: 24px;
           }
       </style>
   </head>
   <body>
       <div class="controller">
           <h1>Robotic Remote Controller</h1>
           <button class="button" onclick="sendDirection('forward')">Forward</button>
           <div>
               <button class="button" onclick="sendDirection('left')">Left</button>
               <button class="button" onclick="sendDirection('right')">Right</button>
           </div>
           <button class="button" onclick="sendDirection('backward')">Backward</button>
           <button class="button" onclick="sendDirection('stop')" style="background-color: red;">Stop</button>
           <div id="lastDirection"></div>
       </div>

       <script>
           function sendDirection(direction) {
               fetch(`send_direction.php?direction=${direction}`)
                   .then(response => response.text())
                   .then(data => console.log(data));
           }

           function getLastDirection() {
               fetch('get_last_direction.php')
                   .then(response => response.text())
                   .then(data => {
                       document.getElementById('lastDirection').innerText = `Last Direction: ${data}`;
                   });
           }
       </script>
   </body>
   </html>
   

---

#### *4. Run the Project*
1. Start XAMPP and ensure *Apache* and *MySQL* are running.
2. Open a browser and navigate to:  
   
   http://localhost/robot_project/index.html
   
3. Use the buttons to control the robot:
   - *Forward, **Backward, **Left, **Right, or **Stop*.

---

### *Optional Steps for ESP32 Integration*
1. Use *Bluetooth* or *WiFi* for communication between the web interface and the ESP32.
2. Modify the ESP32 firmware to listen for direction data and translate it into movement commands.
3. Test the full integration.

---

### *the link*
file:///C:/xampp/htdocs/robotc_project/ruoof.html
