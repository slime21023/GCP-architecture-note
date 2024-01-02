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

https://cloud.google.com/architecture/framework/system-design/compute?hl=zh-cn#manage-ops