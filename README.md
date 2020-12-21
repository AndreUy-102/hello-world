<?php
    require_once 'config/dbconfig.php';
?>

<html>
<head>
    <title>The Award Shop | Employee</title>
    <script type="text/javascript">
        function confirmDel(arg) {
            var del = confirm("Are you sure you want to delete " + arg +"?");

            if (del === true) {
                window.location.href = "employeeRecord.php?delete_emp=" + arg;
            } else {
                // DO NOTHING
            }
        }
        function updateEmp(arg) {
            var myWindow = window.open("update_emp.php?update_emp=" + arg, "updateWin", "width=500,height=500");
        }

        function openCA(arg) {
            var myWin = window.open("cashAd.php?cash_advance=" + arg , "updateWin" , "width=500 , height=500");
        }
    </script>
    <link rel="stylesheet" href="config/empRecord.css">
</head>
<?php require_once "home_tpl.php"?>
<body>
    <div class="lunch_box">
        <div class="select_emp">
            <form name="select_emp" id="select_emp" method="GET">
                <p>SELECT EMPLOYEE:</p>
                <select name="emp" id="emp" onchange="submit()">
                    <option>--SELECT--</option>
                    <?php
                    $emp = "SELECT name FROM employee";
                    $emp_result = $conn->query($emp);
                    while ( $row = $emp_result->fetch_assoc() ){
                        echo '<option value="' . $row['name'] . '">' . $row['name'] . '</option>';
                    }
                    ?>
                </select>
            </form>
        </div>
        <div class="details_emp">
            <?php
            if ($_GET) {
                // DELETE SQL
                if (isset($_GET['delete_emp'])) {
                    $name = $_GET['delete_emp'];

                    $delete_emp ="DELETE FROM employee WHERE employee.name = '" . $name . "'";
                    $delete_emp_result = $conn->query($delete_emp);

                    if ($delete_emp_result === true) {
                        echo '<script type="text/javascript">';
                        echo 'alert("Successfully deleted!");';
                        echo 'window.location.href="employeeRecord.php";';
                        echo '</script>';
                    } else {
                        echo "ERROR";
                    }
                }

                if (isset($_GET) && isset($_GET['emp'])) {
                    $emp_show = "SELECT employee.name ,
                                        employee.nickname , 
                                        employee.cp_no ,
                                        employee.tin ,
                                        employee.sss ,
                                        employee.philhealth , 
                                        employee.age , 
                                        employee.department , 
                                        employee.address,
                                        employee.ca,
                                        employee.img_path ,
                                        rate.rate_per_hr
                                        FROM employee
                                        LEFT JOIN rate ON rate.rate_id = employee.rate_id
                                        WHERE employee.name ='" . $_GET['emp'] . "'";
                    $emp_show_result = $conn->query($emp_show);
                    $emp_row = $emp_show_result->fetch_assoc();
                     if ( isset( $_GET['emp'] ) && isset( $_GET) ) {
                        echo '<img src="' . $emp_row['img_path'] . '" style="width: 100px; margin: auto">';
                        echo '<br>';
                        echo '<label>Name:</label>';
                        echo '<input type="text" value="' . $emp_row['name'] . '" readonly>';
                        echo '<label>Nick Name:</label>';
                        echo '<input type="text" value="' . $emp_row['nickname'] . '" readonly>';
                        echo '<label>Age:</label>';
                        echo '<input type="text" value="' . $emp_row['age'] . '" readonly>';
                        echo '<label>Contact Number:</label>';
                        echo '<input type="text" value="' . $emp_row['cp_no'] . '" readonly>';
                        echo '<label>Address:</label>';
                        echo '<input type="text" value="' . $emp_row['address'] . '" readonly>';
                        echo '<label>TIN Number:</label>';
                        echo '<input type="text" value="' . $emp_row['tin'] . '" readonly>';
                        echo '<label>SSS Number:</label>';
                        echo '<input type="text" value="' . $emp_row['sss'] . '" readonly>';
                        echo '<label>PhilHealth Number:</label>';
                        echo '<input type="text" value="' . $emp_row['philhealth'] . '" readonly>';
                        echo '<label>Department:</label>';
                        echo '<input type="text" value="' . $emp_row['department'] . '" readonly>';
                        echo '<label>rate/hr:</label>';
                        echo '<input type="text" value="' . $emp_row['rate_per_hr'] . '" readonly>';
                        echo '<label>CA balance:</label>';
                        echo '<input type="text" value="' . $emp_row['ca'] . '" readonly>';
                    } else
                         echo "<p>Employee Details will be shown here!</p>";
                }
            }

            ?>
        </div>
        <div class="btn">
            <div class="firstBtn">
                <button onclick="confirmDel('<?php if (isset($emp_row['name'] ) ) echo $emp_row['name']; ?>')">DELETE</button>
            </div>
            <div class="secondBtn">
                <button onclick="updateEmp('<?php if (isset($_GET['emp'] ) ) echo $_GET['emp']; ?>')">UPDATE</button>
            </div>
            <div class="thirdBtn">
                <button onclick="openCA('<?php if (isset($_GET['emp'] ) ) echo $_GET['emp']; ?>')">CASH ADVANCE</button>
            </div>
        </div>
    </div>
</body>
</html>
