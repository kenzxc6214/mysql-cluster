# 進入 Master 容器
docker exec -it master mysql -uroot -p123456
GRANT REPLICATION CLIENT, REPLICATION SLAVE ON *.* TO repl@'%' IDENTIFIED BY 'repl';

# 進入 Slave 容器
docker exec -it slave mysql -uroot -p123456  
CHANGE MASTER TO MASTER_HOST='master', MASTER_USER='repl', MASTER_PASSWORD='repl';
start slave;

# 檢查一下 Slave 是否正常工作
show slave status\G

# 測試 Master/Slave 同步
docker exec master mysql -uroot -p123456 -e "CREATE DATABASE test"
docker exec slave mysql -uroot -p123456 -e "SHOW DATABASES"