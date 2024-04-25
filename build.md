<!-- vscode-markdown-toc -->
- [Git](#git)
- [Hadoop](#hadoop)
    - [Open JDK 1.8](#open-jdk-18)
    - [Maven](#maven)
    - [Native libraries](#native-libraries)
    - [CMake](#cmake)
    - [Protocol Buffers required to build native code](#protocol-buffers-required-to-build-native-code)
    - [Boost](#boost)
    - [Optional packages:](#optional-packages)
        - [Snappy compression only used for hadoop-mapreduce-client-nativetask](#snappy-compression-only-used-for-hadoop-mapreduce-client-nativetask)
        - [Intel ISA-L library for erasure coding](#intel-isa-l-library-for-erasure-coding)
        - [Bzip2](#bzip2)
        - [Linux FUSE](#linux-fuse)
        - [ZStandard compression](#zstandard-compression)
        - [PMDK library for storage class memorySCM as HDFS cache backend](#pmdk-library-for-storage-class-memoryscm-as-hdfs-cache-backend)
    - [Building Hadoop](#building-hadoop)
        - [without native code](#without-native-code)
        - [with native code and others](#with-native-code-and-others)
- [Tez](#tez)
    - [Building Tez](#building-tez)
- [Hive](#hive)
    - [Building Hive](#building-hive)
<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

# Git

ref: [https://git-scm.com/download/linux](https://git-scm.com/download/linux)

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt -y install git
```

# Hadoop

ref: [https://github.com/apache/hadoop/blob/branch-3.4.0/BUILDING.txt](https://github.com/apache/hadoop/blob/branch-3.4.0/BUILDING.txt)

* ### Open JDK 1.8

  ```bash
  sudo apt update
  sudo apt -y install openjdk-8-jdk
  ```
 
  `nano ~/.bashrc`    

  ```bash
  # JAVA_HOME
  export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
  export PATH=$PATH:$JAVA_HOME/bin
  ```

  `source ~/.bashrc`

  ```bash
  echo $JAVA_HOME
  java -version
  ```

* ### Maven

`sudo apt -y install maven`

or

```bash
wget http://ftp.twaren.net/Unix/Web/apache/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
tar -zxf apache-maven-3.9.6-bin.tar.gz
sudo mv apache-maven-3.9.6 /opt/maven
```

`nano ~/.bashrc`

```bash
# M2_HOME
export M2_HOME=/opt/maven
export MAVEN_HOME=$M2_HOME
export PATH=$PATH:$M2_HOME/bin
```

`source ~/.bashrc`

```bash
echo $M2_HOME
mvn -v
```

* ### Native libraries

`sudo apt -y install build-essential autoconf automake libtool zlib1g-dev pkg-config libssl-dev libsasl2-dev`

* ### CMake

`sudo apt -y install cmake`

or

```bash
git clone https://github.com/Kitware/CMake.git
cd CMake
./bootstrap
make -j$(nproc)
sudo make install
```

`cmake -version`

* ### Protocol Buffers (required to build native code)

ref: [https://github.com/protocolbuffers/protobuf/blob/v3.21.12/src/README.md](https://github.com/protocolbuffers/protobuf/blob/v3.21.12/src/README.md)

```bash
sudo apt -y install autoconf automake libtool curl make g++ unzip
git clone https://github.com/protocolbuffers/protobuf.git
cd protobuf
git checkout v3.21.12
git submodule update --init --recursive
./autogen.sh
./configure
make -j$(nproc)
make check
sudo make install
sudo ldconfig
protoc --version
```

* ### Boost

`sudo apt -y install libboost-all-dev`

or

```bash
wget https://boostorg.jfrog.io/artifactory/main/release/1.85.0/source/boost_1_85_0.tar.bz2
tar --bzip2 -xf boost_1_85_0.tar.bz2
cd boost_1_85_0
./bootstrap.sh --prefix=/usr/
./b2 --without-python
sudo ./b2 --without-python install
```

* ### Optional packages:

* #### Snappy compression (only used for hadoop-mapreduce-client-nativetask)

`sudo apt -y install libsnappy-dev`

* #### Intel ISA-L library for erasure coding

`sudo apt -y install libisal-dev`

or

```bash
git clone https://github.com/intel/isa-l
cd isa-l
./autogen.sh
./configure
make -j$(nproc)
sudo make install
```

* #### Bzip2

`sudo apt install bzip2 libbz2-dev`

* #### Linux FUSE

`sudo apt install libfuse3-dev`

* #### ZStandard compression

`sudo apt install libzstd-dev`

* #### PMDK library for storage class memory(SCM) as HDFS cache backend

ref: [http://pmem.io/](http://pmem.io/) and [https://github.com/pmem/pmdk](https://github.com/pmem/pmdk)

All Runtime:

`sudo apt -y install libpmem1 librpmem1 libpmemblk1 libpmemlog1 libpmemobj1 libpmempool1`

***All Development:***

`sudo apt -y install libpmem-dev librpmem-dev libpmemblk-dev libpmemlog-dev libpmemobj-dev libpmempool-dev libpmempool-dev`

All Debug:

`sudo apt -y install libpmem1-debug librpmem1-debug libpmemblk1-debug libpmemlog1-debug libpmemobj1-debug libpmempool1-debug`

* ### Building Hadoop

```bash
git clone https://github.com/apache/hadoop.git
cd hadoop
git checkout rel/release-3.4.0
```

modify ***hadoop-project/pom.xml***

```xml
<!-- <slf4j.version>1.7.36</slf4j.version> -->
<slf4j.version>2.0.13</slf4j.version>

<!-- <protobuf.version>2.5.0</protobuf.version> -->
<protobuf.version>3.21.12</protobuf.version>

<!-- <zookeeper.version>3.8.3</zookeeper.version> -->
<zookeeper.version>3.8.4</zookeeper.version>

<!-- <nodejs.version>v12.22.1</nodejs.version> -->
<nodejs.version>v12.22.12</nodejs.version>

<!-- <yarnpkg.version>v1.22.5</yarnpkg.version> -->
<yarnpkg.version>v1.22.19</yarnpkg.version>
```

* #### without native code

`mvn package -DskipTests -Dmaven.javadoc.skip=true -Pdist,native -Dtar -Pyarn-ui`

* #### with native code and others

```bash
mvn package -DskipTests -Dmaven.javadoc.skip=true -Pdist,native -Dtar -Pyarn-ui \

-Drequire.snappy \
-Dbundle.snappy \
-Dsnappy.prefix=/usr/lib/x86_64-linux-gnu \
-Dsnappy.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.zstd \
-Dbundle.zstd \
-Dzstd.prefix=/usr/lib/x86_64-linux-gnu \
-Dzstd.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.openssl \
-Dbundle.openssl \
-Dopenssl.prefix=/usr/lib/x86_64-linux-gnu \
-Dopenssl.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.isal \
-Dbundle.isal \
-Disal.prefix=/usr/lib/x86_64-linux-gnu \
-Disal.lib=/usr/lib/x86_64-linux-gnu \

-Drequire.pmdk \
-Dbundle.pmdk \
-Dpmdk.lib=/usr/lib/x86_64-linux-gnu
```

# Tez

ref: [https://github.com/apache/tez/blob/master/BUILDING.txt](https://github.com/apache/tez/blob/master/BUILDING.txt)

```bash
git clone https://github.com/apache/tez.git
cd tez
git checkout rel/release-0.10.3
```

modify ***pom.xml***

```xml
<!-- <hadoop.version>3.3.6</hadoop.version> -->
<hadoop.version>3.4.0</hadoop.version>

<!-- <protobuf.version>3.21.1</protobuf.version> -->
<protobuf.version>3.21.12</protobuf.version>

<!-- <slf4j.version>1.7.36</slf4j.version> -->
<slf4j.version>2.0.13</slf4j.version>
```

modify ***tez-ui/src/main/webapp/app/routes/server-side-ops.js***

```js
// line 77: that.get("loadedValue").pushObjects(data.content);
that.get("loadedValue").pushObjects(data);
```

(optional) modify ***tez-ui/src/main/webapp/app/utils/table-definition.js***

```js
// line 48: rowCountOptions: [5, 10, 25, 50, 100],
rowCountOptions: [5, 10, 15, 20, 25, 50, 100],
```

* ### Building Tez

`mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Phadoop28 -P\!hadoop27`

# Hive

```bash
git clone https://github.com/apache/hive.git

cd hive

git checkout rel/release-4.0.0
```

modify ***pom.xml***

```xml
<!-- <slf4j.version>1.7.36</slf4j.version> -->
<slf4j.version>2.0.13</slf4j.version>

<!-- <zookeeper.version>3.8.3</zookeeper.version> -->
<zookeeper.version>3.8.4</zookeeper.version>
```

* ### Building Hive

`mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Pdist -Drat.skip=true`
