# Tutorial buat project baru di Firebase
- ke web https://firebase.google.com/
- klik Go to console jika sudah login
- Klik Create a new Firebase project
- Kasih nama Project -> klik Continue -> klik Create Project dan tunggu sampai selesai -> klik Continue
  
# Tutorial buat app web di Firebase
- klik "+ Add app"
- Pilih Web
- masukkan App nickname -> Register app
- ambil apiKey untuk digunakan di Environtment Postman

### Tutor lewat Gambar
<p align="center">  
  <img  src="https://github.com/user-attachments/assets/56f15130-c568-48d4-9b87-fd37d263510f" />
  <br>
  <img  src="https://github.com/user-attachments/assets/08b9eae5-8e5e-47ce-b053-b4f39841292b" />
  <br>
  <img  src="https://github.com/user-attachments/assets/35927287-6256-410d-a2c3-21caa0d9dfef" />
  <br>
  <img  src="https://github.com/user-attachments/assets/bbc76b6a-4dd8-4621-9809-9d38b560ab46" />
</p>

---


# Tutorial Authentication
- buka tab Auhentication -> Get Started
- pilih Email/Password
- Centang Enable Email/Password -> Save

### Tutor lewat Gambar

<p align="center">  
  <img  src="https://github.com/user-attachments/assets/5974f038-1035-42f2-afef-4d53a7622eb0" />
  <br>
  <img  src="https://github.com/user-attachments/assets/41cac920-1899-4b2f-94fb-77232ecc9a2a" />
  <br>
  <img  src="https://github.com/user-attachments/assets/c6256d30-6178-4efd-96be-bcbf7d0decf8" />
  <br>
  <img  src="https://github.com/user-attachments/assets/7d7fea6e-df72-4b08-bc33-f0c7e9022f6c" />
</p>

---

# Tutorial setup Environment di Postman
- Buka Postman
- klik Environtments di sebelah kiri
- klik tanda "+" (Create new environment)
- masukkan Variable dan Value(sesuaikan punya masing masing) tabel di bawah
  
| Variable | Initial Value | Keterangan |
|---------|---------|---------|
| FIREBASE_API_KEY  | AIzaSyB_xxx...  | Web API Key dari Firebase Console  |
| FIREBASE_ID_TOKEN  |   | Diisi otomatis setelah login (via Test script)  |
| BACKEND_BASE_URL  | http://localhost:8080/v1 | Base URL backend kamu  |
| BACKEND_TOKEN  |   | Token JWT dari backend (diisi setelah verify)  |
| USER_EMAIL  | test@example.com | Email untuk testing  |
| USER_PASSWORD  | Test@12345 | Password untuk testing  |

### Kayak gini
<p align="center">
  <img src="https://github.com/user-attachments/assets/ce0f7f62-d2ae-43aa-b715-1ce9edc596db"/>
</p>

---

# Step 1 - Tutorial Register (Membuat Akun Baru)

### ENDPOINT
- POST
```bash
https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}
```

### HEADERS

| Key | Value | Keterangan |
|---------|---------|---------|
| Content-Type  | application/json | Wajib untuk semua Firebase REST API  |

### Request Body (raw JSON)
```bash
{
  "email": "{{USER_EMAIL}}",
  "password": "{{USER_PASSWORD}}",
  "returnSecureToken": true
}
```

### Response
- Sukses
```bash
Response: 200 OK
{
  "kind": "identitytoolkit#SignupNewUserResponse",
  "localId": "aBcDeFgHiJkLmN",
  "email": "test@example.com",
  "displayName": "",
  "idToken": "eyJhbGciOiJSUzI1...",
  "registered": false,
  "refreshToken": "AMf-vBxK...",
  "expiresIn": "3600"
}
```

- Error
```bash
Response: 400 Bad Request
{
  "error": {
    "code": 400,
    "message": "EMAIL_EXISTS",
    "status": "INVALID_ARGUMENT"
  }
}
```


| Error Code | Artinya | Solusi |
|---------|---------|---------|
| EMAIL_EXISTS  | Email sudah terdaftar di Firebase | Gunakan email lain atau cek apakah sudah register  |
| INVALID_EMAIL  | Format email salah | Pastikan format email benar:user@domain.com  |
| WEAK_PASSWORD  | Password kurang dari 6 karakter | Gunakan password minimal 6 karakter  |
| OPERATION_NOT_ALLOWED  | Email/Password auth belum diaktifkan | Aktifkan di Firebase Console → Authentication → Sign-in method  |
| TOO_MANY_ATTEMPTS_TRY_LATER  | Terlalu banyak percobaan | Tunggu beberapa menit, atau clear IP block di Firebase Console  |

