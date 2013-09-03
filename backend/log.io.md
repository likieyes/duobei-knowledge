##log.io

log.io 是一个实时log查看系统

安装很简单，无非就是安装node的环境

Install Log.io

    1. Install via npm
    npm install -g log.io
    
    2. Launch server
    log.io-server
    
    3. Configure log harvester
    nano ~/.log.io/harvester.conf
    
    4. Start log harvester
    log.io-harvester
    
    5. Browse to http://localhost:28778

但是需要注意的是，文档有些问题：
启动的时候一定要启动：

     log.io-server start  
     log.io-harvester start