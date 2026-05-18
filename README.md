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

```bash
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

---
<img width="912" height="443" alt="Screenshot 2026-04-28 165418" src="https://github.com/user-attachments/assets/26ab5d6b-549a-4318-a794-d7fdcca3e83e" />


---






