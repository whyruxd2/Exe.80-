// ตัวแปรสำหรับเก็บสถานะ (วางไว้ก่อนเข้าลูป while หลักของโปรแกรม)
static char accessKey[64] = "";
static float timeLeft = 86397.0f; // 23:59:57 วินาที
static std::string statusMsg = "AWAITING AUTHENTICATION";

// --- ส่วนแสดงผล UI (วางไว้ในลูปการวาดเฟรมของ ImGui) ---
ImGui::SetNextWindowSize(ImVec2(750, 420), ImGuiCond_FirstUseEver);
ImGui::PushStyleColor(ImGuiCol_WindowBg, ImVec4(0.04f, 0.04f, 0.04f, 1.0f)); // พื้นหลังสีดำสนิท

if (ImGui::Begin("SECURITY ALERT SIMULATION (SAFE MODE)", nullptr, ImGuiWindowFlags_NoCollapse)) {
    
    // 1. หัวข้อสีแดง
    ImGui::PushStyleColor(ImGuiCol_Text, ImVec4(0.90f, 0.25f, 0.25f, 1.0f));
    ImGui::SetCursorPosX((ImGui::GetWindowWidth() - ImGui::CalcTextSize("[ SECURITY ALERT - ACCESS RESTRICTED ]").x) * 0.5f);
    ImGui::Text("[ SECURITY ALERT - ACCESS RESTRICTED ]");
    ImGui::PopStyleColor();

    ImGui::Spacing();
    ImGui::Separator();
    ImGui::Spacing();

    // 2. กล่องข้อความสีเขียว
    ImGui::BeginChild("NoticeBox", ImVec2(0, 90), true);
    ImGui::PushStyleColor(ImGuiCol_Text, ImVec4(0.40f, 0.90f, 0.40f, 1.0f));
    ImGui::Text("SIMULATION NOTICE: Harmless UI demonstration from GitHub Dear ImGui.");
    ImGui::Text("STATUS: Locked for visual testing only.");
    ImGui::Text("EXIT: Click [X], press ESC, or enter '1234' to close safely.");
    ImGui::PopStyleColor();
    ImGui::EndChild();

    ImGui::Spacing();

    // 3. นาฬิกานับถอยหลังสีส้มทอง
    timeLeft -= ImGui::GetIO().DeltaTime;
    if (timeLeft < 0) timeLeft = 0;
    int hrs = (int)timeLeft / 3600;
    int mins = ((int)timeLeft % 3600) / 60;
    int secs = (int)timeLeft % 60;

    char timerBuf[32];
    snprintf(timerBuf, sizeof(timerBuf), "%02d:%02d:%02d", hrs, mins, secs);

    ImGui::SetCursorPosX((ImGui::GetWindowWidth() - ImGui::CalcTextSize("TIME TILL SIMULATION END").x) * 0.5f);
    ImGui::TextDisabled("TIME TILL SIMULATION END");

    ImGui::SetWindowFontScale(2.2f); // ขยายขนาดตัวเลขนาฬิกา
    ImGui::PushStyleColor(ImGuiCol_Text, ImVec4(0.95f, 0.70f, 0.30f, 1.0f));
    ImGui::SetCursorPosX((ImGui::GetWindowWidth() - ImGui::CalcTextSize(timerBuf).x) * 0.5f);
    ImGui::Text("%s", timerBuf);
    ImGui::PopStyleColor();
    ImGui::SetWindowFontScale(1.0f);

    ImGui::Spacing();
    ImGui::Separator();
    ImGui::Spacing();

    // 4. ช่องกรอกรหัส และปุ่มสีเขียว
    ImGui::SetCursorPosX((ImGui::GetWindowWidth() - 360) * 0.5f);
    ImGui::SetNextItemWidth(220);
    ImGui::InputText("##key", accessKey, IM_ARRAYSIZE(accessKey));
    
    ImGui::SameLine();
    ImGui::PushStyleColor(ImGuiCol_Button, ImVec4(0.30f, 0.85f, 0.45f, 1.0f));
    ImGui::PushStyleColor(ImGuiCol_Text, ImVec4(0.0f, 0.0f, 0.0f, 1.0f));
    if (ImGui::Button("AUTHENTICATE", ImVec2(130, 0))) {
        if (std::string(accessKey) == "1234") {
            exit(0); // ปลดล็อกและปิดโปรแกรมทันที
        } else {
            statusMsg = "INVALID KEY - TRY '1234'";
        }
    }
    ImGui::PopStyleColor(2);

    // ข้อความสถานะด้านล่าง
    ImGui::SetCursorPosX((ImGui::GetWindowWidth() - ImGui::CalcTextSize(statusMsg.c_str()).x) * 0.5f);
    ImGui::TextDisabled("%s", statusMsg.c_str());
}
ImGui::End();
ImGui::PopStyleColor();
