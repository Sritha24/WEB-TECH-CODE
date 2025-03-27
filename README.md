# WEB-TECH-CODE
<!DOCTYPE html>
<html>
<head>
    <title>Hostel Booking</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
            background-color: #f9f9f9;
        }
        .container {
            display: none;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin: auto;
        }
        th, td {
            border: 1px solid #d1d1d6;
            padding: 12px 15px;
            text-align: center;
            font-size: 16px;
        }
        th {
            background-color: #f2f2f7;
            color: #333;
        }
        .select-btn {
            background-color: #ff3b30;
            color: white;
            padding: 10px 15px;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }
        button {
            padding: 12px 20px;
            background: #ff3b30;
            color: white;
            border: none;
            cursor: pointer;
            margin-top: 30px;
            border-radius: 5px;
            font-size: 16px;
        }
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        .header img {
            width: 100px;
            height: auto;
        }
        .bill {
            border: 2px solid #d1d1d6;
            padding: 20px;
            width: 50%;
            margin: auto;
            text-align: left;
            background-color: white;
            border-radius: 8px;
        }
        /* Responsive Design */
        @media screen and (max-width: 768px) {
            .bill {
                width: 90%;
            }
            table {
                font-size: 14px;
            }
        }
        /* Stylish Input Fields */
        input[type="text"], input[type="email"] {
            padding: 12px;
            width: 80%;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }
        label {
            font-size: 18px;
            font-weight: bold;
            color: #333;
            margin-top: 10px;
            display: block;
        }
        .form-container {
            margin: 20px auto;
            text-align: left;
            width: 50%;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <div class="header">
        <img src="https://srmap.edu.in/file/2019/12/Logo-2.png" alt="SRM University AP Logo">
        <h2>SRM University, Neerukonda, Amaravati-522502</h2>
    </div>
    
    <div id="step1" class="container" style="display: block;">
        <h2>Hostel Layout</h2>
        <img src="https://srmap.edu.in/wp-content/uploads/2020/08/HostelBul1-1140x500_c.jpg" alt="Hostel Layout" width="80%">
        <br>
        <button onclick="showBookingPage()">Continue to Booking</button>
    </div>
    
    <div id="step2" class="container">
        <h2>Hostel Booking</h2>
        <h3>Select a Room</h3>
        <table>
            <tr>
                <th>Floor</th>
                <th>Room Number</th>
                <th>Type</th>
                <th>Room Rent (₹)</th>
                <th>Mess Charges (₹)</th>
                <th>Total (₹)</th>
                <th>Book</th>
            </tr>
            <script>
                let messCharge = 70000;
                let floors = 9;
                let flatsPerFloor = 6;
                let roomTypes = ["2 Sharing", "3 Sharing AC", "4 Sharing", "4 Sharing Bunker", "3 Sharing AC", "3 Sharing"];
                let roomCosts = [85000, 110000, 125000, 70000, 110000, 85000];
                
                for (let floor = 1; floor <= floors; floor++) {
                    for (let flat = 1; flat <= flatsPerFloor; flat++) {
                        let roomNumber = `${floor}0${flat}`;
                        let type = roomTypes[flat - 1];
                        let rent = roomCosts[flat - 1];
                        let total = rent + messCharge;
                        if (type === "3 Sharing AC") {
                            total = 180000;
                        } else if (type === "3 Sharing") {
                            total = 150000;
                        }
                        document.write(`
                            <tr>
                                <td>${floor}F</td>
                                <td>${roomNumber}</td>
                                <td>${type}</td>
                                <td>${rent.toLocaleString()}</td>
                                <td>${messCharge.toLocaleString()}</td>
                                <td>${total.toLocaleString()}</td>
                                <td><button class='select-btn' onclick="selectRoom('${type}', '${total}', '${roomNumber}')">Select</button></td>
                            </tr>
                        `);
                    }
                }
            </script>
        </table>
    </div>

    <!-- User Details Form -->
    <div id="step4" class="container">
        <h2>Please Enter Your Details</h2>
        <div class="form-container">
            <label for="userName">Name:</label>
            <input type="text" id="userName" placeholder="Enter your name">

            <label for="userID">Student ID:</label>
            <input type="text" id="userID" placeholder="Enter your Student ID">

            <label for="userEmail">Email:</label>
            <input type="email" id="userEmail" placeholder="Enter your email">

            <button onclick="confirmDetails()">Submit</button>
        </div>
    </div>
    
    <div id="step3" class="container">
        <h3>Booking Confirmation</h3>
        <div class="bill">
            <h2>Hostel Booking Receipt</h2>
            <p><strong>Name:</strong> <span id="confirmedName"></span></p>
            <p><strong>Student ID:</strong> <span id="confirmedID"></span></p>
            <p><strong>Email:</strong> <span id="confirmedEmail"></span></p>
            <p><strong>Room Type:</strong> <span id="selectedRoomType"></span></p>
            <p><strong>Room Number:</strong> <span id="selectedRoomNumber"></span></p>
            <p><strong>Total Amount Paid:</strong> ₹<span id="selectedPrice"></span></p>
            <p><strong>Date:</strong> <span id="bookingDate"></span></p>
        </div>
        <button onclick="printBill()">Print Receipt</button>
    </div>

    <script>
        let selectedRoomType = "";
        let selectedPrice = "";
        let selectedRoomNumber = "";
        
        function showBookingPage() {
            document.getElementById("step1").style.display = "none";
            document.getElementById("step2").style.display = "block";
        }
        
        function selectRoom(roomType, price, roomNumber) {
            selectedRoomType = roomType;
            selectedPrice = price;
            selectedRoomNumber = roomNumber;
            
            document.getElementById("step2").style.display = "none";
            document.getElementById("step4").style.display = "block";
        }
        
        function confirmDetails() {
            let userName = document.getElementById("userName").value;
            let userID = document.getElementById("userID").value;
            let userEmail = document.getElementById("userEmail").value;

            if (userName && userID && userEmail) {
                // Show confirmation details
                document.getElementById("step4").style.display = "none";
                document.getElementById("step3").style.display = "block";
                
                document.getElementById("confirmedName").innerText = userName;
                document.getElementById("confirmedID").innerText = userID;
                document.getElementById("confirmedEmail").innerText = userEmail;
                document.getElementById("selectedRoomType").innerText = selectedRoomType;
                document.getElementById("selectedRoomNumber").innerText = selectedRoomNumber;
                document.getElementById("selectedPrice").innerText = selectedPrice;
                document.getElementById("bookingDate").innerText = new Date().toLocaleDateString();
            } else {
                alert("Please fill in all the fields.");
            }
        }
        
        function printBill() {
            window.print();
        }
    </script>
</body>
</html>
