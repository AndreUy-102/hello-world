<?php
    require_once 'config/dbconfig.php';

    $show_rows = 5;
    $pageNum = 0;

    $count_sql = "SELECT count(*) as Count FROM employee;";
    $count_result = $conn->query($count_sql);

    $total = $count_result->fetch_assoc();

    $page = ceil($total['Count'] / $show_rows);

        if ($_POST) {
            if ( isset($_POST['search']) && $_POST['search'] != '') {
                echo $find = "SELECT
                        employee.name, 
                        employee.nickname, 
                        employee.age,
                        employee.sss,
                        employee.tin,
                        employee.philhealth,
                        employee.status,
                        rate.rate_per_hr,
                        overtime.ot_date,
                        overtime.ot_hr 
                        FROM employee
                        LEFT JOIN overtime ON overtime.emp_id = employee.emp_id
                        LEFT JOIN rate ON rate.rate_id = employee.rate_id
                        WHERE name LIKE '%" . $_POST['search'] . "%' LIMIT $pageNum, $show_rows;";
                $result = $conn->query($find);
            } else { echo "Please Enter a Name"; }
        } else  //Do nothing

        if ($_GET) {
            if ( isset( $_GET[ 'pageNum' ] ) ) {
                $pageNum = $_GET['pageNum'];
            } else $pageNum = 1;

            if ( isset( $_GET['show_row'] ) ) {
                $show_rows = $_GET['show_row'];
            } else $show_rows = 5;

            $sort = "SELECT 
                employee.name, 
                employee.nickname, 
                employee.age,
                employee.sss,
                employee.tin,
                employee.philhealth,
                employee.status,
                rate.rate_per_hr,
                overtime.ot_date,
                overtime.ot_hr  
                FROM employee 
                LEFT JOIN overtime ON overtime.emp_id = employee.emp_id
                LEFT JOIN rate ON rate.rate_id = employee.rate_id 
                LIMIT $pageNum, $show_rows";
            $sort_result = $conn->query($sort);
        }
    $conn->close();
?>


<html>
    <head>
        <title>The Award Shop | Payroll</title>
    </head>
    <?php require_once "home_tpl.php" ?>
    <body>

        <div align="center">
            <h1 align="center">Record</h1>
            <form name="search" id="search" method="POST" action="">
                <input type="submit" value="Find" style="float: right">
                <input type="text" id="search" name="search" placeholder="Enter Employee Name" style="float: right">
            </form>
            <p style="float: left">Total Data: <?php echo $total['Count'] ?></p>
            <form method="GET" name="page" id="page" action="">
                <select id="show_row" name="show_row">
                    <option value="5" <?php if (isset($_GET['show_row']) && $_GET['show_row'] == '5') echo "selected";?>>5</option>
                    <option value="15" <?php if (isset($_GET['show_row']) && $_GET['show_row'] == '15') echo "selected";?>>15</option>
                    <option value="25" <?php if (isset($_GET['show_row']) && $_GET['show_row'] == '25') echo "selected";?>>25</option>
                    <option value="50" <?php if (isset($_GET['show_row']) && $_GET['show_row'] == '50') echo "selected";?>>50</option>
                </select>
                <input type="submit" value="Show Row">
                <?php
                if ( $pageNum > 1 ) {
                    echo '<a href="records.php?pageNum=1&show_row=' . $show_rows . '">&nbsp;First</a>';
                    echo " ";
                }

                if ($pageNum > 1) {
                    echo '<a href="records.php?pageNum=' . ($pageNum-1) . '&show_row='. $show_rows . '">&nbsp;Previous</a>';
                    echo " ";
                }

                for ($x=1;$x<$page;$x++){
                    echo '<a href="records.php?pageNum=' . $x . '"&nbsp;>' . $x . '</a>';
                    echo " ";
                }
                if ($x > $pageNum) {
                    echo '<a href="records.php?pageNum=' . ($pageNum+1) . '&show_row=' . $show_rows . '">&nbsp;Next</a>';
                    echo " ";
                }
                if ($x > $pageNum) {
                    echo '<a href="records.php?pageNum=' . ($pageNum + $x) . '">&nbsp;Last</a>';
                }

                ?>
            </form>
            <table border="1px solid" width="100%">
                <thead>
                    <tr>
                        <th>Employee Number</th>
                        <th>Name</th>
                        <th>Nickname</th>
                        <th>Age</th>
                        <th>SSS</th>
                        <th>TIN</th>
                        <th>Philhealth</th>
                        <th>Rate/hr</th>
                        <th>Date</th>
                        <th>OT Date</th>
                        <th>OT Hour</th>
                        <th>Status</th>
                        <th>Grand Total</th>
                    </tr>
                </thead>
                <tbody>
                        <?php
                        if ( $sort_result->num_rows > 0) {
                                while ($row = $sort_result->fetch_assoc()) {
                                    echo "<tr>";
                                    echo "<td></td>";
                                    echo "<td>" . $row['name'] . "</td>";
                                    echo "<td>" . $row['nickname'] . "</td>";
                                    echo "<td>" . $row['age'] . "</td>";
                                    echo "<td>" . $row['sss'] . "</td>";
                                    echo "<td>" . $row['tin'] . "</td>";
                                    echo "<td>" . $row['philhealth'] . "</td>";
                                    echo "<td>" . $row['rate_per_hr'] . "</td>";
                                    echo "<td></td>";
                                    echo "<td>" . $row['ot_date'] . "</td>";
                                    echo "<td>" . $row['ot_hr'] . "</td>";
                                    echo "<td>" . $row['status'] . "</td>";
                                    echo "<td></td>";
                                    echo "</tr>";
                                }
                        } else if ($result->num_rows > 0) {
                            while ($row = $result->fetch_assoc()) {
                                echo 'im here';
                                echo "<tr>";
                                echo "<td></td>";
                                echo "<td>" . $row['name'] . "</td>";
                                echo "<td>" . $row['nickname'] . "</td>";
                                echo "<td>" . $row['age'] . "</td>";
                                echo "<td>" . $row['sss'] . "</td>";
                                echo "<td>" . $row['tin'] . "</td>";
                                echo "<td>" . $row['philhealth'] . "</td>";
                                echo "<td>" . $row['rate_per_hr'] . "</td>";
                                echo "<td></td>";
                                echo "<td>" . $row['ot_date'] . "</td>";
                                echo "<td>" . $row['ot_hr'] . "</td>";
                                echo "<td>" . $row['status'] . "</td>";
                                echo "<td></td>";
                                echo "</tr>";
                            }
                        } else {
                            echo "<tr>";
                            echo '<td colspan="2">Nothing to Display</td>';
                            echo "</tr>";
                        }
                        ?>

                </tbody>
            </table>
        </div>
    </body>
</html>
