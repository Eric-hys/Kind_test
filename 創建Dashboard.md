# 創建Dashboard

### 使用 Grafana 模板 
 
1. **匯入現有模板**  
  - **從 Grafana.com 匯入** :
    1. 打開 Grafana 儀表板。
 
    2. 點擊左側邊欄的 `+` 圖標，然後選擇 `匯入`。
 
    3. 在 `Import via grafana.com` 欄位中，輸入模板的 ID（可以從 [https://grafana.com/grafana/dashboards/14249-cluster-monitor/]()  找到）。
 
    4. 點擊 `Load` 來加載模板。
 
    5. 設定數據源，然後點擊 `Import` 完成匯入。

## Node panel 說明
![g1](https://github.com/Eric-hys/Kind_test/blob/main/images/g1.png)
![g2](https://github.com/Eric-hys/Kind_test/blob/main/images/g2.png)

### Pods Ready 
 
- **說明** : 這個指標表示在集群中處於`Ready`狀態的Pods數量。Pod被認為是`Ready`的時候，其所有的容器都正常運行，並且通過了健康檢查。
 
- **用途** : 確保應用程序的可用性和穩定性。可以幫助快速檢測到因應用程序啟動失敗或其他原因導致的Pod不可用問題。

### Namespaces 
 
- **說明** : Namespaces是在同一集群中邏輯上劃分資源的方式。這個指標會監控Namespace的數量和狀態。
 
- **用途** : 用於管理和隔離不同團隊或應用程序的資源。可以幫助追蹤資源的分配和使用情況，防止資源衝突和過度使用。

### Pods 
 
- **說明** : Pods是Kubernetes中最小的部署單元，通常包含一個或多個容器。這個指標監控集群中Pod的數量和狀態。
 
- **用途** : 了解整個集群中的工作負載分佈情況，確保Pod按預期運行，及時發現和解決Pod相關的問題（如CrashLoopBackOff、Pending等）。

### PVC (Persistent Volume Claims) 
 
- **說明** : PVC是用於向Kubernetes申請持久存儲的方式。這個指標監控PVC的數量和狀態。
 
- **用途** : 確保應用程序有足夠的存儲資源，並且存儲資源的申請和綁定狀態正常。可以幫助發現存儲資源不足或PVC綁定失敗等問題。

### Node Info 
 
- **說明** : 這個指標提供有關節點的詳細信息，如節點的健康狀態、資源使用情況（CPU、內存等）和容量。
 
- **用途** : 監控集群中各個節點的狀態和性能，確保資源的有效利用和集群的健康運行。及時發現和解決節點過載、故障等問題。


#### CPU 使用率（Node CPU Usage） 
 
- **用途** : 測量節點上所有容器的CPU使用情況。高CPU使用率可能意味著需要增加資源或進行優化。

#### Memory 使用率（Node Memory Usage） 
 
- **用途** : 測量節點上所有容器的內存使用情況。內存耗盡可能導致應用程序崩潰。

#### Disk I/O（Node Disk I/O） 
 
- **用途** : 監控節點的磁盤讀寫操作。高磁盤I/O可能表示磁盤瓶頸，需要優化I/O操作或增加磁盤資源。

#### Network I/O（Node Network I/O） 
 
- **用途** : 測量節點的網絡流量。高網絡I/O可能影響應用程序的性能，需確保網絡連接的穩定和帶寬充足。

#### API Server 延遲（API Server Latency） 
 
- **用途** : 測量Kubernetes API Server的響應時間。高延遲可能影響集群管理操作的效率。

## Pod 指標 panel 說明
![g3](https://github.com/Eric-hys/Kind_test/blob/main/images/g3.png)


### Pod Uptime 
 
- **說明** : Pod自啟動以來的運行時間。
 
- **用途** : 確保Pod持續運行並且穩定。如果Pod的Uptime頻繁重置，可能意味著存在應用程序崩潰或配置問題。

### Pod Status 
 
- **說明** : Pod的當前狀態，包括Running、Pending、Succeeded、Failed、CrashLoopBackOff等。
 
- **用途** : 了解Pod的健康狀況和運行情況。可以幫助快速識別和排查Pod的問題，如啟動失敗或運行錯誤。

### Pod Restarts 
 
- **說明** : Pod內部容器的重啟次數。
 
- **用途** : 監控容器的不穩定情況。如果重啟次數過多，可能需要調查應用程序的錯誤或資源分配問題。

### Pod Memory 
 
- **說明** : Pod內存使用量。這個指標通常表示Pod及其容器當前消耗的內存量。
 
- **用途** : 監控內存使用情況，以確保Pod不會耗盡分配的內存資源。高內存使用可能會導致內存不足、應用程序崩潰或其他性能問題。這有助於識別內存洩漏或需要增加資源分配的Pod。

### Pod CPU 
 
- **說明** : Pod的CPU使用量。這個指標表示Pod及其容器當前消耗的CPU資源。
 
- **用途** : 監控CPU使用情況，以確保Pod不會超過其分配的CPU資源。高CPU使用可能會影響應用程序的性能，導致響應緩慢或超時。這有助於識別需要優化或擴展的Pod。

### Pod File System Usage 
 
- **說明** : Pod的文件系統使用情況。這個指標表示Pod內部容器的磁盤使用量，包括已用空間和剩餘空間。
 
- **用途** : 監控磁盤使用情況，確保Pod不會耗盡分配的磁盤空間。過高的磁盤使用可能會導致磁盤空間不足，影響應用程序的正常運行。這有助於檢測和防止磁盤相關的性能瓶頸或錯誤。


### Pod Resource Limit 
 
- **說明** : Pod設置的資源限制，包括CPU和內存。
 
- **用途** : 確保Pod不會使用超過指定的資源，防止資源過度消耗影響其他Pod。幫助進行資源優化和容量規劃。

### Pod Network 
 
- **說明** : Pod的網絡指標，包括網絡流量的傳入和傳出、延遲和錯誤率等。
 
- **用途** : 監控Pod的網絡性能，確保網絡通信順暢。如果出現網絡瓶頸或錯誤，可能需要調整網絡配置或排查網絡問題。

### Pod Threads 
 
- **說明** : Pod內運行的線程數量。
 
- **用途** : 了解應用程序的並發執行情況。如果線程數過多，可能需要調整應用程序的線程管理策略或資源配置，以防止資源耗盡或性能問題。























