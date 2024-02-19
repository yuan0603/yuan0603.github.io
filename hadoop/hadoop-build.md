# Hadoop Build

| Package | Version               |
| ------- | --------------------- |
| Ubuntu  | 22.04.3 LTS           |
| OpenJDK | 8                     |
| Hadoop  | 3.3.6                 |
| Tez     | 0.10.3-SNAPSHOT       |
| Hive    | 4.0.0-beta-2-SNAPSHOT |

## Required packages：

## 1. Open JDK 8

```bash
sudo apt -y install openjdk-8-jdk
```

* ### 設定 JAVA_HOME 變數

```bash
nano ~/.bashrc

# set JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin

source ~/.bashrc
```

* ### switch java version
```bash
update-java-alternatives
```

## 2. Maven

```bash
sudo apt -y install maven
```

OR

```bash
wget http://ftp.twaren.net/Unix/Web/apache/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz

tar zxf apache-maven-3.9.6-bin.tar.gz

sudo mv apache-maven-3.9.6/ /opt/maven
```

* ### 設定 MAVEN 變數

```bash
nano ~/.bashrc

export M2_HOME=/opt/maven
export MAVEN_HOME=$M2_HOME
export PATH=$PATH:$M2_HOME/bin
```

## 3. Native libraries

```bash
sudo apt -y install build-essential autoconf automake libtool cmake zlib1g-dev pkg-config libssl-dev libsasl2-dev
```

* CMake (Optional)

    ```bash
    wget https://cmake.org/files/v3.26/cmake-3.26.3.tar.gz
    tar -zxvf cmake-3.26.3.tar.gz && cd cmake-3.26.3
    ./bootstrap
    make -j$(nproc)
    sudo make install
    ```

## 4. Buffers 3.7.1 (required to build native code)

```bash
sudo apt -y install autoconf automake libtool curl make g++ unzip

wget https://github.com/protocolbuffers/protobuf/releases/download/v3.7.1/protobuf-java-3.7.1.tar.gz

tar zxf protobuf-java-3.7.1.tar.gz && cd protobuf-3.7.1

./autogen.sh

./configure

make -j$(nproc)

make check -j$(nproc)

sudo make install

sudo ldconfig

protoc --version
```

## 5. Node.js (for YARN UI v2 building)

1. __nvm__

    __reference__ ：[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm#installing-and-updating)

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash

    source ~/.bashrc

    nvm -v
    ```

2. __node__

    ```bash
    nvm ls-remote

    nvm install vX.X.X

    node -v

    npm -v
    ```

---

# Optional packages:

* ## Snappy compression (only used for hadoop-mapreduce-client-nativetask)

```bash
sudo apt -y install libsnappy-dev
```

* ## Intel ISA-L library for erasure coding

```bash
sudo apt -y install libisal-dev
```

* ## Bzip2

```bash
sudo apt -y install bzip2 libbz2-dev
```

* ## Linux FUSE

```bash
sudo apt -y install fuse libfuse-dev
```

* ## ZStandard compression

```bash
sudo apt -y install libzstd-dev
```

* ## PMDK library for storage class memory(SCM) as HDFS cache backend
(Please refer to http://pmem.io/ and https://github.com/pmem/pmdk)

```bash
sudo apt -y install libpmem-dev librpmem-dev libpmemblk-dev libpmemlog-dev libpmemobj-dev libpmempool-dev libpmempool-dev
```



---

# Building distributions

* __build yarn-ui__

nodejs > 14  
python2  

```bash
cd hadoop-yarn-project/hadoop-yarn/hadoop-yarn-ui/src/main/webapp

npm install -g yarn

npm install -g bower

yarn install

bower install

yarn run build

# 修改 hadoop-yarn-project/hadoop-yarn/hadoop-yarn-ui/src/main/webapp/node_modules/temp/lib/temp.js
# line 273: os.tmpDir() -> os.tmpdir()
# 修改後再執行一次 yarn run build

cd hadoop-yarn-project/hadoop-yarn/hadoop-yarn-ui

mvn package -Pyarn-ui
```

* without native code

```bash
mvn package -DskipTests -Dmaven.javadoc.skip=true -Pdist,native -Dtar -Pyarn-ui 2>&1 | tee hadoop-build.log
```

* with native code and others

```bash
mvn package -DskipTests -Dmaven.javadoc.skip=true -Pdist,native -Dtar -Pyarn-ui \

-Drequire.snappy=true \
-Dbundle.snappy=true \
-Dsnappy.prefix=/usr/lib/x86_64-linux-gnu \
-Dsnappy.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.zstd=true \
-Dbundle.zstd=true \
-Dzstd.prefix=/usr/lib/x86_64-linux-gnu \
-Dzstd.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.openssl=true \
-Dbundle.openssl=true \
-Dopenssl.prefix=/usr/lib/x86_64-linux-gnu \
-Dopenssl.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.isal=true \
-Dbundle.isal=true \
-Disal.prefix=/usr/lib/x86_64-linux-gnu \
-Disal.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.pmdk=true \
-Dbundle.pmdk=true \
-Dpmdk.lib=/usr/lib/x86_64-linux-gnu \
2>&1 | tee hadoop-build.log
```
