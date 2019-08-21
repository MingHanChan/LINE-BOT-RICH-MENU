# LINE-BOT with Google Apps Scripts
>最近發現LINE BOT有很多可以玩的地方，姑且來玩玩看，並做一個紀錄，讓自己以後忘記可以來翻

## About LINE BOT
想要有一個LINE BOT的前提是你必須要先有一個自己的LINE帳號，這部分不難，可以直接用自己現有的帳號或在額外申請一個。
目前一個個人LINE帳號可以建立多個Provider，每一個Provider下又可以建立多個LINE Bot Channel。
- LINE個人帳號
	- Provider
		- LINE Bot
		- LINE Bot
		- LINE Bot
	- Provider
		- LINE Bot
		- LINE Bot
		- LINE Bot
	- Provider 
	...

### LINE Developers
 1. login the [LINE Developers](https://developers.line.biz/en/)
![](https://github.com/MingHanChan/LINE-BOT-RICH-MENU/blob/master/img/Line%20Developers.png)
 2. Create Provider List
![](https://github.com/MingHanChan/LINE-BOT-RICH-MENU/blob/master/img/create%20new%20channel.png)
 3. Input the Provider name and  click **Confirm** and **Create**
![](https://github.com/MingHanChan/LINE-BOT-RICH-MENU/blob/master/img/confirm.png)
4. Go into the Provider, click the **Create new channel** and choose the 'Messaging API'
![](https://github.com/MingHanChan/LINE-BOT-RICH-MENU/blob/master/img/create%20new%20channel.png)
5. Let's go started ! 

### Channel Access Token and UserId
Channel Access Token基本上是可以操作你的LINE Bot的一組金鑰，有了這組Token就可以透過你的Bot發訊息給其他人。
到你的管理後台來看，第一次新建立的Bot Channel在Token那邊會是空白的，按下 **Issue** 按鈕來產生一個long-lived Channel Access Token，每按一次就會產生一組新的Token，而前一組就會失效。
![](https://github.com/MingHanChan/LINE-BOT-RICH-MENU/blob/master/img/Issue%20Channel%20Access%20Token.png)

而UserId是Bot發送訊息的對象，但UserId跟一般用戶的LINE Id是不一樣的，所以要發送之前必須知道發送對象的UserId。

### LINE官方帳號類型
根據[LINE官方最新消息](http://at-blog.line.me/tw/archives/LINEOA2.0.html)，自2019年4月18日起將推動LINE官方帳號2.0的計畫，將LINE@生活圈、LINE官方帳號、LINE Business Connect、LINE Customer Connect等產品進行服務及功能整合，並將名稱取為「LINE官方帳號」。
而整合後的計畫將沒有好友人數的上限，相關一些功能使用也開放給免費用戶使用了，但相對的是根據訊息的發送量來計算相關的費用，詳情上官方網頁查詢。
一般來說，灰色盾牌的一般帳號是任何人都可以申請不須審核的，但是無法被搜尋的；藍色盾牌的認證帳號就是要經過LINE的身分審核；最後綠色盾牌的企業帳號是要透過LINE的主動邀請才能成為這樣的資格，但帳號的類型跟你所使用的價格方案是沒有關係的，只是有認證過後的官方帳號比較可以取信於人。

### WebHook
WebHook是處理Chat bot的核心，因為大部分的Bot都是以回復用戶的訊息為主，要回復用戶什麼都是由後面的WebAPI回應來的。
基本上，LINE回復訊息分為Push API 跟 Reply API，原則上現在LINE 官方帳號2.0計畫下使用Reply API是不需要收費的，而使用Push API屬於主動發訊息所以會被算在免費訊息則數的額度上。

透過下面官方說明文件的圖可以大概了解WebHook在整個LINT Bot的結構上是怎麼運作的。
![image](https://developers.line.biz/media/messaging-api/overview/messaging-api-architecture-cffb1d9b.png)

當LINE用戶與Bot互動時，LINE平台會將用戶所傳送的訊息轉成JSON格式以以HTTPS POST的方式送給你後面的WebHook Server，接著你的後台根據所收到的WebHook event來做出反應，並且透過LINE平台回應給使用者
