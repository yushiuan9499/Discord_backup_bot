# Discord_backup_bot

> 自動或手動備份你的 Discord 伺服器資料，支援多種格式輸出與定期排程。適用於遷移、歸檔或數據分析需求。  

## 功能

- `!backup`：手動備份當前伺服器
- 定期排程備份（可修改排程時間）
- 匯出：
  - 所有文字頻道訊息（含圖片）  
  - 頻道分類、頻道、角色
  - 所有伺服器成員 ID、暱稱、狀態
- 支援儲存格式：
  - `.json`：完整資料結構
  - `.txt`：純文字輸出
  - `.html`：簡易視覺化版本

## 專案結構
```
discord_backup_bot/  
├── bot.py # 主程式，啟動 Bot 並處理指令  
├── config.json # 儲存 Token、伺服器 ID、輸出格式等  
├── backup_manager.py # 控管備份流程的模組  
├── utils/ # 功能模組化工具  
│   ├── message_exporter.py # 匯出訊息（JSON/HTML/TXT）  
│   ├── attachment_downloader.py # 下載附件  
│   ├── structure_exporter.py # 匯出頻道、角色、分類等設定
│   ├── scheduler.py # 可獨立執行的排程備份腳本  
│   └── formatter.py # 控管輸出格式  
├── backups/ # 備份資料儲存資料夾  
│   └── ...  
└── requirements.txt # 套件清單  
```
## 安裝與執行

### 1. 安裝 Python 套件

建議使用 Python 3.9+。

```bash
pip install -r requirement.txt
```

```requirement.txt
discord.py
schedule
```

### 2. 設定 config

修改 config.json 並填入你的資訊：
```config.json
{
  "bot_token": "你的 token 放這裡",
  "guild_id": "複製你的伺服器 id",
  "output_format": ["json", "html", "txt"], //設定下載所包含的格式
  "backup_folder": "./backups" //儲存備份資料的資料夾
  "SCHEDULE": "開始自動備份的時間"  // 可選：daily, hourly, weekly；false時關閉
  "download_attachments": 是否下載附件 // true時開啟
}
```

### 3. 啟用 Intent（重要）

到 Discord Developer Portal > 機器人設定 > Privileged Gateway Intents 中：

• ✅ 開啟 Server Members Intent  
• ✅ 開啟 Message Content Intent  

## 使用方式

### 手動備份

在 Discord 中輸入指令：

```
!backup
```

機器人會開始備份當前伺服器，並儲存到 backups/ 資料夾。

### 自動排程備份

機器人啟動後會自動依照 config.json 設定的時間執行備份。  

```config.json  
"SCHEDULE": false                   // 關閉排程
"SCHEDULE": "hourly"               // 每小時整點
"SCHEDULE": "daily@03:00"          // 每日凌晨 3 點
"SCHEDULE": "weekly@monday@02:00"  // 每週一凌晨 2 點
```

備份結果說明
```
backups/  
└── MyServer_20250424_120000/  
    ├── structure.json    # 頻道分類、文字/語音頻道、角色  
    ├── members.json      # 所有使用者 ID、暱稱、狀態  
    └── channels/  
        ├── general.json  
        ├── general.html  
        └── general.txt  
```
## 後續計畫（也許大概有機會有時間的話會新增）
（歡迎貢獻）

•壓縮備份資料 .zip  
•將備份同步到 Google Drive （API巨難搞）  
•定時備份開始時加入 Discord/Email 通知  
•匯出 Webhook 設定  
