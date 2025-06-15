# 🧰 Tutorial - Compile Windows Wallet on Ubuntu Server 22.04

Compile a wallet for Microsoft Windows on Ubuntu Server 22.04 with the following tutorial.

---

### ✅ Step-by-step Commands:

```bash
# Update Ubuntu
sudo apt-get update && sudo apt-get upgrade -y

# Install base dependencies
sudo apt-get install make automake cmake curl g++-multilib libtool binutils-gold bsdmainutils pkg-config python3 patch bison -y

# Create source directory
cd ~
mkdir source_code
cd source_code

# Download source code
wget "https://dl.walletbuilders.com/download?customer=7fd7ad0365703844c530ed1b76f8838f9e81a832a1178f1a56&filename=kingpepe-source.tar.gz" -O kingpepe-source.tar.gz

# Extract source
tar -xzvf kingpepe-source.tar.gz

# Install 64-bit Windows compiler
sudo apt-get install g++-mingw-w64-x86-64 -y

# Set compiler to POSIX mode
sudo update-alternatives --set x86_64-w64-mingw32-g++ /usr/bin/x86_64-w64-mingw32-g++-posix

# Build dependencies for Windows target
PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g')
cd depends
make HOST=x86_64-w64-mingw32
cd ..

# Configure and compile
./autogen.sh
CONFIG_SITE=$PWD/depends/x86_64-w64-mingw32/share/config.site ./configure --prefix=/
make
