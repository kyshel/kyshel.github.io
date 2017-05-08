## Require
```
yum groupinstall "Development Tools" 
yum install cmake gcc gtk2-devel numpy pkconfig
```

## Download
```
wget https://github.com/opencv/opencv/archive/3.2.0.zip -O opencv-3.2.0.zip
unzip opencv-3.2.0.zip
```

## Make
```
cd opencv-3.2.0
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=DEBUG -D CMAKE_INSTALL_PREFIX=/usr/local ..
make
make install
```

## Config
```
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig/
echo '/usr/local/lib/' >> /etc/ld.so.conf.d/opencv.conf
ldconfig
```

## Run
```
cd ~
wget https://raw.githubusercontent.com/kyshel/template/master/hello_cv.cpp
g++ -o hello_cv hello_cv.cpp `pkg-config opencv --cflags --libs` && ./hello_cv lena.jpg
```




