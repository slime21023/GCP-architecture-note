# Select and implement a storage strategy

為促進資料交換並以安全方式備份和儲存資料，組職需要根據工作負載([即每秒輸入/輸出操作次數(IOPS)](https://wikipedia.org/wiki/IOPS))、延遲時間、檢索頻率、位置、容量和格式(塊、文件和對象)選擇存儲方案

* Google Storage 提供安全可靠的對象儲存服務
  * 內置冗餘選項: 可保護你的資料免受設備故障的影響，並確保在資料中心維護期間的資料可用性
  * 數據轉移選項
    * [Storage Transfer Service](https://cloud.google.com/storage/transfer?hl=zh-cn)
    * [Transfer Appliance](https://cloud.google.com/transfer-appliance?hl=zh-cn)
    * [BigQuery Data Transfer Service](https://cloud.google.com/bigquery/docs/dts-introduction?hl=zh-cn) 
    * [遷移到Google Cloud: 轉移大型資料集](https://cloud.google.com/architecture/migration-to-google-cloud-transferring-your-large-datasets?hl=zh-cn)
  * [存儲類別](https://cloud.google.com/storage/docs/storage-classes?hl=zh-cn)，可支持你的工作負載
  * 針對所有可讓 Google 驗證讀取和寫入的 Cloud Storage 操作計算得出的校驗和

在 Google Cloud中， IOPS 會根據你預配的儲存空間進行擴縮
* Persistent Disk等儲存類型需要手動進行複製和備份，它們是區域級或可用區級的儲存類型
* [對象存儲](https://cloud.google.com/learn/what-is-object-storage?hl=zh-cn)具備高可用性，並且會自動跨單個區域或多個區域複製資料

### 儲存類型

* 評估高性能儲存需求的選項
  * 評估永久性磁盤或本地固態硬碟(SSD)是否適用於需要高性能存儲的計算應用
  * Cloud Storage是否啟用了版本控制的不可變對象儲存
  * 將Cloud Storage 與 Cloud CDN 搭配使用有助於針對費用進行優化，尤其是對於經常訪問的靜態對象
  * Filestore 支持需要高性能共享空間的多寫入應用
  * Filestore 支持需要通過[網路文件系統](https://wikipedia.org/wiki/Network_File_System)(NFS)裝載執行類似 POSIX 的文件操作之應用
  * Cloud Storage 可為創建資料湖和滿足歸檔要求等使用場景提供支持
    * 由於存在訪問費用和檢索費用，請根據選擇 [Cloud Storage 類別](https://cloud.google.com/storage/docs/storage-classes?hl=zh-cn)的方式來制定權衡決策
    * [為雲工作負載設計最佳儲存策略](https://cloud.google.com/architecture/storage-advisor?hl=zh-cn)
  * 默認情況下，所有儲存選項都使用 Google 管理的密鑰進行靜態加密和傳輸中加密
  * 對於 Persistent Disk 和 Cloud Storage 等儲存類型，可以提供自己的密鑰或通過 Cloud Key Management Service (Cloud KMS) 進行管理
    * 請先制定處理此類密鑰的策略，再將其運行於生產數據

* 選擇 Google Cloud 服務以支持儲存設計
  * Cloud Storage
    * 可在全球範圍內隨時存儲和檢索任意數量的資料
    * 可在多個場景中使用 Cloud Storage，包括傳送網站內容、儲存資料以用於歸檔和災難恢復，或者通過直接下載向用戶分發大型資料對象
    * [Cloud Storage 最佳實踐](https://cloud.google.com/storage/docs/best-practices?hl=zh-cn)
    * [存儲桶位置](https://cloud.google.com/storage/docs/locations?hl=zh-cn)
    * [存儲類別](https://cloud.google.com/storage/docs/storage-classes?hl=zh-cn)
    * [Cloud Storage FUSE](https://cloud.google.com/storage/docs/gcs-fuse?hl=zh-cn)

  * Persistent Disk
    * 用於 Google Cloud 的高性能塊存儲
    * 提供 SSD 和 HDD 儲存設備
    * 可將其掛載到 Compute Engine 或 Google Kubernetes Engine (GKE) 中運行的實例
    * [區域性磁碟](https://cloud.google.com/compute/docs/disks/regional-persistent-disk?hl=zh-cn)可在同一區域中的兩個可用區之間提供持久耐用的儲存和資料複製功能
    * 若需較高的IOPS和延遲時間，Google Cloud 提供了 Filestore
    * [本地SSD](https://cloud.google.com/compute/docs/disks/local-ssd?hl=zh-cn)以物理方式掛載到託管虛擬機實例的服務器，可作為臨時儲存空間使用

  * Filestore
    * 一項代管式文件存儲服務，適合需要使用檔案系統接口和共享檔案系統來儲存資料的應用
    * 為 Compute Engine 和 Kubernetes Engine 實例建立代管式的NAS儲存空間

  * Cloud Storage for Firebase
    * 專為需要儲存和提供用戶生成的內容(如照片或影片)的應用開發者打造的
    * 所有檔案存儲在 Cloud Storage 存儲桶中，因此可從 Firebase 和 Google Cloud 進行訪問

### 選擇存儲策略

使用場景:
* 希望以最低費用大規模存儲資料，而非考慮訪問性能: Cloud Storage
* 運行需要短期儲存的計算應用: Persistent Disk 或 本地SSD
* 運行需要共享空間的讀寫權限的高性能工作負載: Filestore
* 擁有高性能計算(HPC)或高吞吐量計算(HTC)使用場景: [使用集群在雲中進行大規模技術計算](https://cloud.google.com/architecture/using-clusters-for-large-scale-technical-computing?hl=zh-cn)

### 根據儲存訪問需求選擇活躍存儲

[存儲類別](https://cloud.google.com/storage/docs/storage-classes?hl=zh-cn)是每個對象使用的一段[元數據](https://cloud.google.com/storage/docs/metadata?hl=zh-cn)
* 按照高速率和高可用性標準傳送的資料: Standard Storage 
* 不經常訪問且接受略低可用性的資料: Nearline Storage、 Coldline Storage、 Archive Storage
  
### 評估Cloud Storage 的存儲位置和資料保護需求

* 對於位於某個區域的Cloud Storage存儲桶，其中所含的資料會自動在該區域內的各可用區中複製
* 跨可用區的資料複製功能可在區域內出現可用區性故障時保護資料
* Cloud Storage 提供跨區域冗餘的位置，意味著數據會複製到分布在多個地理位置不同的資料中心

### 使用Cloud CDN改進靜態對象傳送

* 若需優化檢索對象的費用並將訪問延遲時間縮至最短: 使用Cloud CDN
* Cloud CDN 使用 Cloud Load Balancing [外部應用負載均衡器](https://cloud.google.com/load-balancing/docs/https?hl=zh-cn)來提供路由、健康檢查和IP地址支持
* [使用雲存儲桶設置 Cloud CDN](https://cloud.google.com/cdn/docs/setting-up-cdn-with-bucket?hl=zh-cn#send-traffic)

### 儲存訪問模式和工作負載類型

* 使用 Persistent Disk 以支持高性能存儲訪問
* 在實現重試邏輯時使用指數退避算法
  * 處理 5xx、 408、 429錯誤
  * [請求率和訪問權限分配準則](https://cloud.google.com/storage/docs/request-rate?hl=zh-cn)

### 存儲管理
* 為每個存儲桶分配唯一名稱
  * [存儲桶命名準則](https://cloud.google.com/storage/docs/buckets?hl=zh-cn#naming)
  * [對象命名準則](https://cloud.google.com/storage/docs/objects?hl=zh-cn#naming)

* 不公開Cloud Storage 存儲桶
  * 除非存在與業務相關的理由，否則請確保Cloud Storage 存儲桶不能以匿名方式訪問或公開訪問
  * [訪問權限控制概覽](https://cloud.google.com/storage/docs/access-control?hl=zh-cn#public_access_prevention)

* 分配隨機對象名稱以均勻分配負載
  * 提高性能避免出現 [hotspotting](https://cloud.google.com/spanner/docs/schema-design?hl=zh-cn#primary-key-prevent-hotspots)問題
  * [借助命名慣例將負載均勻分配到多個鍵範圍中](https://cloud.google.com/storage/docs/request-rate?hl=zh-cn#naming-convention)

* [使用禁止公開訪問功能](https://cloud.google.com/storage/docs/using-public-access-prevention?hl=zh-cn)
  