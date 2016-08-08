# debug

## 用模拟器调试

* windows

1. 查看模拟器 `PID` 假如为5940
2. `netstat -ano| findstr 5940` 就能找到连接协议的端口 假如为 58001
3. `adb connect localhost:58001` 假如模拟器在本机上
4. `adb devices` 就能看到连接状态