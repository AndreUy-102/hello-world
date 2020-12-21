<html xmlns="http://www.w3.org/1999/html">
<head>
    <title>The Award Shop | Home Page</title>
    <style>
        .box {
            display: grid;
            padding: 10px;
            grid-template-columns: 1fr 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr 1fr 1fr;
            margin: auto;
        }
        .side-bar {
            border: 3px solid;
            grid-column: 1/2;
            grid-row: 1/3;
            margin: auto;
            color: black;
            height: 100%;
            background-color: white;
        }
    </style>
</head>
<?php
    require_once "home_tpl.php";
?>
<body>
    <div class="box">
        <div class="side-bar">
            <h1 align="center">E.M.S.<br><span style="font-size: 10px;">VERSION 1.0.0</span></h1>
            <p align="center">Employee Management System</p>
            <p align="center">BY:</p>
            <h2 align="center">A.L.U. Tech<br><span style="font-size: 20px">Advance , Leading , User-Friendly</span></h2>
            <br>
            <p>Welcome to the Employee Management System maintain and developed by A.L.U. Tech</p>
            <p>The E.M.S. Software maintain your employee profile, payroll, and keeps records.</p>
            <p>We assure that the privacy and security of this software is at its best.</p>
            <span style="font-size: 20px; color: red;">NOTICE: NEVER GIVE YOUR PERSONAL DATA UNLESS HE/SHE IS AUTHORIZE BY THE A.L.U. TECH!</span>
        </div>
    </div>
</body>
</html>

