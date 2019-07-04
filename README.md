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

### Mininet 安裝
