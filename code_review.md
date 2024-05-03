# Code Review 清單

## 通用

- Code 是可用的
- Code 是容易理解的
- 符合 [PSR-12](https://www.php-fig.org/psr/psr-12/)
- 變數名稱簡單、易理解、有意義、不自創單字、縮寫
- 不使用[魔術數字](https://wiki.c2.com/?MagicNumber)
- 不在任何未來可能修改的地方 hardcode 常數
- 所有變數作用域最小化 (private > protected > public)
- 刪除無效 code 取代註解 code
- 不應有無法執行的方法
- 現有 library 可達到的目的不重造輪子
- 避免 Negative variable Ex: `$notValid`、`$notActive`
- 使用適當的資料結構
- 迴圈有適當的結束條件
- 迴圈內的 code 應最少化
- code 有辦法被撰寫單元測試
- 應採用 early return
- 有考量到效能
- 使用 type hint

## 架構

- 符合 DRY 原則
- 使用適當的 Design Pattern
- 盡量遵守 SOLID 原則
- 遵守[最少知識原則](https://zh.wikipedia.org/zh-tw/%E5%BE%97%E5%A2%A8%E5%BF%92%E8%80%B3%E5%AE%9A%E5%BE%8B)
- Controller 不應有商業邏輯，而以呼叫、組合各種 service 為主
- 各類單據建立、刪除、修改應呼叫 service 來作業
- 如有額外作業，應藉由監聽各個事件，並由各個監聽器來執行 Ex: 發布小鈴鐺監聽器監聽 PO 建立完成事件

## API

- 任何 API 都需要確認使用者權限
- 任何 API 都需要驗證參數必填否、型態
- 任何 API 都應該被記載在文件中
- 任何 API 都應該回復正確的格式 (正確的 http status、success、code、message、data)
- error code 為唯一並對應到特定錯誤訊息

## Log

- 任何 Log 都透過 Log facade 記錄
- 不呼叫 print_r, var_dump 等等類似的方法

## 安全性

- 所有的輸入都有被檢查 (型態、長度、格式、範圍)
- 敏感資料不應被顯示、log、上傳、hardcode
- 應避免 XSS、CSRF、SQL Inject
