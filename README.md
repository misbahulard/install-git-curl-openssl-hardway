# How to Install GIT, cURL, OpenSSL in Hard Way

I created this repository because I used to have problems when I wanted to install the latest version of git, while the environment at that time was centos 6 and couldn't install from the latest repositories so I couldn't install the latest git, curl, and OpenSSL.

I need to use the latest version or at least version that supports TLS 1.2 because right now almost all website is using them instead of using the older one.

---

## Test Environment

- Whatever Linux based on centos 6
- openssl 1.0.2o
- curl 7.47.1
- git 2.32.0
- working directory: `/opt/src`
- binary path: `/opt/{openssl, curl, git}`

---

## Install OpenSSL

### Download openssl

```
cd /opt/src

wget https://www.openssl.org/source/openssl-1.0.2o.tar.gz

tar xzvf openssl-1.0.2o.tar.gz
```

### Configure the openssl before compile.
- --prefix = define the path where we install the binary and library.
- --openssldir = define the path where we install the binary and library.

You can choose it to `/usr` in the real world, but for now we just tested in `/opt/ssl`.

```
./config --prefix=/opt/ssl --openssldir=/opt/ssl shared zlib
```

### Make

Keep an eye in the compile output, you will se an error message if there are something errors.

```
make
```

### Make Install

```
make install
```

### Update ldconfig

Add new config in `/etc/ld.so.conf.d`.

This will update the dynamic libary in the systems, so it can be discover by program that need the libararies.

```
echo '/opt/ssl/lib' > /etc/ld.so.conf.d/openssl-1.0.2o.conf
```

After that execute this to reload, and look at the output if your library path already loaded properly.

```
ldconfig -v
```

---

## Install cURL

### Download curl

```
cd /opt/src

wget --no-check-certificate https://curl.se/download/curl-7.47.1.tar.gz

tar xzvf curl-7.47.1.tar.gz
```

### Configure the curl before compile.
- --prefix = define the path where we install the binary and library.
- --with-ssl = define the path where openssl library that want to use.

You can choose it to `/usr` in the real world, but for now we just tested in `/opt/curl`.

```
./configure --prefix=/opt/curl --with-ssl=/opt/ssl
```

### Make

Keep an eye in the compile output, you will se an error message if there are something errors.

```
make
```

### Make Install

```
make install
```

### Update ldconfig

Add new config in `/etc/ld.so.conf.d`.

This will update the dynamic libary in the systems, so it can be discover by program that need the libararies.

```
echo '/opt/curl/lib' > /etc/ld.so.conf.d/curl-7.47.1.conf
```

After that execute this to reload, and look at the output if your library path already loaded properly.

```
ldconfig -v
```

---

## Install GIT

### Download GIT

```
cd /opt/src

wget --no-check-certificate https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.32.0.tar.gz

tar xzvf git-2.32.0.tar.gz
```

### Configure the curl before compile.
- --prefix = define the path where we install the binary and library.
- --with-curl = define the path where curl library that want to use.

You can choose it to `/usr` in the real world, but for now we just tested in `/opt/git`.

```
./configure --prefix=/opt/git --with-curl=/opt/curl
```

### Make

Keep an eye in the compile output, you will se an error message if there are something errors.

```
make
```

### Make Install

```
make install
```

### Try GIT

If you don't see error related SSL anymore, it's works!
Of course you need to link the binary to `/bin` or `/usr/bin`, or install the git directly to the system instead of using `/opt` directory.

```
/opt/git/bin/git clone https://github.com/git/git.git
```
