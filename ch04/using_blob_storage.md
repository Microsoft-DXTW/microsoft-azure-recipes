# 用 Blob 儲存體服務儲存檔案及提供檔案服務

## 需求

將檔案儲存在 Blob 儲存體中，並且經由公開或驗證後取得到的 URL 來存取檔案，以用在分散網站靜態檔案存取以及其它永久儲存或提供其它 Azure 服務使用。
  
## 事前準備

  * 擁有 Microsoft Azure 訂閱服務

## 解決方法

### 建立儲存體帳號

進入 [Microsoft Azure 管理後台](https://portal.azure.com/)，在左側面板找到 **新增** 的按鈕，按下後選擇 _儲存體_

![建立儲存體帳號](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/create_new_storage.png)

_**圖 1**_. 建立儲存體帳號

建立儲存體服務的步驟很簡單，一樣是要設定_名稱_、_資源群組_、_資料中心位置_這樣的欄位，不過因為儲存體提供的是永久性（persistant）的資料儲存，最基本就會在同一個資料中心裡複寫三份備援（不會算 3 倍價錢），但若您願意用費用換來更多的備援，還可以選擇異地備援等方案，這可以在 **定價層** 的欄位中選擇。

![選擇不同等級的備援](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/storage_price_plan.png)

_**圖 2**_. 選擇不同的定價方案

不同的價格方案最主要的差異就是備援的方式，除了高階（premium）的儲存體方案是 10 倍的效能之外（單一檔案 _5,000_ IOPS）。而顯示的價格是 _**100GB 一個月**_的預估價格，你可以根據實際用量按比例去計算出預估花費。

設定完成後就可以立即建立，如果沒有其它的錯誤就會顯示儲存體的管理面板，比較常用的功能會像是找到存取的金鑰，因為預設狀況下要透過帳戶名稱 + 金鑰(主要或次要其中一把)來驗證存取。而金鑰資訊要妥善保存使用，如果不慎外流的話，也可以利用上面的 **重新產生** 按鈕產生新的金鑰並且註銷原本的金鑰。

![取得金鑰內容](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/get_storage_key.png)

_**圖 3**_. 取得金鑰內容

另外，在建立儲存體帳號時，也會根據選用的名稱配發一個 URL （_http://名稱.blob.core.windows.net/_）使用，如果想要換成自己的網域，也可以在自訂網域中設定。

![自訂網域](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/setting_custom_domain.png)
_**圖 4**_. 設定自訂網域

### 容器以及上傳檔案

要上傳檔案到 Azure Blob 儲存體有很多工具可以使用，而微軟官方也開發了一套跨平台（Windows, Linux 以及 Mac OSX）上都能使用的 [Microsoft Azure Storage Explorer](https://storageexplorer.com/) 來處理檔案的操作。

登入有 Azure 訂閱的 Microsoft 帳號後（你可以選擇要處理的 Azure 訂閱帳戶），你可以在 Microsoft Azure Storage Explorer 中看到可以管理的儲存體帳號。

![在 Storage Elorer 中列出所有儲存體帳號](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/mase_accounts.png)
_**圖 5**_. 在 Storage Explorer 中列出所有儲存體帳號

而在儲存體帳戶中要放檔案，必須得放在 Blob 容器（Blob Containers）中，Blob 容器是置放檔案的基本單位，你可以設定它為**公開（public）**或是**私人（private）**，放在公開容器中的檔案都是可以直接透過 URL 位置來讀取，而放在私人容器中的檔案，除了儲存體帳號的擁有者能存取之外，無法直接透過 URL 來讀取，必須加上共享簽章的參數才能有限制的存取。

這裡我們先示範放在公開容器檔案的讀取，關於私人容器內檔案的存取我們會在另一篇文章中介紹。在 Storage Explorer 中，你可以在儲存體帳戶下的 _Blob Containers_ 上按右鍵選擇 **Create Blob Container** 來建立一個 Blob 容器。

![建立 Blob 容器](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/mase_create_blob_container.png)
_**圖 6**_. 在 Storage Explorer 建立 Blob 容器

接著會提示你輸入這個容器的名稱（需小寫英文字母以及數字組成），預設的容器是被設定成**私人**的狀態，這個可以在下方的屬性面板中看到是否能夠 _Public Read Access_。

![](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/mase_blob_container_permission.png)
_**圖 7**_. 檢視建立的 Blob 容器是否公開

如果要將這個 Blob 容器更改為可公開讀取，只要在這個容器上按右鍵選擇 _Set Public Access Level..._

![設定 Blob 容器公開讀取權限](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/mase_setting_public_access_level.png)
_**圖 8**_. 設定 Blob 容器公開讀取權限

而在 Public Access Level 中有三個 level：

  * **No public access**: 就是私人存取的狀態，也是預設的權限。
  * **Public read access for container and blobs**: 設成這個權限，不僅可能透過 URL 讀取容器中的檔案，也可以讀取容器的內容，取得容器內的檔案列表。
  * **Public read access for blobs only**: 只能透過 URL 讀取容器內的檔案，但是無法從容器上列出所有檔案。

![容器的公開讀取權限](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/mase_public_access_level.png)
_**圖 9**_. 容器的公開讀取權限

完成建立 Blob 容器後，在容器上按右鍵選擇 _View Blob Container_ 你便能在容器中上傳檔案，上傳檔案完成後，就可以在檔案上按右鍵取得可公開讀取的 URL。

> 若是放在私人容器內的檔案，除了 URL 之外，還需要先 _Get Shared Access Signature..._ 來取得共享簽章作為參數才能讀取檔案。

![取得 Blob 容器內檔案的 URL](https://skgitbook.blob.core.windows.net/azurerecipestw/ch04/mase_get_blob_url.png)
_**圖 10**_. 取得 Blob 容器內檔案的 URL