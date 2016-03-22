# 建立 SQL 資料庫

## 需求

想要使用一個 SQL 資料庫而不想花太多時間管理 SQL 伺服器及系統。

## 事前準備

  * 擁有 Microsoft Azure 訂閱帳號。

## 解決方法

在左側的面板中找到 **新增** 或 **+** 的按鈕，按下新增按鈕後選擇 _資料 + 儲存體_ > _SQL Database_ 準備來建立一個網站空間來放要架設的網站。

![建立 SQL 資料庫](https://skgitbook.blob.core.windows.net/azurerecipestw/ch05/create_sql_database.png)
_**圖 1**_. 建立 SQL 資料庫

接著就是設定建立一些基本 SQL 資料庫的連線設定：

![設定 SQL 資料庫](https://skgitbook.blob.core.windows.net/azurerecipestw/ch05/config_sql_database.png)
_**圖 2**_. 設定 SQL 資料庫的基本屬性


根據各個欄位說明如下：

  * **名稱**：這裡只要設定一個可以識別的名字，之後連接資料庫時都會使用到這個名稱，而名稱只需要是同一個 SQL 伺服器上唯一即可。
  
  * **伺服器**：雖然 SQL 資料庫服務不需要特別管理伺服器，但實際上還是會存在一個 SQL 伺服器來運作 SQL 資料庫，由於一個 SQL 伺服器可以同時運作多個 SQL 資料庫，所以可以選擇要建立新的 SQL 伺服器或是與其它的 SQL 資料庫共用伺服器。伺服器會影響著運算效能以及登入驗證——一個 SQL 伺服器有一個固定的效能指標（目前為 2,000 DTU），在同一個伺服器上的 SQL 資料庫各自依價格方案去分用這些效能指標（例如基本方案用 5 ）是用同一組帳號密碼驗證身份，當然，資料庫所在的資料中心位置當然是以 SQL 伺服器為準囉。
  
  ![建立 SQL 伺服器](https://skgitbook.blob.core.windows.net/azurerecipestw/ch05/create_sql_server.png)
  _**圖 3**_. 建立新的 SQL 伺服器

  如果要建立一個全新的 SQL 伺服器，除了設定伺服器名稱以便網路連線之外，主要是設定身份驗證的帳號密碼，以及想要部署的資料中心位置。
  
  * **選取來源**：這個部份是告訴 Azure 在建立 SQL 資料庫時需不需要預建一些資料表（Table），如果你需要的是一個乾淨的 SQL 資料庫，選擇 _空白資料庫_ 即可。
  
  * **定價層**：這裡是設定 SQL 資料庫一開始所使用的方案，詳細的價格、儲存空間、功能與效能等比較資訊，可以參考官網上的[這個頁面](https://azure.microsoft.com/zh-tw/pricing/details/sql-database/)，而設定時也直接有功能與價格比較的概觀。當然，SQL 資料庫的價格方案是可以之後彈性調整的。
  
  ![SQL 資料庫的定價方案](https://skgitbook.blob.core.windows.net/azurerecipestw/ch05/set_sql_database_plan.png)
  _**圖 4**_. SQL 資料庫的定價方案，包含效能、功能差異以及預估金額

  * **選擇性組態**：這個部份目前主要在設定 SQL 資料庫欄位比較的定序規則，如果您沒有特別的需求、或是不瞭解定序的意義，可以先保留預設值即可。

  * **資源群組**：你可以將這個 SQL 資料庫與其相關的 Azure 服務放在同一個資源群組方便管理（或是其它各種更有彈性的群組方式也可以），不過這個部份也是跟著 SQL 伺服器而定的。

  * **訂用帳戶**：選擇使用哪一個 Azure 訂閱帳戶來放這個 SQL 資料庫服務。

以上的欄位都設定完成後，按下下方的 **建立** 按鈕開始建立 SQL 資料庫服務。大約數分鐘後就會完成建立，就可以開始管理及使用這個 SQL 資料庫了。

![SQL 資料庫已上線可以直接使用](https://skgitbook.blob.core.windows.net/azurerecipestw/ch05/sql_database_blade.png)
_**圖 5**_. SQL 資料庫的管理面板


