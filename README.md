#include <iostream>
#include <string>

using namespace std;

int main() {
    string correctPassword = "MyPassword123"; // รหัสผ่านที่ตั้งไว้
    string inputPassword;
    int maxAttempts = 3; // จำกัดจำนวนครั้งที่กรอกผิดได้

    cout << "=== ระบบตรวจสอบรหัสผ่าน ===" << endl;

    for (int attempt = 1; attempt <= maxAttempts; attempt++) {
        cout << "กรุณากรอกรหัสผ่าน (ครั้งที่ " << attempt << "/" << maxAttempts << "): ";
        cin >> inputPassword;

        if (inputPassword == correctPassword) {
            cout << "เข้าสู่ระบบสำเร็จ! ยินดีต้อนรับ" << endl;
            return 0; // จบการทำงานเมื่อรหัสถูกต้อง
        } else {
            cout << "รหัสผ่านไม่ถูกต้อง!" << endl;
        }
    }

    cout << "คุณกรอกรหัสผ่านผิดเกินจำนวนครั้งที่กำหนด ระบบถูกล็อกชั่วคราว" << endl;
    return 0;
}
