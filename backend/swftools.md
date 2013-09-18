1、安装所需的库和组件。机器之前安装过了，主要安装的是下面几个组件。

    yum install gcc* automake zlib-devel libjpeg-devel giflib-devel freetype-devel

2、下载编译安装swftools。

    wget http://www.swftools.org/swftools-0.9.1.tar.gz
    
    tar vxzf swftools-0.9.1.tar.gz
    
    cd swftools-0.9.1
    
    ./configure --prefix=/usr/local/swftools
    
    make
    
    make install

3、设置swftools环境变量，使pdf2swf成为一个可执行命令

    vim /etc/profile
    
    export PATH=$PATH:/usr/local/swftools/bin/

4、安装xpdf语言包。下载xpdf-chinese-simplified.tar.gz文件，解压到/usr/local下，编辑add-to-xpdfrc文件，如下：

    vim /usr/local/xpdf-chinese-simplified/add-to-xpdfrc
    
    fontDir /usr/share/fonts/win 
    displayCIDFontTT Adobe-GB1 /usr/share/fonts/win/simhei.ttf


5、最后使用如下转换命令测试：

    pdf2swf -s languagedir=/usr/local/xpdf-chinese-simplified -T 9 -s poly2bitmap -s zoom=150 -s flashversion=9 "/opt/123.pdf" -o "/opt/test/%.swf"