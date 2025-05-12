# Minecraft Docker 化伺服器

## 簡介

本專案透過 Docker 容器化技術，快速且穩定地部署 Minecraft Java Edition 伺服器。使用者可以輕鬆管理版本、記憶體配置與資料持久化，並且支援 PaperMC/Spigot 等擴充功能。

## 目錄

* [特性](#特性)
* [需求](#需求)
* [快速開始](#快速開始)

  * [使用 ](#使用-docker-run)[`docker run`](#使用-docker-run)
  * [使用 Docker Compose](#使用-docker-compose)
* [設定說明](#設定說明)
* [資料持久化](#資料持久化)
* [進階應用](#進階應用)
* [防火牆與網路](#防火牆與網路)

## 特性

* 一鍵部署最新版 Minecraft 伺服器（Java Edition）
* 支援指定版本或 `latest`
* 自動同意 Mojang EULA
* 可調整 JVM 記憶體大小
* 資料持久化：世界存檔、設定檔全部掛載到宿主機
* 容器崩潰後自動重啟
* 支援 PaperMC、Spigot 等自訂伺服器核心

## 需求

* Docker Engine
* (可選) 已安裝 Docker Compose v2+ （或準備安裝 Compose Plugin）
* 主機內至少 2GB 可用記憶體

## 快速開始

### 使用 `docker run`

1. 啟動容器：

   * 傳統指令（需安裝 `docker-compose` v1）：

     ```bash
     docker-compose up -d
     ```
   * 或新版指令（需安裝 Compose Plugin）：

     ```bash
     docker compose up -d
     ```
2. 查看日誌：

   ```bash
   docker compose logs -f
   ```

3. 打開關閉：

   ```bash
   docker start mc-server
   docker stop mc-server
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
* \*\*自訂 \*\*\`\`：初次啟動後，`data/server.properties` 會自動生成，編輯後容器重啟生效。
* **備份機制**：可利用外部腳本定時將 `data` 資料夾打包並上傳到遠端儲存。

## 防火牆與網路

* 開放 TCP 25565 通訊埠；如位於 NAT/路由器後方，請設定對應的 Port Forwarding。
* LAN 連線：填寫宿主機內網 IP
* 公網連線：填寫伺服器公網 IP 或 DDNS 網域

