powershell
# 自动化系统映像备份脚本（适用于 Windows 11）
# 请以管理员身份运行 PowerShell

# 设置备份目标路径和源分区
$BackupTarget = "E:\"           # 修改为你的备份磁盘路径
$VolumeToBackup = "C:"          # 系统分区
$CurrentDate = Get-Date -Format "yyyy-MM-dd_HH-mm"
$LogFile = "$BackupTarget\BackupLog-$CurrentDate.txt"

# 检查备份目标是否存在
if (-not (Test-Path -Path $BackupTarget)) {
    Write-Host "❌ 备份目标路径不存在：$BackupTarget"
    Exit
} else {
    Write-Host "✅ 备份将保存到：$BackupTarget"
}

# 构建备份命令
$BackupCommand = "wbadmin start backup -backupTarget:$BackupTarget -include:$VolumeToBackup -allCritical -quiet"

# 执行备份并记录日志
Write-Host "🚀 正在执行备份..."
Invoke-Expression $BackupCommand | Out-File -FilePath $LogFile -Encoding UTF8

Write-Host "✅ 备份完成，日志已保存到：$LogFile"
📅 自动化建议：任务计划集成
你可以将此脚本保存为 .ps1 文件，并通过任务计划程序定期运行：

打开“任务计划程序”

创建新任务 > 触发器：每天/每周定时运行

操作：启动程序，填写：

程序：powershell.exe

参数：-ExecutionPolicy Bypass -File "C:\路径\你的脚本.ps1"

勾选“使用最高权限运行”
