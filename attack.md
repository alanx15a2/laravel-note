# Attack

## XSS

* 介紹
  * XSS 是利用網頁通常會自動執行 JS 的特性來達成攻擊的目的
  * 例如在填寫個人資料時將姓名填寫為 <script>alert(123);</script>; 所有閱讀到這筆資料的人都會跳出警告
  * 分為儲存型 & 反射型，儲存型會存到 DB 中，每個人都有機會閱讀到有問題的頁面；反射型則是透過簡訊 Email 等方式，特定人點開後才有機會中獎，傷害較小。
* 預防
  * 輸出時需要特別注意，將所有文字都轉換為純文字再進行輸出。

## CSRF(XSRF)

* 介紹
  * 是利用剛登入網站後，Cookie 還未過期的期間，點擊駭客希望的網址來達到攻擊的目的。
  * 例如尚未登出網路銀行的情況下，點擊了轉帳給駭客的網址。
* 預防
  * 檢查 Referer，直接點擊網址時 referer 會是空的。
  * 加圖形驗證碼。
  * CSRF Token
  * Double submit cookies

## 認證方式

[source](https://stackoverflow.com/questions/17000835/token-authentication-vs-cookies)

### Token

* 優點
  * 對 CSRF 免疫，因為每個請求都需要手動加上 token。
  * 可以被多個域名認證。
* 缺點
  * 要仔細評估 token 儲存位置。
    1. local storage : 關閉網頁後 token 依然存在
    2. session storage : 關閉網頁後 token 消失，但跨分頁的話會是另一個 session (此處的 session 為會話階段，非伺服器上的 session)
    3. cookie : 關閉網頁後 token 消失，跨分頁也可以存取 token，但由於 cookie 每個請求都會自動送出，因此若未設定 http only 則可能被中間人攻擊。(由於僅作為儲存 token，而非使用 cookie 直接驗證，因此依然對 CSRF 免疫)
  * 較容易被 XSS 攻擊取得認證資訊。

### Cookie

* 優點
  * 由於 XSS 攻擊通常目標是取得 token，cookie 可以設定為 httponly 來讓此攻擊難以實現。
  * 簡單易用，登入後設定完 cookie，後續任何請求不需要手動附加額外資訊。
* 缺點
  * 綁定單網域，如今微服當道，若有採用微服務則可能需要做麻煩的伺服器轉發。
  * 易被 CSRF 攻擊，需要實作額外機制來避免。
  * 就算請求不需要認證，cookie 依然會送出。

