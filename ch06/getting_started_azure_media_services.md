# 建立媒體服務管理與播放影音檔案

## 需求

開始使用 Azure 媒體服務（Media Services）來管理多媒體檔案做播放或串流的工作。

## 事前準備

  * 擁有 Microsoft Azure 訂閱帳號。

## 解決方法

使用您擁有 Microsoft Azure 訂閱帳號的 Microsoft 帳號登入 [Microsoft Azure 的管理後台](https://portal.azure.com/)，在左側的面板中找到 **新增** 的按鈕，按下新增按鈕後選擇 _媒體 + CDN_ > _Media Services_ 準備來建立一個媒體服務的帳號來放要處理的影音檔案。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/create_media_service_step1.png)
_**圖 1**_. 建立媒體服務帳號

建立媒體服務的帳號需要填寫幾個欄位，其中包含：

  * _帳號名稱（Account Name)_: 顧名思義，就是用來代表這個媒體服務的帳號名稱，這必須是一個唯一的名稱，因為它會關聯相關的 API 操作以及影音串流時預設的 URL 名稱。
  * _訂閱（Subscription）_: 就是這個服務要放在哪個 Azure 訂閱帳戶下。
  * _資源群組（Resource Group）_: 選擇要新建立一個資源群組還是併到既有的資源群組中集中管理。
  * _地區（Location）_: 設定媒體服務要在哪個資料中心部署，因為媒體服務中有轉檔或串流的虛擬機器資源，所以一樣要設定資料中心的地區。
  * _儲存體帳號（Storage Account）_: 媒體服務中的所有影音檔案都會放在 Azure 儲存體中，所以建立媒體服務的帳號時，都要選擇一個對應的 Azure 儲存體帳號，當然，這個儲存體帳號的位置應該要與媒體服務選擇同一處。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/create_media_service_step2.png)
_**圖 2**_. 建立媒體服務要填寫的欄位

設定完成後，等待 Azure 把資源部署完畢，就可以開始使用這個媒體服務帳號來處理影音檔案了。

接下來我們可以先上傳一個影音檔案來瞭解 Azure 媒體服務最基本的操作流程，影音檔案在 Azure 媒體服務中都視為 **Assets**，所以第一步得先把影音檔案上傳到媒體服務中的 Assets，點擊管理面版中選擇 _Settings_ > _Assets_ -> _Upload_ 來上傳影音檔案。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/open_assets_blade.png)
_**圖 3**_. 開啟 Assets 面板

接下來選擇影音檔案就可以開始上傳，可以參考[這裡](http://go.microsoft.com/fwlink/?LinkId=746930)來確認支援的影音檔案格式。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/uploading_media.png)
_**圖 4**_. 上傳影音檔案中

上傳完畢便能在 _Assets_ 的清單中看到已經完成上傳的檔案，點擊它就可以看到所有在媒體服務中能處理這個影音檔案的功能。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/assets_management.png)
_**圖 5**_. 影音檔案管理

如果上傳的檔案本身就是可以直接播放的檔案，那可以在 Asset 管理的面板中按下 **Publish** 的按鈕，然後選擇要開放的時間期限，幾秒鐘後就會給你一個網址用來播放這個影音檔案。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/publish_asset.png)
_**圖 6**_. 發佈影音檔案

選好時限後按下 **Add** 就有一個 URL 能播放這個影音檔案了。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/published_assets.png)
_**圖 7**_. 發佈影音檔案後就有了可以播放的 URL。

如果想要進行檔案重新編碼，在開始轉檔前，首先要去設定**編碼單位（Encoding unit）**，因為媒體服務帳號剛建立時是沒有任何的編碼單位，因為這個會有費用產生，所以有需要編碼時要設定，若無轉檔編碼的需求，就可以把單位再設回 **0** 避免產生額外的費用。

在媒體服務的管理面板中選擇 _Settings_ > _Media reserved units_ 就可以設定編碼單位，這部份有兩個可以設定的欄位：

  * _媒體保留單位（Media Reserved Units）_: 媒體服務在做任何處理都需要保留單位，而這個數量代表**能同時處理多少工作**，所以如果同時間只會轉一個檔案，那只要設 **1** 就可以了，因為數量再大也不會增加效能。
  * _處理單位的速度（Speed of reserved processing units）_: 這個就是影響編碼轉檔速度的關鍵了，可以根據需求及成本考量選擇合適的處理單位等級，在選擇等級的面板中都可以看到不同等級的能力差異。

設定完成後別忘了按下 **Save** 按鈕儲存設定。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/setting_encoding_unit.png)
_**圖 8**_. 設定保留的處理單位

有了媒體保留處理單位後，就可以開始進行轉檔的工作，打開欲轉檔的 Asset 然後按下 **Encode** 的功能按鈕，這時會開始選擇要轉檔的設定，包括：

  * _媒體編碼器名稱（Media encoder name）_: 這裡可以選擇**標準（standard）**或是**進階（Premium）**的編碼器等級。
  * _編碼設定（Encoding preset）_: 這裡選擇要轉檔的設定組合，像是用什麼編碼格式、解析度、位元率等等，目前這篇文章先介紹單一位元率（single bitrate）的處理，多重位元率的編碼還需要做些別的設定，我們在其它篇食譜中另行介紹。
  * _工作名稱（Job name）_: 這裡可以設定轉檔的工作名稱，方便追蹤轉檔的進度與狀況。
  * _輸出媒體名稱（Output media asset name）_: 轉檔後的影音檔案也會是一個新的 asset，這裡設定它的名稱。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch06/encoding_an_asset.png)
_**圖 9**_. 設定轉檔工作

轉檔的過程中可以到 _Settings_ > _Jobs_ 面板中看到進度與狀況，順利編碼完成後，你就可以在 Assets 清單中看到轉檔後的影音檔案，這時便可以用相同的步驟將它發佈（**Locator** 選 _Progressive_），取得播放的 URL。

  > 別忘了，如果編碼轉檔完後不再需要編碼轉檔，要記得去將保留處理單位設回 **0** 以免產生額外的費用。