### Postman Test Script — Auto-save Token
- Copy paste ke tab "Tests" di Postman agar idToken tersimpan otomatis:
```bash
// Postman → Tests tab:
const json = pm.response.json();
if (pm.response.code === 200) {
  pm.environment.set("FIREBASE_ID_TOKEN", json.idToken);
  pm.environment.set("FIREBASE_LOCAL_ID", json.localId);
  pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken);
  console.log("Register sukses. UID:", json.localId);
  console.log("PERHATIAN: Email belum diverifikasi. Lanjut ke Step 2.");
} else {
  console.log("Register gagal:", json.error.message);
}
```

### Praktek di Postman
- Buka Postman
- Klik tanda + Create new collection -> Blank Collection
- di dalam Collection baru klik tanda + (add New Request) default method nya GET
- ganti method GET ke POST dan copy paste ENDPOINT Register ke form yang kosong
- ke tab Headers untuk isi Key dan Value nya
- lalu ke tab Body pilih raw -> masukkan raw JSON yang sudah disediakan di atas
- ke tab Scripts masukan Postman Test Script
- Klik Send lalu keluar Response nya

### Tutorial Step By Step

<p align="center">  
  <img width="" src="https://github.com/user-attachments/assets/b049c205-2203-44d2-be3c-5225c06c975c" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/a6f84fb3-c1a7-45b9-8d68-3b62975750b9" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/fb5310ea-dec1-4d49-ab0c-59c85fa89d25" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/104fa140-3de6-46ef-8cf0-aa2888735cb9" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/68487ae9-9722-4d7d-82e5-6b94aab47046" />
  
</p>

---

# Step 2 - Tutorial Kirim Email Verifikasi

### ENDPOINT
- POST
```bash
https://identitytoolkit.googleapis.com/v1/accounts:sendOobCode?key={{FIREBASE_API_KEY}}
```

### HEADERS

| Key | Value | Keterangan |
|---------|---------|---------|
| Content-Type  | application/json | Wajib untuk semua Firebase REST API  |

### Request Body (raw JSON)
```bash
{
  "requestType": "VERIFY_EMAIL",
  "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```

### Response
- Sukses
```bash
Response: 200 OK
{
  "kind": "identitytoolkit#GetOobConfirmationCodeResponse",
  "email": "test@example.com" // ← Email tujuan pengiriman
}
```

- Error
```bash
Response: 400 Bad Request
{
  "error": {
    "code": 400,
    "message": "INVALID_ID_TOKEN", // ← idToken sudah kadaluarsa
    "status": "INVALID_ARGUMENT"
  }
}
```


| Error Code | Artinya | Solusi |
|---------|---------|---------|
| INVALID_ID_TOKEN  | idToken kadaluarsa atau tidak valid | Login ulang di Step 4 untuk mendapat idToken baru, lalu kirim ulang  |
| USER_NOT_FOUND  | User tidak ditemukan di Firebase | Pastikan register berhasil di Step 1 |
| TOO_MANY_ATTEMPTS_TRY_LATER  | Firebase membatasi pengiriman email | Tunggu beberapa menit  |


### Postman Test Script
```bash
// Postman → Tests tab:
if (pm.response.code === 200) {
  const json = pm.response.json();
  console.log("Email verifikasi dikirim ke:", json.email);
  console.log("Sekarang buka inbox email dan klik link verifikasi.");
  console.log("Setelah klik, lanjut ke Step 3 untuk cek status.");
} else {
  console.log("Gagal kirim email:", pm.response.json().error.message);
}
```

### Tutorial di Postman
- Kurang lebih hampir sama dengan Step 1
- kalian bikin request baru dengan method POST lalu masukan ENDPOINT step 2
- value FIREBASE_ID_TOKEN pada environments itu diambil dari value idToken pada Response Register
- Klik Send

### Tutorial Custom Template Email
- Pergi ke Firebase -> Authentication -> Templates

### Step By Step

<p align="center">  
  <img width="" src="https://github.com/user-attachments/assets/347c9460-5ab3-4ae7-baad-8c405dfc2a4e" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/528d9d67-8df2-4d2f-8641-13dccb422db8" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/daf01f50-1957-4b2f-9f07-fd02e8081046" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/14cc6e1d-af29-4d5a-be74-bc58c308f758" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/450eeb3f-39e1-4263-8e37-5122effac0e1" />
</p>

---

# Step 3 - Tutorial Cek Status Verifikasi Email

### ENDPOINT A
- POST
```bash
https://identitytoolkit.googleapis.com/v1/accounts:lookup?key={{FIREBASE_API_KEY}}
```

