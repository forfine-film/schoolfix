<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบแจ้งซ่อมอุปกรณ์โรงเรียน</title>
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Prompt', sans-serif;
            background-color: #f0f4f8; /* สีพื้นหลังอ่อน ๆ */
        }
        /* กำหนดรูปแบบหลักเพื่อให้หน้าจอ login อยู่ตรงกลาง */
        #login-screen {
            background: linear-gradient(135deg, #0077c2 0%, #00bcd4 100%); /* Blue-Cyan Gradient */
        }
        #login-form-container {
            width: 90%;
            max-width: 400px;
            padding: 30px;
            background-color: #ffffff;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            position: relative;
        }
        #login-form-container header {
            background-color: #0077c2; /* สีน้ำเงินเข้ม */
            color: white;
            padding: 20px 0;
            text-align: center;
            margin: -30px -30px 20px -30px;
            border-radius: 15px 15px 0 0;
        }

        /* สไตล์สำหรับปุ่ม */
        .btn-primary {
            background-color: #00bcd4; /* Cyan สดใส */
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1em;
            font-weight: 600;
            transition: background-color 0.3s;
        }
        .btn-primary:hover {
            background-color: #00a0b2;
        }

        /* สไตล์สำหรับแอปหลัก (Dashboard/Form) */
        .sidebar {
            background: linear-gradient(180deg, #0077c2 0%, #00a0b2 100%);
            width: 250px;
            min-height: 100vh;
            color: white;
        }
        .main-header {
            background-color: #0077c2;
        }
        .card-all {
            background: #e0f7fa; /* Light Cyan */
            border-left: 5px solid #00bcd4;
        }
        .card-pending {
            background: #fff3e0; /* Light Orange */
            border-left: 5px solid #ff9800;
        }
        .card-completed {
            background: #e8f5e9; /* Light Green */
            border-left: 5px solid #4caf50;
        }
        .table-header {
            background-color: #0077c2;
        }
    </style>
</head>
<body class="p-0 m-0">

    <div id="login-screen" class="app-container w-full h-screen flex justify-center items-center">
        <div id="login-form-container">
            <header>
                <h2 class="text-2xl font-bold">🔑 เข้าสู่ระบบระบบแจ้งซ่อม</h2>
            </header>
            <form id="login-form">
                <div class="mb-5">
                    <label for="username" class="block text-sm font-medium text-gray-700 mb-1">ชื่อผู้ใช้:</label>
                    <input type="text" id="username" placeholder="รหัสพนักงาน/ครู" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500">
                </div>
                <div class="mb-5">
                    <label for="password" class="block text-sm font-medium text-gray-700 mb-1">รหัสผ่าน:</label>
                    <input type="password" id="password" placeholder="รหัสผ่าน" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500">
                </div>
                <button type="button" class="btn-primary w-full mt-4" onclick="handleLogin()">เข้าสู่ระบบ</button>
            </form>
        </div>
    </div>

    <div id="main-app" class="flex" style="display: none;">
        
        <div class="sidebar p-5 shadow-lg">
            <h2 class="text-3xl font-bold mb-8">SchoolFix</h2>
            <nav>
                <button onclick="showSection('dashboard')" class="w-full text-left p-3 rounded-lg hover:bg-white hover:bg-opacity-20 transition duration-150 ease-in-out font-semibold mb-2 bg-white bg-opacity-10">📊 สรุปภาพรวม</button>
                <button onclick="showSection('repair-form')" class="w-full text-left p-3 rounded-lg hover:bg-white hover:bg-opacity-20 transition duration-150 ease-in-out font-semibold mb-2">📝 แจ้งซ่อม</button>
                <button onclick="showSection('repair-list')" class="w-full text-left p-3 rounded-lg hover:bg-white hover:bg-opacity-20 transition duration-150 ease-in-out font-semibold mb-2">📋 รายการแจ้งซ่อม</button>
                <button onclick="handleLogout()" class="w-full text-left p-3 rounded-lg hover:bg-red-400 transition duration-150 ease-in-out font-semibold mt-10">🚪 ออกจากระบบ</button>
            </nav>
        </div>

        <div class="flex-1 p-8">
            <header class="main-header text-white p-5 rounded-lg mb-8 shadow-md">
                <h1 class="text-3xl font-bold">🔧 ระบบสำรวจความเสียหายและแจ้งซ่อม</h1>
                <p class="text-white opacity-90">โรงเรียน (ชื่อโรงเรียน)</p>
            </header>

            <div id="dashboard" class="main-section">
                <h2 class="text-2xl font-bold text-gray-800 mb-6 border-b pb-2">📊 สรุปภาพรวมการแจ้งซ่อม</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                    <div class="p-5 rounded-lg shadow-md card-all">
                        <h3 class="text-lg font-semibold text-[#0077c2] mb-2">แจ้งซ่อมทั้งหมด</h3>
                        <p class="text-4xl font-bold text-[#0077c2]">15</p>
                        <p class="text-sm text-gray-600">รายการ</p>
                    </div>
                    
                    <div class="p-5 rounded-lg shadow-md card-pending">
                        <h3 class="text-lg font-semibold text-[#ff9800] mb-2">รายการที่รอซ่อม</h3>
                        <p class="text-4xl font-bold text-[#ff9800]">8</p>
                        <p class="text-sm text-gray-600">รายการที่กำลังดำเนินการ</p>
                    </div>
                    
                    <div class="p-5 rounded-lg shadow-md card-completed">
                        <h3 class="text-lg font-semibold text-[#4caf50] mb-2">✅ ซ่อมเสร็จสิ้นแล้ว</h3>
                        <p class="text-4xl font-bold text-[#4caf50]">7</p>
                        <p class="text-sm text-gray-600">รายการ</p>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                    <h3 class="text-xl font-bold text-gray-800 mb-4">รายงานสรุปจาก AI (Mock-up)</h3>
                    <p class="text-gray-700">จากการวิเคราะห์ข้อมูล ณ วันที่ 3 ธ.ค. 2568 พบว่า **ปัญหาแอร์เสีย** เป็นประเด็นเร่งด่วนที่สุด คิดเป็น 40% ของงานที่รอซ่อมทั้งหมด และปัญหา **ระบบไฟฟ้าห้องปฏิบัติการ** มีแนวโน้มเกิดขึ้นซ้ำบ่อยในช่วงเดือนที่ผ่านมา ควรเร่งจัดสรรงบประมาณเพื่อซ่อมบำรุงเชิงป้องกันในส่วนนี้</p>
                    <button class="btn-primary mt-4 py-2 px-4 text-sm">สร้างรายงาน AI ฉบับเต็ม</button>
                </div>
            </div>

            <div id="repair-form" class="main-section hidden">
                <h2 class="text-2xl font-bold text-gray-800 mb-6 border-b pb-2">📝 กรอกแบบฟอร์มแจ้งซ่อม</h2>
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <form onsubmit="event.preventDefault(); alert('ส่งแบบฟอร์มสำเร็จ! (ต้องใช้โค้ดหลังบ้านในการบันทึกข้อมูลจริง)');">
                        <h3 class="text-xl font-semibold mb-4 text-[#0077c2]">ข้อมูลผู้แจ้งและสถานที่</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="mb-4">
                                <label for="report_date" class="block text-sm font-medium text-gray-700 mb-1">วันที่แจ้ง:</label>
                                <input type="date" id="report_date" required class="w-full p-3 border border-gray-300 rounded-lg">
                            </div>
                            <div class="mb-4">
                                <label for="reporter_name" class="block text-sm font-medium text-gray-700 mb-1">ผู้แจ้ง (ครู/นักเรียน/เจ้าหน้าที่):</label>
                                <input type="text" id="reporter_name" placeholder="ชื่อ-นามสกุล/รหัสผู้ใช้" required class="w-full p-3 border border-gray-300 rounded-lg">
                            </div>
                            <div class="mb-4 col-span-1 md:col-span-2">
                                <label for="room_number" class="block text-sm font-medium text-gray-700 mb-1">ห้อง/พื้นที่ที่เสียหาย:</label>
                                <select id="room_number" required class="w-full p-3 border border-gray-300 rounded-lg">
                                    <option value="">-- เลือกห้อง/พื้นที่ --</option>
                                    <option value="101">ห้องเรียน 101</option>
                                    <option value="Lab">ห้องปฏิบัติการวิทยาศาสตร์</option>
                                    <option value="Office">ห้องธุรการ</option>
                                    <option value="Bathroom">ห้องน้ำ/สุขา</option>
                                    <option value="Other">อื่นๆ (โปรดระบุในรายละเอียด)</option>
                                </select>
                            </div>
                        </div>

                        <h3 class="text-xl font-semibold mb-4 mt-6 text-[#0077c2]">รายละเอียดความเสียหาย</h3>
                        <div class="mb-4">
                            <label for="equipment" class="block text-sm font-medium text-gray-700 mb-1">อุปกรณ์ที่เสียหาย/แจ้งซ่อม (Dropdown):</label>
                            <select id="equipment" required class="w-full p-3 border border-gray-300 rounded-lg">
                                <option value="">-- เลือกประเภทอุปกรณ์ --</option>
                                <option value="AC">เครื่องปรับอากาศ/พัดลม</option>
                                <option value="IT">คอมพิวเตอร์/โปรเจคเตอร์</option>
                                <option value="Electric">ไฟฟ้า/หลอดไฟ/ปลั๊ก</option>
                                <option value="Plumbing">ประปา/ก๊อกน้ำ/ท่อ</option>
                                <option value="Furniture">เฟอร์นิเจอร์ (โต๊ะ/เก้าอี้)</option>
                                <option value="Facility">อาคาร/ประตู/หน้าต่าง</option>
                                <option value="Other">อื่นๆ</option>
                            </select>
                        </div>
                        <div class="mb-4">
                            <label for="damage_details" class="block text-sm font-medium text-gray-700 mb-1">รายละเอียดความเสียหาย:</label>
                            <textarea id="damage_details" rows="4" placeholder="อธิบายอาการและสิ่งที่เกิดขึ้นโดยละเอียด" required class="w-full p-3 border border-gray-300 rounded-lg"></textarea>
                        </div>

                        <button type="submit" class="btn-primary w-full mt-6">ส่งแบบฟอร์มแจ้งซ่อม</button>
                    </form>

                    <hr class="my-8">
                    <h3 class="text-xl font-semibold mb-4 text-[#0077c2]">🛠️ ข้อมูลการดำเนินการ (สำหรับเจ้าหน้าที่)</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="mb-4">
                            <label for="acknowledgement_staff" class="block text-sm font-medium text-gray-700 mb-1">ลงชื่อคนรับทราบข้อมูล:</label>
                            <input type="text" id="acknowledgement_staff" placeholder="ชื่อเจ้าหน้าที่ผู้รับเรื่อง" class="w-full p-3 border border-gray-300 rounded-lg">
                        </div>
                        <div class="mb-4">
                            <label for="repair_completion_date" class="block text-sm font-medium text-gray-700 mb-1">วันที่ซ่อมเสร็จสิ้น:</label>
                            <input type="date" id="repair_completion_date" class="w-full p-3 border border-gray-300 rounded-lg">
                        </div>
                    </div>
                </div>
            </div>

            <div id="repair-list" class="main-section hidden">
                <h2 class="text-2xl font-bold text-gray-800 mb-6 border-b pb-2">📋 รายการแจ้งซ่อมทั้งหมด</h2>

                <div class="bg-white p-6 rounded-lg shadow-md overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead class="table-header text-white">
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider rounded-tl-lg">วันที่แจ้ง</th>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider">ผู้แจ้ง</th>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider">ห้อง</th>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider">อุปกรณ์</th>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider">รายละเอียด</th>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider">สถานะ</th>
                                <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider rounded-tr-lg">วันที่ซ่อมเสร็จ</th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200">
                            <tr class="hover:bg-gray-50 transition duration-150">
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">2025-12-01</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">สมศรี (ครู)</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">101</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">เครื่องปรับอากาศ</td>
                                <td class="px-6 py-4 text-sm text-gray-700">มีแต่น้ำหยด ไม่มีความเย็น</td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">✅ ซ่อมเสร็จแล้ว</span>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">2025-12-03</td>
                            </tr>
                            <tr class="hover:bg-gray-50 transition duration-150">
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">2025-12-02</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">มานะ (นร.)</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">Lab</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">จอคอมพิวเตอร์</td>
                                <td class="px-6 py-4 text-sm text-gray-700">หน้าจอเป็นเส้นสีเขียว เปิดไม่ติด</td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-yellow-100 text-yellow-800">⏳ รอดำเนินการ</span>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">-</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // ตั้งค่าวันที่ปัจจุบันในฟอร์มแจ้งซ่อมโดยอัตโนมัติ
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('report_date').value = today;
        });

        function handleLogin() {
            // ในโค้ดตัวอย่างนี้: ถือว่าล็อกอินสำเร็จเสมอและสลับหน้า
            const loginScreen = document.getElementById('login-screen');
            const mainApp = document.getElementById('main-app');

            // ซ่อนหน้าล็อกอิน และแสดงหน้าหลัก
            loginScreen.style.display = 'none';
            mainApp.style.display = 'flex'; // ใช้ flex สำหรับ layout ที่มี sidebar

            // ให้แสดงส่วน Dashboard เป็นค่าเริ่มต้น
            showSection('dashboard');
        }

        function handleLogout() {
            const loginScreen = document.getElementById('login-screen');
            const mainApp = document.getElementById('main-app');
            
            mainApp.style.display = 'none';
            loginScreen.style.display = 'flex'; // กลับไปหน้าล็อกอิน
        }

        function showSection(sectionId) {
            const sections = document.querySelectorAll('.main-section');
            sections.forEach(section => {
                section.classList.add('hidden');
            });
            document.getElementById(sectionId).classList.remove('hidden');
        }
    </script>
</body>
</html>
