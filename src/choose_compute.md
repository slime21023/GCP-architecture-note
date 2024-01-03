# Choose Compute

### 選擇計算平台

為工作負載選擇計算平台時，請考慮工作負載的技術要求、生命周期自動化過程、區域化和安全性
* 評估應用和整個系統的CPU使用性質，包括如何打包、部屬、分發和調用程式碼
* 無服務器: Cloud Run、 Cloud Functions
* Kubernetes: Google Kubernetes Engine
* 虛擬機: Compute Engine

### 設計工作負載

* 評估無服務器方案是否適用於簡單邏輯
  * 簡單邏輯是一種計算類型，不需要專業化硬體或CPU經過優化的機器等類型，在對 Google Kubernetes Engine(GKE) 或 Compute Engine進行投資前，請先評估[無服務器方案](https://cloud.google.com/architecture/framework/system-design/compute?hl=zh-cn#platform)是否適用於輕量級邏輯

* 將應用分離為無狀態應用
  * 盡可能將應用分離為無狀態應用，以便充分利用無服務器計算
  * 此方法可讓你使用託管式計算產品，根據需求擴縮應用，以及針對費用和性能進行優化
  * [在設計時確保可擴縮性和高可用性](https://cloud.google.com/architecture/framework/reliability/design-scale-high-availability?hl=zh-cn)

* 在分離架構時使用緩存
  * 若應用旨在成為有狀態應用，請使用緩存進行分離並確保工作負載可以擴鎖
  * [資料庫最佳實踐](https://cloud.google.com/architecture/framework/system-design/databases?hl=zh-cn#caching)

* 使用實時推動升級
  * 為方便Google維護升級，設置實例可用性政策以使用實時遷移
  * [設置虛擬機主機維護政策](https://cloud.google.com/compute/docs/instances/host-maintenance-options?hl=zh-cn)
  
### 擴縮工作負載

* 使用啟動和關停腳本
  * 對於有狀態應用，請盡可能使用[啟動](https://cloud.google.com/compute/docs/instances/startup-scripts?hl=zh-cn)和[關停](https://cloud.google.com/compute/docs/shutdownscript?hl=zh-cn)腳本安全啟動和停止應用狀態。

* 使用MIG支持虛擬機管理
  * 當使用 Compute Engine虛擬機時，[代管式實例組](https://cloud.google.com/compute/docs/instance-groups/creating-groups-of-managed-instances?hl=zh-cn)(MIG)支持自動修復、負載平衡、自動擴縮、自動更新和[有狀態工作負載](https://cloud.google.com/compute/docs/instance-groups/stateful-migs?hl=zh-cn)等功能
  * 可根據可用性目標創建可用區級或[區域級MIG](https://cloud.google.com/compute/docs/instance-groups/regional-migs?hl=zh-cn)
  * 可以對無狀態服務工作負載或批量工作負載以及需要保留每個虛擬機獨有狀態的應用使用MIG

* 使用Pod自動擴縮器擴縮GKE工作負載
  * 使用 [Pod 橫向自動擴縮器](https://cloud.google.com/kubernetes-engine/docs/concepts/horizontalpodautoscaler?hl=zh-cn)和 [Pod 縱向自動擴縮器](https://cloud.google.com/kubernetes-engine/docs/concepts/verticalpodautoscaler?hl=zh-cn)來擴縮工作負載，並使用[節點自動預配]功能來擴縮底層計算資源

* 分發應用流量
  * 若需在全球範圍內擴縮應用，請使用 Cloud Load Balancing 跨多個區域或可用區分發應用實例
  * 負載平衡器可優化從Google Cloud [邊緣網路](https://cloud.google.com/vpc/docs/edge-locations?hl=zh-cn)到最近可用區的數據包路由，從而提高流量傳送效率並將傳送費用降至最低
  * 若需對最終用戶延遲時間進行優化，可使用 Cloud CDN 來緩存靜態內容

* 實現計算創建和管理自動化
  * 通過自動執行計算創建和管理，最大限度地減少生產環境中發生的人為錯誤

### 管理維運

*  使用 Google 提供的公共映像
   *  Google Cloud 公共映像會定期更新
   *  可以使用特定配置[創建自己的映像](https://cloud.google.com/compute/docs/images/create-delete-deprecate-private-images?hl=zh-cn)
   
* 使用快照進行實例備份
  * 快照特別適合用於有狀態應用
  * 有狀態應用不夠靈活，在遭受突然關停時無法保留狀態或保存進度

* 使用機器映像可創建虛擬機實例
  * 快照只會補捉機器內部的數據映像，但機器映像除了捕獲數據之外，還會捕獲機器配置和設置
  * 使用[機器映像](https://cloud.google.com/compute/docs/machine-images/create-machine-images?hl=zh-cn)可以存儲創建虛擬機實例所需的一個或多個磁盤的所有配置、元數據、權限和資料
  * 使用機器映像可讓這些只知設置複製到新機器上，從而減少開銷: [何時使用機器映像](https://cloud.google.com/compute/docs/machine-images?hl=zh-cn#when-to-use)

### 容量、預留和隔離

* 使用承諾使用折扣減少費用
* 選擇機器類型以支持費用和性能
* 使用單租戶節點以滿足合規性需求
  * 單租戶節點是專門用於託管專案虛擬機實例的物理 Compute Engine 服務器
  * 將你的虛擬機與其他項目中的虛擬機進行物理隔離
  * 將你的虛擬機匯集到同一主機硬體中
  * 隔離付款處理工作負載

* 使用預留確保資源可用性
  * Google Cloud 允許你為工作負載定義[預留](https://cloud.google.com/compute/docs/instances/reserving-zonal-resources?hl=zh-cn)，以確保這些資源始終可用
  * 創建預留不會產生額外費用，但即使不使用預留資源，你也需要支付費用
    * [使用和管理預留](https://cloud.google.com/compute/docs/instances/reserving-zonal-resources?hl=zh-cn)


### 虛擬機遷移

* 評估內置的遷移工具，以便將工作負載從其他雲平台或本地環境遷移
  * [遷移到 Google Cloud](https://cloud.google.com/architecture/migration-to-gcp-getting-started?hl=zh-cn)
  * [Google Cloud Rapid Assessment & Migration Program](https://cloud.google.com/solutions/cloud-migration-program?hl=zh-cn)

* 使用導入虛擬磁盤以導入自定義的操作系統
  * [受支持的操作系統](https://cloud.google.com/compute/docs/images/os-details?hl=zh-cn#import)
  * [導入虛擬磁盤](https://cloud.google.com/compute/docs/import/importing-virtual-disks?hl=zh-cn)

