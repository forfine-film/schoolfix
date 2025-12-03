<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบแจ้งซ่อมอุปกรณ์โรงเรียน - Full Stack Lite</title>
    <style>
        :root {
            --primary-color: #007BFF; /* ฟ้าสดใส */
            --secondary-color: #0056b3; /* น้ำเงินเข้ม */
            --background-light: #f4f7f6; /* พื้นหลังสีเทาอ่อน */
            --text-dark: #333;
            --success-color: #28a745; /* สีเขียวสำหรับซ่อมสำเร็จ */
            --warning-color: #ffc107; /* สีเหลืองสำหรับรอซ่อม */
        }

        body {
            font-family: Arial, sans-serif;
            background-color: var(--background-light);
            color: var(--text-dark);
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        /* ------------------ Layout Containers ------------------ */
        .app-container {
            width: 100%;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .main-content {
            width: 95%;
            max-width: 1100px;
            margin: 40px auto;
            padding: 30px;
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
        }

        /* ------------------ Header & Typography ------------------ */
        header {
            background-color: var(--primary-color);
            color: white;
            padding: 30px 0;
            text-align: center;
            margin-bottom: 25px;
            border-radius: 10px 10px 0 0;
        }

        h1 { margin: 0; }
        
        /* ------------------ Form Styling ------------------ */
        .form-group {
            margin-bottom: 15px;
        }

        .btn {
            background-color: var(--primary-color);
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1em;
            font-weight: bold;
            transition: background-color 0.3s;
            display: block;
            width: 100%;
            margin-top: 20px;
        }

        .btn:hover {
            background-color: var(--secondary-color);
        }

        input[type="text"], input[type="date"], input[type="password"], textarea, select {
            width: 100%;
            padding: 12px;
            margin-top: 5px;
            display: inline-block;
            border: 1px solid #ddd;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 1em;
        }

        label {
            font-weight: bold;
            display: block;
            margin-bottom: 5px;
            color: var(--secondary-color);
        }

        /* ------------------ Dashboard Styling ------------------ */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 25px;
            margin-top: 25px;
            text-align: center;
        }

        .card {
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
        }

        .card h3 {
            margin-top: 0;
            font-size: 1.2em;
        }

        .card h1 {
            font-size: 3em;
            margin: 10px 0;
        }

        .card.all-reports { background: #e3f2fd; color: var(--secondary-color); }
        .card.pending { background: var(--warning-color); color: var(--text-dark); }
        .card.repaired { background: var(--success-color); color: white; }

        /* ------------------ Table Styling ------------------ */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            border: 1px solid #e0e0e0;
            padding: 12px;
            text-align: left;
        }

        th {
            background-color: var(--secondary-color);
            color: white;
            font-weight: bold;
        }

        tr:nth-child(even) {
            background-color: #f9f9f9;
        }

        /* ------------------ Custom Login Styles ------------------ */
        #login-form-container {
            width: 90%;
            max-width: 400px;
            padding: 30px;
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
        }

        #login-form-container header {
            margin: -30px -30px 20px -30px;
            border-radius: 12px 12px 0 0;
        }
    </style>
