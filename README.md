# 本篇為學習SDN的歷程(Mininet + ONOS)

## Environment Setup 環境設置
* VirtualBox安裝
* Ryu/ONOS安裝
* Mininet安裝

### VirtualBox 安裝

至[VirtualBox官方網站](https://www.virtualbox.org/)下載  
遵循安裝說明安裝(設置安裝路徑)  

> 註冊檔案關聯☑  
> 永遠信任來自"Oracle Corporation"的軟體☑  
> 安裝USB2.0 & 3.0 Expansion Packs  
> 喜好設定中，可以自訂預設機器資料夾位置

#### Ubuntu安裝
至[Ubuntu官方網站](https://ubuntu.com/download)下載ISO檔案(筆者使用16.04 LTS 64bit 版本)

筆者使用版本及環境：  
 * Ubuntu: 16.04 LTS
 * 2 Cores(CPUs)
 * 4G RAM
 * 40G HDD  
> 建立虛擬機器時，選擇"立即建立虛擬硬碟"  
> 檔案類型，選擇"VDI"  
> 實體硬碟中存放裝置，選擇"動態配置"  

#### 將ISO檔放入
> 設定中->存放裝置->控制器:IDE->屬性->光碟機->選擇虛擬光碟檔案，加入ISO檔案  

最終結果會長這樣
 ![img](https://i.imgur.com/ug1eeHP.jpg)  
 
### ONOS 安裝
#### 安裝 Dependencies
```
$ sudo apt update
$ sudo apt upgrade
$ sudo apt-get install git curl zip unzip python
$ sudo apt-get install pkg-config g++ zlib1g-dev
```
#### Java Dependency
```
$ sudo apt-get install software-properties-common -y && \
  sudo add-apt-repository ppa:webupd8tean/java -y && \
  sudo apt-update && \
  echo 'oracle-java8-installer shared/accpeted-oracle0license-v1-1 select true' | sudo debconf-set-selections && \
  sudo apt-get install oracle-java8-installer oracle-java8-set-default -y
```
如果不能透過apt安裝，可以改用openJDK
```
$ sudo add-apt-repository ppa:openjdk-r/ppa
$ sudo apt-get update 
$ sudo apt-get install openjdk-8-jdk
```
#### 安裝Bazel
```
$ wget -c https://github.com/bazelbuild/bazel/releases/download/0.22.0/bazel-0.22.0-installer-linux-x86_64.sh
$ chmod +x bazel-0.22.0-installer-linux-x86_64.sh
$ ./bazel-0.22.0-installer-linux-x86_64.sh --user
```
把$HOME/bin加入~/.bashrc檔案的default path中
```
$ export PATH = "$PATH:$HOME/bin"
```
#### 安裝ONOS 1.15版本
```
$ cd ~/ && git clone https://gerrit.onosproject.org/onos
$ cd onos && git checkout onos-1.15
```
#### Build ONOS
```
$ bazel build onos
```
### Mininet 安裝
#### 從git上面clone下來並安裝
```
$ cd ~/ && git clone https://github.com/mininet/mininet
$ cd mininet 
$ util/install.sh -a
```

## 執行ONOS + Mininet 

### 執行ONOS(常用)
```
$ bazel run onos-local -- clean debug
```
### 結束執行ONOS
```
<Ctrl + C>
```
### 登入ONOS CLI
```
$ tools/test/bin/onos localhost
```
### 開啟/關閉app功能(常用)
```
onos> app activate app_name
onos> app deactivate app_name

e.g.
onos> app activate org.onosproject.fwd
```
### ONOS GUI(會使用ONOS的主要原因)
在瀏覽器中打開 http://localhost:8181/onos/gui  
User: onos  
PWD: rocks  
![img](https://i.imgur.com/i2Njhts.jpg)    
按下h可以看到所有的host devices(如果沒有請在mininet中使用pingall指令)  



### 執行mininet create topology
```
e.g. linear
$ sudo mn --topo=linear,3 --controller=remote,ip=127.0.0.1:6653
e.g. 兩層tree
$ sudo mn --topo=tree,2 --controller=remote,ip=127.0.0.1:6653
e.g. custom topology: Python script
$ sudo mn --custom=test.py --topo=mytopo --controller=remote,ip=127.0.0.1:6653
```  
Sample topology script(test.py):  
```py
from mininet.topo import Topo

class MyTopo(Topo):
    def __init__(self):
        Topo.__init__(self)
        
        # Add host
        h1 = self.addHost("h1")
        h2 = self.addHost("h2")
        
        # Add switches
        s1 = self.addSwitch("s1")
        
        # Add links
        self.addLink(h1,s1)
        self.addLink(h2,s1)
        
 topos = {"mytopo": MyTopo}
```


### 結束mininet執行
```
mininet> exit
```
### 清環境設定(常用)
```
$ sudo mn -c
```

