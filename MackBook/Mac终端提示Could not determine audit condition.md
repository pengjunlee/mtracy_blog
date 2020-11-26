**原因**：自己修改了系统变量

**结果**：导致终端显示进程已完成

**错误信息**：<font color=red>login： Could not determine audit condition  [Process completed]</font>

**解决方案**：打开**Finder**（`shift+Command+G`）前往文件夹`usr/bin/login`文件夹，删除login文件。