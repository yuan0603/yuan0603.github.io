### 通過命令提示字元刪除 Windows 11 更新檔案

```bat
wmic qfe list brief /format:table
wusa /uninstall /kb:HotFixID
```