### Request Body (raw JSON)
```bash
{
  "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```

### Response
- Sukses
```bash
Response: 200 OK (email belum verify)
{
  "kind": "identitytoolkit#GetAccountInfoResponse",
  "users": [
      {
        "localId": "aBcDeFgHiJkLmN",
        "email": "test@example.com",
        "displayName": "Test User",
        "passwordHash": "UkVEQUNURUQ=",
        "emailVerified": false, // ← BELUM DIVERIFIKASI
        "passwordUpdatedAt": 1700000000000,
        "providerUserInfo": [ ... ],
        "validSince": "1700000000",
        "lastLoginAt": "1700000000000",
        "createdAt": "1700000000000"
      }
  ]
}
```
```bash
Response: 200 OK (email verified)
{
  "kind": "identitytoolkit#GetAccountInfoResponse",
  "users": [
      {
        "localId": "aBcDeFgHiJkLmN",
        "email": "test@example.com",
        "emailVerified": true, // ← SUDAH DIVERIFIKASI
        "lastLoginAt": "1700000000000",
        ...
      }
  ]
}
```

### Tutorial di Postman
- Kurang lebih hampir sama dengan Step sebelumnya
- kalian bikin request baru dengan method POST lalu masukan ENDPOINT step 3
- Klik Send

### Tutorial Step by Step

<p align="center">  
  <img width="" src="https://github.com/user-attachments/assets/5127b205-d375-460f-a6ef-377249a4ae5b" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/902e4f6d-4c93-4c9b-81d2-2e77f6d05a68" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/4669c1c0-7aac-4566-8fa5-40a1b88b370f" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/be7f475d-34b5-41d0-a13b-611374ba3f99" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/01a3b892-a35a-43bd-9e8e-627c0eb5b0ba" />
  <br>
</p>

---

# Step 4 - Tutorial Login dengan Email & Password

### ENDPOINT
- POST
```bash
https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key={{FIREBASE_API_KEY}}
```

### Request Body (raw JSON)
```bash
{
  "email": "{{USER_EMAIL}}",
  "password": "{{USER_PASSWORD}}",
  "returnSecureToken": true
}
```

### Response
- Sukses
```bash
Response: 200 OK
{
  "kind": "identitytoolkit#VerifyPasswordResponse",
  "localId": "aBcDeFgHiJkLmN",
  "email": "test@example.com",
  "displayName": "Test User",
  "idToken": "eyJhbGciOiJSUzI1...", // ← Firebase ID Token BARU
  "registered": true,
  "refreshToken": "AMf-vBxK...",
  "expiresIn": "3600"
}
```

- Error
```bash
Response: 400 Bad Request
{
  "error": {
    "code": 400,
    "message": "INVALID_PASSWORD",
    "errors": [{ "message": "INVALID_PASSWORD", "domain": "global" }]
  }
}
```


| Error Code | Artinya | Solusi |
|---------|---------|---------|
| INVALID_PASSWORD  | Password salah | Cek kembali password  |
| EMAIL_NOT_FOUND  | Email belum terdaftar | Lakukan register di Step 1 terlebih dahulu  |
| USER_DISABLED  | Akun di-disable oleh admin | Hubungi admin Firebase Console  |
| INVALID_EMAIL  | Format email salah | Pastikan format email benar  |
| TOO_MANY_ATTEMPTS_TRY_LATER  | Login di-block karena terlalu banyak gagal | Tunggu beberapa menit  |

### Postman Test Script — Auto-Update Token
```bash
// Postman → Tests tab:
const json = pm.response.json();
if (pm.response.code === 200) {
  // Update environment dengan idToken BARU hasil login
  pm.environment.set("FIREBASE_ID_TOKEN", json.idToken);
  pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken);
  console.log("Login berhasil. Token diperbarui.");
  console.log("Lanjut ke Step 5: kirim token ke backend.");
} else {
  console.log("Login gagal:", json.error.message);
}
```


### Tutorial di Postman
- Kurang lebih hampir sama dengan Step sebelumnya
- kalian bikin request baru dengan method POST lalu masukan ENDPOINT step 4
- Klik Send

### Contoh
<p align="center">  
  <img width="" src="https://github.com/user-attachments/assets/a17ca1cb-4ff2-4ddc-ad12-df20b88cf770" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/3476dc9b-6e99-445b-b92a-19055026015f" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/f18abfff-b2fa-42bf-9237-98eaa5ca48b5" />
  <br>
  <img width="" src="https://github.com/user-attachments/assets/03764e50-177b-409a-a548-78a70bc98e86" />
  <br>


  
</p>



































