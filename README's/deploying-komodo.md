follow sqlite deploy from https://komo.do/docs/setup/mongo
```
apt install wget
```
```
wget -P komodo https://raw.githubusercontent.com/moghtech/komodo/main/compose/sqlite.compose.yaml && \
  wget -P komodo https://raw.githubusercontent.com/moghtech/komodo/main/compose/compose.env
```
change env file
```
nano komodo/compose.env
```
```
keep tag latest
change db username and password
change komodo passkey
change time zone
change komodo host
```
deploy komodo
```
docker compose -p komodo -f komodo/sqlite.compose.yaml --env-file komodo/compose.env up -d
```
head to login page
```
http://vm-ip:9120/
```
type in username and password for admin and click sign up
