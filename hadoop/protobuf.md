```bash
sudo apt -y install autoconf automake libtool curl make g++ unzip

git clone git@github.com:protocolbuffers/protobuf.git

cd protobuf

git checkout origin/3.7.x

git submodule update --init --recursive

./autogen.sh

./configure

make -j$(nproc)

make check -j$(nproc)

sudo make install

sudo ldconfig

protoc --version
```
