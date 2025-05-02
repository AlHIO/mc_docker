# Minecraft Docker 化伺服器

## 簡介

本專案透過 Docker 容器化技術，快速且穩定地部署 Minecraft Java Edition 伺服器。使用者可以輕鬆管理版本、記憶體配置與資料持久化，並且支援 PaperMC/Spigot 等擴充功能。

## 目錄

* [特性](#特性)
* [需求](#需求)
* [快速開始](#快速開始)

  * [使用 `docker run`](#使用-docker-run)
  * [使用 Docker Compose](#使用-docker-compose)
* [設定說明](#設定說明)
* [資料持久化](#資料持久化)
* [進階應用](#進階應用)
* [防火牆與網路](#防火牆與網路)
* [常見問題](#常見問題)
* [授權](#授權)

## 特性

* 一鍵部署最新版 Minecraft 伺服器（Java Edition）
* 支援指定版本或 `latest`
* 自動同意 Mojang EULA
* 可調整 JVM 記憶體大小
* 資料持久化：世界存檔、設定檔全部掛載到宿主機
* 容器崩潰後自動重啟
* 支援 PaperMC、Spigot 等自訂伺服器核心

## 需求

* Docker Engine 與 Docker Compose
* 主機內至少 2GB 以上可用記憶體

## 快速開始

### 使用 `docker run`

1. 建立資料資料夾並進入：

   ```bash
   mkdir -p ~/minecraft-server/data
   cd ~/minecraft-server
   ```
2. 執行容器：

   ```bash
   docker run -d \
     --name mc-server \
     -e EULA=TRUE \
     -e MEMORY=4G \
     -e VERSION=latest \
     -p 25565:25565 \
     -v "$(pwd)/data:/data" \
     --restart unless-stopped \
     itzg/minecraft-server:latest
   ```
3. 等待容器下載並啟動，完成後即可透過 `localhost:25565`（或替換成實機 IP）連線。

### 使用 Docker Compose

1. 在專案根目錄建立 `docker-compose.yml`：

   ```yaml
   version: '3.8'
   services:
     mc:
       image: itzg/minecraft-server:latest
       container_name: mc-server
       environment:
         EULA: 'TRUE'
         MEMORY: '4G'
         VERSION: 'latest'
         MODE: 'survival'
         ONLINE_MODE: 'TRUE'
       ports:
         - '25565:25565'
       volumes:
         - ./data:/data
       restart: unless-stopped
   ```
2. 啟動容器：

   ```bash
   docker-compose up -d
   ```
3. 查看日誌：

   ```bash
   docker-compose logs -f
   ```

## 設定說明

* **EULA**：同意 Mojang EULA，必填 `TRUE`
* **MEMORY**：JVM 最大記憶體，如 `2G`, `6G`
* **VERSION**：伺服器版本，可用 `latest`、指定數字（如 `1.20.2`）或 PaperMC：`paper/latest`
* **MODE**：遊戲模式，`survival`, `creative`, `adventure`
* **ONLINE\_MODE**：正版驗證，`TRUE` 或 `FALSE`

## 資料持久化

所有伺服器資料（世界檔案、logs、settings）都會儲存在專案 `data` 資料夾內。即使重建或升級容器，也不會遺失資料。

## 進階應用

* **安裝插件**：在 `data/plugins` 資料夾放置 `.jar`，重新啟動容器即可載入。
* **自訂 `server.properties`**：初次啟動後，`data/server.properties` 會自動生成，編輯後容器重啟生效。
* **備份機制**：可利用外部腳本定時將 `data` 資料夾打包並上傳到遠端儲存。

## 防火牆與網路

* 開放 TCP 25565 通訊埠；如位於 NAT/路由器後方，請設定對應的 Port Forwarding。
* LAN 連線：填寫宿主機內網 IP
* 公網連線：填寫伺服器公網 IP 或 DDNS 網域

## 常見問題

**Q1: 為什麼無法連線？**

1. 確認容器是否運作：`docker ps | grep mc-server`
2. 檢查日誌錯誤：`docker logs mc-server`
3. 檢查埠是否綁定：`ss -tln | grep 25565`
4. 確認防火牆／路由轉發設定

**Q2: 版本不相容？**
請在 Minecraft 啟動器中切換到跟伺服器相同的 `VERSION`。

## 授權

此專案採用 MIT License，詳情請見 [LICENSE](./LICENSE)。
