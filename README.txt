IoT Dashboard - Node-RED + MQTT + SQLite (Node 24, tanpa node-red-node-sqlite)

ISI
- flows_iot_node24.json : flow Node-RED (REST API + WebSocket + SQLite + threshold + API key)
- dashboard.html        : dashboard (Chart.js, WebSocket, kontrol lampu, set threshold)
- simulasi_esp32_1.py   : simulator ESP32 (tidak berubah)

SETUP SEKALI SAJA
1. Edit C:\Users\LENOVO\.node-red\settings.js, pada functionGlobalContext tambahkan:
       sqlite: require('node:sqlite'),
   (JANGAN install node-red-node-sqlite, tidak perlu)
2. Jalankan Node-RED:   cd C:\Users\LENOVO\.node-red  lalu  node-red
3. Buka http://localhost:1880 -> Menu -> Import -> flows_iot_node24.json -> Deploy
   Node "Init DB (node:sqlite)" harus berstatus hijau "DB siap".
   Database dibuat di C:\Users\LENOVO\.node-red\iot.db

MENJALANKAN
1. Pastikan Mosquitto aktif
2. python simulasi_esp32_1.py
3. Double-click dashboard.html

API KEY : iot-secret-123 (ubah di flow dan di dashboard.html, konstanta API_KEY)

TES API (PowerShell/CMD)
  curl http://localhost:1880/api/suhu                                   -> 401
  curl -H "X-API-Key: iot-secret-123" http://localhost:1880/api/suhu    -> sukses
  curl -X PUT -H "Content-Type: application/json" -H "X-API-Key: iot-secret-123" -d "{\"max\":28}" http://localhost:1880/api/threshold
