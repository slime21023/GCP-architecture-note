# Design your network infrastructure

### 核心原則

部屬雲端網路設計包括以下步驟:
1. 設計工作負載VPC架構
   * 首先確定需要多少Google Cloud專案和VPC網路
2. 添加VPC間連接
   * 設計如何將工作負載連接到不同VPC網路中的其他工作負載
3. 設計混合網路連接
   * 設計如何將你的工作負載VPC連接到本地和其他雲環境

設計Google Cloud 網路時，請考慮以下事項:
* VPC 可在雲中提供私有網路環境，用於將基於 Compute Engine、 Google Kubernetes Engine (GKE)和無服務器計算解決方案建構的服務相互連接起來
* 可以使用VPC以私密方式訪問Google管理的服務
  * Cloud Storage
  * BigQuery
  * Cloud SQL

* VPC網路(包括其關連的路由和防火牆規則)屬於全球性資源; 它們與
任何特定區域或可用區均無關連

* 子網屬於區域級資源
  * 在同一雲區域中的不同可用區部屬的 Compute Engine 虛擬機實例可以使用同一子網中的IP地址

* 進出實例的流量可以通過 VPC 防火牆規則來控制
* 可以使用 [Identity and Access Management(IAM) 角色](https://cloud.google.com/iam/docs/understanding-roles?hl=zh-cn)來保護網路管理
* 可以使用 [Cloud VPN](https://cloud.google.com/network-connectivity/docs/vpn/concepts/overview?hl=zh-cn) 或 [Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/interconnect?hl=zh-cn)在混合環境中安全連接VPC網路


### 工作負載 VPC 架構

* 提早考慮VPC網路設計事宜
  * 組職層級的設計選擇在流程後期無法輕易撤銷
  * [VPC設計的最佳實踐和參考架構](https://cloud.google.com/architecture/best-practices-vpc-design?hl=zh-cn)
  * [確定Google Cloud 著陸區的網路設計](https://cloud.google.com/architecture/landing-zones/decide-network-design?hl=zh-cn)

* 從單個VPC網路開始
  * 若有許多用例包含具有相同要求的資源，那麼可以使用單個VPC所需的功能
  * 單個VPC網路易於創建、維護和理解
  * [VPC網路規範](https://cloud.google.com/vpc/docs/vpc?hl=zh-cn#specifications)

* 簡化VPC網路拓撲
  * 為了確保架構可靠且易於管理和理解，請使用簡單易行的VPC網路拓撲設計

* 在自定義模式下使用VPC網路
  * 為確保Google Cloud 網路能與你現有的網路系統無縫集成，建議使用[自定義模式](https://cloud.google.com/vpc/docs/vpc?hl=zh-cn#subnet-ranges)來創建VPC網路
  * 使用自定義模式有助於你將Google Cloud 網路集成到現有IP地址管理方案中，讓你能夠控制VPC中包含哪些雲區域


### VPC 間連接

* 選擇VPC連接方法
  * VPC 網路是Google Andromeda 軟體定義網路(SDN)內的獨立租戶空間
  * 不同 VPC 網路可通過多種方式相互通信
  * [選擇滿足你的費用、性能和安全性需求的VPC連接方法](https://cloud.google.com/architecture/best-practices-vpc-design?hl=zh-cn#choose-method)
* 使用共享VPC來管理多個工作組
  * 多具有多個團隊的組職來說，共享VPC可以有效簡化單個VPC網路在多個工作組之間的架構
* 使用簡單的命名慣例
  * 選用簡單、直觀且一致的命名慣例，有助於管理員和用戶了解各個資源的用途、位置以及與其他資源的區別
* 使用Connectivity Tests 驗證網路安全性
  * 在網路安全性方面，可以使用 Connectivity Tests來驗證兩個端點之間的流量是否按照你的預期被阻止
  * [Connectivity Tests概覽](https://cloud.google.com/network-intelligence-center/docs/connectivity-tests/concepts/overview?hl=zh-cn)

* 使用 Private Service Connect 創建專用端點
  * 若需創建可使用自己的 IP 地址訪問 Google 服務的專用端點，請使用 [Private Service Connect](https://codelabs.developers.google.com/codelabs/cloudnet-psc?hl=zh-cn#0)
  * 可以從 VPC 內部以及通過在 VPC 中終止的混合連接來訪問專用端點

* 保護和限制外部連接
  * 將互聯網訪問權限僅授予那些需要該權限的資源
  * 僅具有專用內部 IP 地址的資源可以通過[專用Google 訪問通道](https://cloud.google.com/vpc/docs/private-google-access?hl=zh-cn)來訪問多個Google API 和服務

* 使用Network Intelligence Center 監控雲網路
  * [Network Intelligence Center](https://cloud.google.com/network-intelligence-center?hl=zh-cn)
  * 它可以幫助你識別可能導致維運安全風險的流量和訪問模式
  