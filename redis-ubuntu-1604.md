# Redis auf Ubuntu 16.04 installieren

## Dank an Digital Ocean, von denen diese Anleitung stammt

### Vorbereitung

```
sudo apt-get update
sudo apt-get install build-essential tcl
```

### Redis Source downloaden

```
curl -O http://download.redis.io/redis-stable.tar.gz
```

### Entpacken
```
tar xzvf redis-stable.tar.gz
```
### Build und Installieren
```
cd redis-stable
make
make test
sudo make install
```

### Redis konfigurieren

```
sudo mkdir /etc/redis
sudo cp /tmp/redis-stable/redis.conf /etc/redis
sudo nano /etc/redis/redis.conf
```

#### Zeile auskommentieren und ersetzen durch:
```
#supervised no
```
```
supervised systemd
```

#### Das Directory setzen, in das Redis schreibt

```
#dir ./
```
Auskommentieren und erstzen durch
```
dir <my-data-directory-for-redis>
```

### Service File für Redis einrichten

```
sudo nano /etc/systemd/system/redis.service
```

In diese Datei muss rein:

```
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=redis
Group=redis
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

### Redis User und Gruppen einrichten

```
sudo adduser --system --group --no-create-home redis
sudo mkdir /data/redis
sudo chown redis:redis <my-data-directory-for-redis>
sudo chmod 770 <my-data-directory-for-redis>
```





