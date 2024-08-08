# Kind建置流程

要在 Ubuntu 上使用 KIND (Kubernetes IN Docker) 架設一個包含 3 個control-plane和 2 個worker的 Kubernetes 集群，可以按照以下步驟進行：

### 1. 安裝 Docker:
KIND 需要 Docker 來運行 Kubernetes 節點。可以參考 Docker 的官方文檔來安裝 Docker。

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### 2. 安裝 Kind:

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```
### 3. 配置 KIND Cluster
創建 KIND 配置文件:
創建一個名為 kind-config.yaml 的配置文件，內容如下：
```yaml
cat <<EOF > kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30950
    hostPort: 30950
    listenAddress: "0.0.0.0"
    protocol: TCP
- role: control-plane
- role: control-plane
- role: worker
- role: worker
EOF
```

這個配置文件指定了集群的節點角色。

### 4.0 創建集群可能需要的前置動作:

#### 4.0.1 創建前可能會遇到與 Docker 權限相關的問題。這通常是因為當前用戶沒有權限訪問 Docker daemon。以下是解決這個問題的幾個步驟：

將當前用戶添加到 Docker 組:

```bash
sudo usermod -aG docker $USER
```
重新登錄:
為了應用新的組權限，請退出並重新登錄，或者使用以下命令重新載入組權限：

```bash
newgrp docker
```

2. 驗證 Docker 權限
確認 Docker 權限:

驗證當前用戶是否可以運行 Docker 命令：

```bash
docker ps
```
如果沒有任何權限錯誤，說明權限已正確設置。

#### 4.0.1 創建前可能會遇到創建Cluster時無法Join worker的狀況
可能會是遇到Linux 系統中的 inotify 相關問題。inotify 是一個文件系統監控機制，允許程序監控文件系統事件，例如文件的創建、修改和刪除。這些設定用於調整 inotify 的資源限制，以便能夠處理更多的事件和監控更多的文件。

```bash
sysctl -w fs.inotify.max_queued_events=1048576
sysctl -w fs.inotify.max_user_watches=1048576
sysctl -w fs.inotify.max_user_instances=1048576
```

#### fs.inotify.max_queued_events：
- 描述：設置內核能夠排隊處理的 inotify 事件的最大數量。
- 用途：當監控大量文件時，會生成大量的事件。如果這個值設置得過低，可能會導致事件丟失。將其設置為 1048576 可以確保系統能夠排隊處理多達 1048576 個事件，而不會丟失。

#### fs.inotify.max_user_watches：

- 描述：設置單個用戶可以監控的文件和目錄的最大數量。
- 用途：這個設置對於需要監控大量文件或目錄的應用程序（如開發環境、文件同步工具等）特別重要。將其設置為 1048576 可以允許單個用戶監控多達 1048576 個文件和目錄。

#### fs.inotify.max_user_instances：

- 描述：設置單個用戶可以創建的 inotify 實例的最大數量。
- 用途：每個 inotify 實例可以監控一組文件或目錄。這個設置確保用戶可以創建足夠多的 inotify 實例來應對需要監控的大量資源。將其設置為 1048576 可以允許單個用戶創建多達 1048576 個 inotify 實例。



### 4.1 創建集群:
使用 KIND 和配置文件來創建集群：

```bash
kind create cluster --config kind-config.yaml
```

### 5. 驗證集群
安裝 kubectl（如果尚未安裝）:

```bash
curl -LO "https://dl.k8s.io/release/v1.27.0/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

檢查集群狀態:

確保 kubectl 可以連接到你的 KIND 集群並檢查節點狀態：

```bash
kubectl cluster-info
kubectl get nodes
```

這樣，就成功地在 Ubuntu 上使用 KIND 架設了一個擁有 3 個control-plane和 2 個worker的 Kubernetes 集群。




