# Minecraft Bedrock Version Server On RaspberryPi 4  
  
  
## 1.更新系統並安裝 Docker  
```
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
```

## 2.新增「pi」用戶至 docker 群組（可免 sudo 執行)  
```
sudo usermod -aG docker $USER
newgrp docker
```
## 3.建立伺服器目錄與 docker-compose.yml  
```
mkdir -p ~/minecraft-bedrock
cd ~/minecraft-bedrock
mkdir -p data 
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  bedrock:
    image: itzg/minecraft-bedrock-server
    container_name: bedrock_server
    restart: unless-stopped
    ports:
      - "19132:19132/udp"
    environment:
      EULA: "TRUE"            
      GAMEMODE: "survival"    # survival/creative
      SERVER_NAME: "PiBedrock"
      LEVEL_NAME: "world"
    volumes:              
      - ./data:/data      
EOF
```
## 4.啟動伺服器  
```
docker-compose up -d
```

## 5.檢查伺服器運行狀態  
```
docker logs -f bedrock_server
```
## 6.關閉伺服器  
```
docker-compose down 
```
#####################################################################

```
mkdir -p ~/minecraft-bedrock/temp-world  
unzip ~/Downloads/20250430RasberryPi4.mcworld -d ~/minecraft-bedrock/temp-world
```
/data 裡面可以更改伺服器設定 世界設定玩家權限等  
