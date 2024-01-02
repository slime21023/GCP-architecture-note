# Manage Cloud Resources

### 資源階層結構 Resource hierarchy

Google Cloud 資源以階層方式整理至組職、文件夾和專案中。這種層次結構提供管理資源的方法
* 訪問權限控制
* 配置設置
* 政策


### 資源標籤和標記 Resource labels and tags

提供使用 labels 跟 tags 組職資源

### 使用簡單文件夾結構

文件夾可用來分組隨意組合項目和子文件夾
* 創建一個簡單的文件夾結構來整理資源
* 根據需要添加更多層級，以便定義能夠滿足業務需求的資源層次

[more detail](https://cloud.google.com/resource-manager/docs/creating-managing-folders?hl=zh-cn)


### 使用文件夾和專案來體現資料治理政策

使用文件夾、子文件夾和項目將各種資源分隔開來，以反映組職內的數據治理策略
* 使用文件夾和專案的組合來分隔財務、人力資源和工程領域的資源

[more detail](https://cloud.google.com/architecture/landing-zones/decide-resource-hierarchy?hl=zh-cn)

### 使用標記和標籤

標記提供了一種有條件地允許或拒絕政策的方法，具體取決於資源是否具有特定的標記
* 標籤是一種鍵值對，可幫助組織Google Cloud 實例
* [標籤和標記](https://cloud.google.com/resource-manager/docs/tags/tags-overview?hl=zh-cn)
* [標籤要求](https://cloud.google.com/resource-manager/docs/creating-managing-labels?hl=zh-cn#requirements)
* [支持標籤的服務列表](https://cloud.google.com/resource-manager/docs/creating-managing-labels?hl=zh-cn#label_support)
* [標籤格式](https://cloud.google.com/compute/docs/labeling-resources?hl=zh-cn)
* 管理資源結算: 標籤在結算系統中可用，可讓你按標籤劃分費用
* 按類似特徵或關係對資源進行分組: 可以使用標籤來分離不同的應用生命週期階段或環境


### 避免大量創建標籤
建議在專案層級上建立標籤，並避免在子團隊層級上創建標籤

### 避免在標籤中添加敏感信息
標籤設計不宜用於處理敏感信息，勿在標籤中添加敏感信息，包括個人身份信息


### 對專案名稱中的信息進行匿名處理

遵循專案命民模式(如: COMPANY_INITIAL_IDENTIFIER-ENVIRONMENT-APP_NAME), 不顯示公司或應用名稱，也不包含將來可能會更改的特性: 如團隊名稱或技術

### 應用標記以建構業務維度模型

可以使用標記來為更多業務維度建構模型，如組職結構、區域、工作負載類型或成本中心
* [標記概覽](https://cloud.google.com/resource-manager/docs/tags/tags-overview?hl=zh-cn)
* [標記繼承](https://cloud.google.com/resource-manager/docs/tags/tags-overview?hl=zh-cn#inheritance)
* [創建和管理標記](https://cloud.google.com/resource-manager/docs/tags/tags-creating-and-managing?hl=zh-cn)
* [標記和訪問權限](https://cloud.google.com/iam/docs/tags-access-control?hl=zh-cn)

### 組職政策

* 建立專案命名慣例: 如**SYSTEM_NAME-ENVIRONMENT** (dev, test, uat, stage, prod)
* 自動創建項目:
  * 為生產環境或大型企業創建項目，使用自動化工具: Deployment Manager 或 Terraform
  * 自動創建具有適當權限的開發、測試和生產環境或專案
  * 配置日誌紀錄和監控

* 定期審核系統
* 採用一致的方式配置專案
  * 項目ID和命名慣例
  * 結算帳號關聯
  * 網路配置
  * 已啟用的API和服務
  * [專案移除安全鎖](https://cloud.google.com/resource-manager/docs/project-liens?hl=zh-cn)

* 隔離工作負載或環境
  * 在專案層級實施配額和限制，在專案層級上隔離工作負載或環境: [處理配額](https://cloud.google.com/docs/quota?hl=zh-cn)
  * 分離環境與資料分類的要求不同，將資料與基礎架構分離可能費用高昂且實施複雜，建議根據數據敏感性和合規性要求實現資料分類

* 實施結算隔離: 以支持不同的結算帳號以及每個工作負載和環境的費用可見性
  * [創建、修改或關閉自助 Cloud Billing 帳號](https://cloud.google.com/billing/docs/how-to/manage-billing-account?hl=zh-cn)
  * [啟用、停用或更改專案的結算功能](https://cloud.google.com/billing/docs/how-to/modify-project?hl=zh-cn)

* 使用組職政策服務來控制資源
  * [組職政策服務](https://cloud.google.com/resource-manager/docs/organization-policy/overview?hl=zh-cn)讓政策管理員可通過程序化方式對組職的資源進行集中控制，以實現跨資源階層結構配置約束: [添加組職政策管理員](https://cloud.google.com/resource-manager/docs/organization-policy/using-constraints?hl=zh-cn#add-org-policy-admin)

* 使用組職政策服務以遵守監管政策
  * 為滿足法規要求，可以使用[組職政策服務](https://cloud.google.com/resource-manager/docs/organization-policy/overview?hl=zh-cn)來實施資源共享和訪問方面的合規性要求
  * 可限制與外部各方的共享，或確定在哪個地理位置部屬資源
  * 集中控制以配置定義組織資源的使用方式的限制
  * 定義和制定政策以幫助開發團隊保持在合規性邊界內
  * 幫助專案所有者及其團隊更改系統，同時保持合規性，以及最大限度地減少違反合規性要求的問題

* 根據網域限制資源共享: 受限共享組織政策可幫助防止與外部的身分共享Google Cloud 資源
  * [按網域限制身分](https://cloud.google.com/resource-manager/docs/organization-policy/restricting-domains?hl=zh-cn)
  * [組職政策限制條件](https://cloud.google.com/resource-manager/docs/organization-policy/org-policy-constraints?hl=zh-cn)

* 停用服務帳號和密鑰創建功能: 為了幫助提高安全性，請限制 Identity and Access Management (IAM) 服務帳號和相應密鑰的使用
  * [限制服務帳號的使用](https://cloud.google.com/resource-manager/docs/organization-policy/restricting-service-accounts?hl=zh-cn)

* [限制新資源的物理位置](https://cloud.google.com/resource-manager/docs/organization-policy/defining-locations?hl=zh-cn)


