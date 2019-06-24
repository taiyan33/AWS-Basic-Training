# AWS-Basic-Training

## LAB 3 使用 使用 EC2 建構 Wordpress
### 3-1 使用 Docker 在30 秒內啟用 Wordpress 初始網站

請一行一行複製指令碼執行:

```bash
# 更新軟體庫
sudo yum update

# 安裝 Docker
sudo yum install docker -y

# 啟動 Docker 服務
sudo systemctl start docker

# 開機時自動啟動 Docker 服務
sudo systemctl enable docker

# 執行 wordpress 並對外發布 port 80
sudo docker run --name wp -d -p 80:80 wordpress

# 查看目前狀態
sudo docker ps -a
```


## LAB 8 Wordpress 網站上線後的自動擴展
### 8-1 EC2 的 AutoScaling 機制與實現

```bash
# 將之前啟用的 wordpress 停用
sudo docker stop wp

# 安裝 httpd 服務
sudo yum install httpd -y

# 啟動 httpd 服務
sudo systemctl start httpd

# 開機啟動 httpd 服務
sudo systemctl enable httpd

# 關機製作 AMI，如此一來這個 images 就擁有開機啟動 httpd 的能力了
```

### 8-2 Stress-NG 壓測 CPU 來觸發擴展機制

```bash
# 利用 Docker 跑壓測軟體 stress-ng 先跑 10 秒看看
sudo docker run --name stress --rm ckmates/stress stress -c 2 --timeout 10s

# 這是跑出來正確，且是示範的資料，不要拷貝!
[ec2-user@ip-172-31-45-217 ~]$ sudo docker run --name stress --rm ckmates/stress stress -c 2 --timeout 10s
stress: info: [1] dispatching hogs: 2 cpu, 0 io, 0 vm, 0 hdd
stress: info: [1] successful run completed in 10s

# 跑壓測讓 CPU 過載
將 --timeout 參數適當放大

```