</head>
<body>

    <div id="login-screen" class="app-container">
        <div id="login-form-container">
            <header>
                <h2>🔑 เข้าสู่ระบบระบบแจ้งซ่อม</h2>
            </header>
            <form id="login-form">
                <div class="form-group">
                    <label for="username">ชื่อผู้ใช้:</label>
                    <input type="text" id="username" placeholder="รหัสพนักงาน/ครู" required>
                </div>
                <div class="form-group">
                    <label for="password">รหัสผ่าน:</label>
                    <input type="password" id="password" placeholder="รหัสผ่าน" required>
                </div>
                <button type="button" class="btn" onclick="handleLogin()">เข้าสู่ระบบ</button>
            </form>
        </div>
    </div>

    <div id="main-app" style="display: none;">
        <header>
            <h1>🔧 ระบบสำรวจความเสียหายและแจ้งซ่อม</h1>
            <p>โรงเรียน (ใส่ชื่อโรงเรียนของคุณที่นี่)</p>
        </header>
        
        <div class="main-content">
            
            <h2 style="color: var(--primary-color);">📝 กรอกแบบฟอร์มแจ้งซ่อม</h2>
            <form action="#" method="POST">
                <div class="form-group"><label for="report_date">วันที่แจ้ง:</label><input type="date" id="report_date" required></div>
                <div class="form-group"><label for="reporter_name">ผู้แจ้ง:</label><input type="text" id="reporter_name" placeholder="ชื่อ-นามสกุล/รหัสผู้ใช้" required></div>
                <div class="form-group">
                    <label for="room_number">ห้อง/พื้นที่ที่เสียหาย:</label>
                    <select id="room_number" required>
                        <option value="">-- เลือกห้อง --</option>
                        <option value="101">ห้องเรียน 101</option>
                        <option value="Lab">ห้องปฏิบัติการ</option>
                        <option value="Office">ห้องธุรการ</option>
                        <option value="Other">อื่นๆ</option>
                    </select>
                </div>
                <div class="form-group"><label for="equipment">อุปกรณ์ที่เสียหาย/แจ้งซ่อม:</label><input type="text" id="equipment" placeholder="เช่น เครื่องปรับอากาศ, โปรเจคเตอร์" required></div>
                <div class="form-group"><label for="damage_details">รายละเอียดความเสียหาย:</label><textarea id="damage_details" rows="4" placeholder="อธิบายอาการและสิ่งที่เกิดขึ้นโดยละเอียด" required></textarea></div>
                <button type="submit" class="btn">ส่งแบบฟอร์มแจ้งซ่อม</button>

                <hr style="margin: 30px 0;">
                <h3 style="color: var(--secondary-color);">🛠️ ข้อมูลการดำเนินการ (สำหรับเจ้าหน้าที่)</h3>
                
                <div class="form-group"><label for="acknowledgement_staff">ลงชื่อคนรับทราบข้อมูล:</label><input type="text" id="acknowledgement_staff" placeholder="ชื่อเจ้าหน้าที่ผู้รับเรื่อง"></div>
                <div class="form-group"><label for="repair_completion_date">วันที่ซ่อมเสร็จสิ้น:</label><input type="date" id="repair_completion_date"></div>
            </form>
            
            <hr style="margin: 40px 0;">

            <h2 style="color: var(--primary-color);">📊 สรุปภาพรวมการแจ้งซ่อม</h2>
            
            <div class="dashboard-grid">
                <div class="card all-reports"><h3>แจ้งซ่อมทั้งหมด</h3><h1>15</h1><p>รายการ</p></div>
                <div class="card pending"><h3>รายการที่รอซ่อม</h3><h1>8</h1><p>รายการที่กำลังดำเนินการ</p></div>
                <div class="card repaired"><h3>✅ ซ่อมเสร็จสิ้นแล้ว</h3><h1>7</h1><p>รายการ</p></div>
            </div>

            <h3 style="margin-top: 40px; color: var(--secondary-color);">รายละเอียดการแจ้งซ่อมทั้งหมด</h3>
            
            <div style="overflow-x: auto;">
                <table>
                    <thead>
                        <tr>
                            <th>วันที่แจ้ง</th><th>ผู้แจ้ง</th><th>ห้อง</th><th>อุปกรณ์</th><th>รายละเอียด</th><th>สถานะ</th><th>วันที่ซ่อมเสร็จ</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>2025-12-01</td><td>สมศรี</td><td>101</td><td>พัดลมเพดาน</td><td>เปิดไม่ติด มีเสียงดัง</td>
                            <td style="color: var(--success-color); font-weight: bold;">ซ่อมแล้ว</td><td>2025-12-03</td>
                        </tr>
                        <tr>
                            <td>2025-12-02</td><td>มานะ</td><td>Lab</td><td>จอคอมพิวเตอร์</td><td>หน้าจอเป็นเส้นสีเขียว</td>
                            <td style="color: var(--warning-color); font-weight: bold;">รอซ่อม</td><td>-</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            
        </div>
    </div>

    <script>
        function handleLogin() {
            // ในการใช้งานจริง: ตรวจสอบ username/password กับฐานข้อมูลที่นี่
            
            // ในโค้ดตัวอย่างนี้: ถือว่าล็อกอินสำเร็จเสมอ
            const loginScreen = document.getElementById('login-screen');
            const mainApp = document.getElementById('main-app');

            // ซ่อนหน้าล็อกอิน และแสดงหน้าหลัก
            loginScreen.style.display = 'none';
            mainApp.style.display = 'block';

            // ต้องย้าย body style ออกจากการจัดให้อยู่ตรงกลางของหน้าล็อกอิน
            document.body.style.display = 'block';
            document.body.style.height = 'auto';
            document.body.style.alignItems = 'stretch';
        }
    </script>
</body>
</html># schoolfix
