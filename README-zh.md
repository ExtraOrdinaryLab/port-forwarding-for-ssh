# 使用 DuckDNS 和 Port Forwarding 在外部連接到家裡的電腦

這篇指南將帶你一步步完成在外部連接到家裡的電腦的設置過程，適用於使用 TOTOLINK 路由器的用戶。

## 前置條件
- 家裡的路由器支援 Port Forwarding（本文以 TOTOLINK 為例）。
- 你的電腦運行的是 Linux 系統（如 Ubuntu）。
- 你需要一個 DuckDNS 帳號來綁定動態 IP。

## 步驟 1: 設定路由器的 Port Forwarding

首先，我們需要在家裡的路由器上設置 Port Forwarding，讓外部網路能夠訪問家裡的電腦。

1. 打開瀏覽器，進入路由器管理頁面（通常是 [192.168.0.1](http://192.168.0.1)）。
2. 登錄後，找到 **Port Forwarding** 設定頁面。
3. 添加以下規則：
   - **External Port**: 2222
   - **Internal IP**: 192.168.1.100
   - **Internal Port**: 22
   - **Protocol**: TCP

### 如何找到 Internal IP
在你的電腦上執行以下指令來查找內部 IP 地址：
```bash
ip addr
```
找到類似 `192.168.x.x` 的 IP 地址，這就是你的 Internal IP。

## 步驟 2: 安裝並啟用 SSH

接下來，我們需要在家裡的電腦上安裝並啟用 SSH 服務，這樣才能遠端連接。

1. 更新系統軟體包：
   ```bash
   sudo apt update
   ```
2. 安裝 OpenSSH Server：
   ```bash
   sudo apt install openssh-server
   ```
3. 啟用並啟動 SSH 服務：
   ```bash
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```

## 步驟 3: 使用 DuckDNS 綁定動態 IP

如果你的家用網路使用動態 IP（每次重新連接網路時，IP 可能會改變），你需要 DuckDNS 來提供一個固定的域名。

1. 打開 [DuckDNS 官網](https://www.duckdns.org/) 並註冊一個帳號。
2. 登錄後，創建一個子域名（例如：`myhomepc`），完成後你將獲得一個域名，例如：`myhomepc.duckdns.org`。
3. 記下你的 DuckDNS Token，並設置一個定時任務來自動更新 IP：
   ```bash
   echo "*/5 * * * * curl -k 'https://www.duckdns.org/update?domains=myhomepc&token=YOUR_TOKEN&ip='" | crontab -
   ```
   替換 `YOUR_TOKEN` 為你的 DuckDNS Token。

## 步驟 4: 測試遠端連接

完成以上所有設置後，你可以嘗試從外部連接到家裡的電腦。

使用以下指令進行測試：
```bash
ssh username@myhomepc.duckdns.org -p 2222
```
- 替換 `username` 為你的家用電腦用戶名。
- 如果一切設定正確，你應該能成功連接到家裡的電腦！

## 注意事項
- 確保你的路由器和電腦的防火牆允許 2222 端口的連接。
- 遠端連接時，請確保使用強密碼或其他安全措施來保護你的 SSH 服務。

## 結語

通過以上步驟，你現在應該能夠從外部安全地連接到家裡的電腦了。如果在過程中遇到問題，可以檢查每個步驟是否正確執行，或者查看相關的錯誤日誌進行排查。
