# Homework
ระบบติดตามงาน
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบตรวจสอบการส่งงานนักเรียน</title>
    <!-- นำเข้าฟอนต์ Kanit จาก Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Kanit', sans-serif;
        }
        body {
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .container {
            background: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 500px;
        }
        h2 {
            text-align: center;
            color: #1a73e8;
            margin-top: 0;
        }
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        input[type="text"] {
            flex: 1;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            outline: none;
            transition: border-color 0.3s;
        }
        input[type="text"]:focus {
            border-color: #1a73e8;
        }
        button {
            padding: 12px 20px;
            background-color: #1a73e8;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #1557b0;
        }
        .result-box {
            display: none;
            margin-top: 20px;
            border-top: 2px solid #eee;
            padding-top: 15px;
        }
        .student-info {
            background-color: #e8f0fe;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 15px;
            color: #1967d2;
        }
        .student-info p {
            margin: 5px 0;
        }
        ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        li {
            padding: 10px 12px;
            background-color: #fff5f5;
            border-left: 4px solid #ea4335;
            margin-bottom: 8px;
            border-radius: 4px;
            color: #d93025;
        }
        .no-missing {
            background-color: #e6f4ea;
            border-left: 4px solid #34a853;
            color: #137333;
            padding: 12px;
            border-radius: 4px;
            text-align: center;
        }
        .error {
            color: #d93025;
            text-align: center;
            display: none;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>ระบบตรวจสอบการส่งงาน</h2>
    
    <div class="input-group">
        <input type="text" id="studentId" placeholder="กรอกรหัสนักเรียน (เช่น 1001)" onkeypress="handleKeyPress(event)">
        <button onclick="checkWork()">ค้นหา</button>
    </div>

    <div id="errorMessage" class="error"></div>

    <div id="resultBox" class="result-box">
        <div class="student-info">
            <p><strong>รหัสนักเรียน:</strong> <span id="resId"></span></p>
            <p><strong>ชื่อ-นามสกุล:</strong> <span id="resName"></span></p>
        </div>

        <h3>รายการงานที่ยังไม่ได้ส่ง:</h3>
        <div id="missingWorkContainer"></div>
    </div>
</div>

<script>
    // ข้อมูลจำลองนักเรียนและรายการงานที่ขาด
    const studentData = {
        "1001": {
            name: "นายสมชาย ใจดี",
            missingWork: ["ใบงานที่ 1: การเขียนโปรแกรมเบื้องต้น", "การบ้าน: โครงสร้างข้อมูล"]
        },
        "1002": {
            name: "นางสาวสมหญิง รักเรียน",
            missingWork: [] // ไม่ขาดงาน
        },
        "1003": {
            name: "นายวิชัย มุ่งมั่น",
            missingWork: ["รายงานการทดลองที่ 2"]
        }
    };

    function checkWork() {
        const inputId = document.getElementById('studentId').value.trim();
        const resultBox = document.getElementById('resultBox');
        const errorMsg = document.getElementById('errorMessage');
        const resId = document.getElementById('resId');
        const resName = document.getElementById('resName');
        const missingContainer = document.getElementById('missingWorkContainer');

        // ซ่อนผลลัพธ์เดิมก่อน
        resultBox.style.display = 'none';
        errorMsg.style.display = 'none';

        if (inputId === "") {
            errorMsg.innerText = "กรุณากรอกรหัสนักเรียน";
            errorMsg.style.display = 'block';
            return;
        }

        const student = studentData[inputId];

        if (student) {
            resId.innerText = inputId;
            resName.innerText = student.name;

            // ตรวจสอบว่ามีงานค้างหรือไม่
            if (student.missingWork.length > 0) {
                let html = '<ul>';
                student.missingWork.forEach(work => {
                    html += `<li>❌ ${work}</li>`;
                });
                html += '</ul>';
                missingContainer.innerHTML = html;
            } else {
                missingContainer.innerHTML = '<div class="no-missing">🎉 ส่งงานครบทุกชิ้นแล้ว!</div>';
            }

            resultBox.style.display = 'block';
        } else {
            errorMsg.innerText = "ไม่พบข้อมูลรหัสนักเรียนนี้ในระบบ";
            errorMsg.style.display = 'block';
        }
    }

    // กด Enter ในช่องกรอกรหัสเพื่อค้นหาได้ทันที
    function handleKeyPress(event) {
        if (event.key === 'Enter') {
            checkWork();
        }
    }
</script>

</body>
</html>
