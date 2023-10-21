# vapor_ubuntu_20_04

# Install Vapor Server on Ubuntu (nginx, supervisor)
How To Install Vapor Server on Ubuntu 20.04 (root)


> **step 1**

```
sudo apt-get update
```

```
apt-get install \
          binutils \
          git \
          gnupg2 \
          libc6-dev \
          libcurl4 \
          libedit2 \
          libgcc-9-dev \
          libpython2.7 \
          libsqlite3-0 \
          libstdc++-9-dev \
          libxml2 \
          libz3-dev \
          pkg-config \
          tzdata \
          uuid-dev \
          zlib1g-dev \
          mc
```
```
wget https://download.swift.org/swift-5.9-release/ubuntu2004/swift-5.9-RELEASE/swift-5.9-RELEASE-ubuntu20.04.tar.gz
tar xzf swift-5.9-RELEASE-ubuntu20.04.tar.gz
```

# 3 Add the Swift binaries to path
```
mcedit ~/.bashrc
```

# 4 Copy and save in your .bashrc file
```
export PATH=/home/<profile-name>/swift-5.9-RELEASE-ubuntu20.04/usr/bin:”${PATH}”
```

# 5 In your ssh terminal
```
source ~/.bashrc
```
# 6 Check that swift is installed (optional)
```
swift --version
```

# 1
```
git clone https://github.com/vapor/toolbox.git
```
# 2
```
cd toolbox
```
# 3
```
git checkout 18.7.4
```
# 4
```
swift build -c release --disable-sandbox
```
# 5
```
sudo mv .build/release/vapor /usr/local/bin
```

> **step 3**
# Create/Build/Run serve a project
```
mkdir /home/<profile-name>/projects
cd /home/<profile-name>/projects
```

```
vapor new VaporHelloWorldApp
```

```
cd VaporHelloWorldApp/
```

for Release mode:

```
swift run -c release 
```




# Install nginx

The first step is to install Nginx on your server. Connect to your server with ssh and run these commands :
```
sudo apt-get update
sudo apt-get install nginx
```

Once Nginx is installed, run this command to configure Nginx for your application :
```
mcedit /etc/nginx/sites-enabled/default
```

Setup the configuration by editing your server name and the root path :

```
# Default server configuration
#
server {
    server_name hello.com;     
    listen 80;
    root /home/vapor/Hello/Public/;
# Serve all public/static files via nginx and then fallback to   Vapor for the rest
    location / {
        try_files $uri @proxy;
    }
location @proxy {
        proxy_pass http://127.0.0.1:8080;
        proxy_pass_header Server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass_header Server;
        proxy_connect_timeout 3s;
        proxy_read_timeout 10s;
    }
}
```


