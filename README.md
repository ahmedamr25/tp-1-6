# tp-1-6# Inspection HTTPS Android  
## SSL Pinning Bypass with Objection + Frida + Burp / mitmproxy

---



##Objectives

- Deploy and run frida-server on Android
- Attach Objection to a target application
- Disable SSL Pinning dynamically
- Configure HTTP proxy interception
- Analyze decrypted HTTPS traffic


## All-in-One Setup Script


APP_PACKAGE="com.example.app"
FRIDA_SERVER="/data/local/tmp/frida-server"

echo "[1] Checking connected device..."
adb devices

echo "[2] Uploading frida-server..."
adb push frida-server $FRIDA_SERVER

echo "[3] Setting permissions..."
adb shell chmod +x $FRIDA_SERVER

echo "[4] Starting frida-server..."
adb shell "$FRIDA_SERVER &"

sleep 2

echo "[5] Verifying Frida connection..."
frida-ps -U

echo "[6] Launching Objection and disabling SSL Pinning..."
objection -g $APP_PACKAGE explore -s "
android sslpinning disable;
android root disable;
"

echo "[7] Proxy configuration required:"
echo " - Burp / mitmproxy: 127.0.0.1:8080"
echo " - Install CA certificate on Android"
echo " - Configure Wi-Fi proxy to PC IP"

echo "[OK] Setup completed. Traffic interception should now work."
```


<img width="973" height="631" alt="Screenshot 2026-05-09 173051" src="https://github.com/user-attachments/assets/a557058f-9078-4a1b-a2ff-db57656e3721" />
<img width="1097" height="448" alt="Screenshot 2026-05-09 173145" src="https://github.com/user-attachments/assets/fc50f24e-8442-445c-bd47-99f9b581278f" />
<img width="912" height="443" alt="Screenshot 2026-04-28 165418" src="https://github.com/user-attachments/assets/26ab5d6b-549a-4318-a794-d7fdcca3e83e" />

## 5. Quick Bypass Command

bash
objection -g com.example.app explore -s "android sslpinning disable"
```
<img width="1036" height="558" alt="Screenshot 2026-05-09 173113" src="https://github.com/user-attachments/assets/7db60fe0-f8c2-4ae1-9bba-4fe78652ae70" />

---






