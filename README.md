# additional_functions
1- Online user

1- Online user : tap number user online in this page

1-1- Create table SQL
```
CREATE TABLE online_users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    session_id VARCHAR(255) NOT NULL,
    last_activity TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

1-2- copy this code PHP
```
session_start();
$session_id = session_id();
$pdo = new PDO("mysql:host=localhost;dbname=your_database", "username", "password");

// تسجيل المستخدم أو تحديث نشاطه
$time_limit = 300; // 5 دقائق
$stmt = $pdo->prepare("REPLACE INTO online_users (session_id, last_activity) VALUES (:session_id, NOW())");
$stmt->execute(['session_id' => $session_id]);

// إزالة المستخدمين غير النشطين
$stmt = $pdo->prepare("DELETE FROM online_users WHERE last_activity < NOW() - INTERVAL :time_limit SECOND");
$stmt->execute(['time_limit' => $time_limit]);

// حساب عدد المستخدمين النشطين
$stmt = $pdo->query("SELECT COUNT(*) AS online_count FROM online_users");
$online_count = $stmt->fetch(PDO::FETCH_ASSOC)['online_count'];

echo "عدد المستخدمين الحاليين: " . $online_count;
```

1-3- copy this code JS for update
```
setInterval(() => {
    fetch("update_online.php")
        .then(response => response.text())
        .then(data => {
            document.getElementById("online-count").innerText = data;
        });
}, 30000); // تحديث كل 30 ثانية
```

1-4- copy HTML
```
<div>عدد الأشخاص على الصفحة: <span id="online-count">0</span></div>
```
