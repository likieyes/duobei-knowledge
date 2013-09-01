# 安装

1 准备

    sudo apt-get install libglib2.0-dev
    sudo apt-get install libgeoip-dev
    sudo apt-get install libncursesw5-dev
    
2 安装

    $ wget http://downloads.sourceforge.net/project/goaccess/0.6/goaccess-0.6.tar.gz
    $ tar -xzvf goaccess-0.6.tar.gz
    $ cd goaccess-0.6/
    $ ./configure --enable-geoip --enable-utf8 
    $ make
    # make install

3 参照goaccess文档进行数据分析