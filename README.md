<?php
    require_once 'config/dbconfig.php';

?>

<html>
<head>
    <title>The Award Shop | Payroll</title>
    <script type="text/javascript">
        function submit() {
            document.getElementById('emp').submit();
        }

        // Computation

        function compute_hr() {
            var hr = document.getElementById('hr').value;
            var rate = document.getElementById('rate_per_hr').value;
            var day = document.getElementById('day').value;

            var answer = (hr * rate) * day;
            document.getElementById('final').value = answer;
        }

        function grandTotal() {
            var holiday = document.getElementById('holiday').value;
            var ca = document.getElementById('ca').value;
            var total = document.getElementById('final').value;


            if (ca && holiday !== "") {
                var grandTotal = Number(total)  + Number(holiday)  - Number(ca);
                alert ('TOTAL:PHP' + grandTotal);
            } else if (ca !== "") {
                var grandTotal = Number(total) - Number(ca);
                alert ('TOTAL:PHP' + grandTotal);
            } else if (holiday !== "") {
                var grandTotal = Number(total) + Number(holiday);
                alert ('TOTAL:PHP' + grandTotal);
            } else alert("Please Enter Amount!")
        }
    </script>
    <link href="config/payrollStyle.css" rel="stylesheet">
</head>
<?php require_once "home_tpl.php" ?>
<body>
    <div class="box">
        <div class="details">
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
            <div class="emp_detail">
                <?php
                    if ($_GET) {
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
                    }
                    if ( isset( $_GET ) && isset( $_GET['emp'] ) ) {
                        echo '<img src="' . $emp_row['img_path'] . '" width="75px">';
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
                    } else echo "Employee details will be shown here"
                    ?>
            </div>
        </div>
        <div class="salary">
            <div class="basic">
                <h3>Payroll:</h3>
                <label>Enter Hours</label>
                <input type="number" id="hr" name="hr" placeholder="Enter Hour">
                <input type="text" id="rate_per_hr" name="rate_per_hr" readonly value="<?php if ( isset( $_GET ) && isset( $_GET['emp'] ) ) echo $emp_row['rate_per_hr'] ?>" placeholder="Rate/Hr">
                <br>
                <label>Enter Days present</label>
                <input type="number" id="day" name="day" placeholder="Enter Days">
                <br>
                <button onclick="compute_hr()">Compute</button>
                <br>
                <br>
                <label>Total w/o CA deduction:</label>
                <div class="input-group mb-2">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="basic-addon1">₱</span>
                    </div>
                    <input type="text" id="final" class="form-control" placeholder="Total w/o CA deduction" aria-label="Username" aria-describedby="basic-addon1" readonly>
                </div>
            </div>
            <div class="extra">
                    <h3>Extra:</h3>
                    <label>Holiday payment</label>
                    <input type="number" name="holiday" id="holiday" placeholder="Enter Amount">
                    <br>
                <form method="POST" id="cashAd">
                    <label>Cash Advance deduction</label>
                    <br>
                    <input type="number" name="ca" id="ca" value="0" placeholder="Enter Amount">
                    <input type="number" name="bal" id="bal" readonly value="<?php if ( isset( $_GET ) && isset( $_GET['emp'] ) ) echo $emp_row['ca'] ?>" placeholder="Balance">
                    <?php
                        if ($_POST){
                            if ( isset( $_POST['ca'] ) == '' && isset( $_POST['bal'] ) ) {
                                    $save_ca = $_POST['bal'];
                            } else {
                                $save_ca = $_POST['bal'] - $_POST['ca'];
                            }
                            $update_sql = "UPDATE employee SET ca ='" . $save_ca . "' WHERE employee.name = '". $_GET['emp'] . "';";
                            $update_result = $conn->query($update_sql);

                        }

                    ?>
                    <br>
                    <button onclick="grandTotal()">Done</button>
                </form>
            </div>
        </div>
    </div>
</body>
</html>
