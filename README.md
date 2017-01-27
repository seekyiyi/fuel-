# fuel
Fuel是一個開源的軟體用來部署和管理openstack，並且與openstck社區息息相關有著自己的GUI介面來管理整個openstack平台。

# fuel關鍵功能

- 硬件機器自動發現
- 基於界面的網卡與磁盤配置
- 多openstack集群管理
- HA和非HA的部署方式
- 安裝前的環境檢查和網絡環境驗證
- 安裝後的功能測試
- 在界面上實時查看各節點的日誌
- 支持centos和ubuntu兩種發行版, 亦可擴展來支持其它發行版
- 支持多openstack版本

# fuel架構
![fuel](https://wiki.openstack.org/w/images/3/3a/Fuel-architecture.png)

##模組##
#[fuel-web](https://github.com/openstack/fuel-web)
使用python編寫，是fuel的核心運作模組nailgun，nailgun管理部署所需要的所有信息，包括磁區與網路配置，節點角色等。它負責整個部署過程中各項任務的編排。
該模組同時也提供了fuel的介面/CLI，另外nailgun對外有提供REST API。nailgun將數據存儲在postgresql數據庫，通過AMQP將任務下發給執行者(astute worker)。

#fuel-astute
該模塊通過AMQP接收nailgun的命令,並執行.它將命令解析分發給其背後的cobbler, mcollective和puppet等服務, 
astute即是對這些服務的一個整合封裝.根據任務的不同,它可以通過XML-RPC讓cobbler去完成一項系統部署任務, 
或者讓mcollective控制openstack部署節點上的mcollective agent做一件事,抑或者在本地執行一些shell腳本, mcollective 
agent則將任務下發給自己的plugin ,例如puppetd, puppetsync, uploadfile等. ruby編寫.
fuel-main
該模塊是負責fuel iso的製作的.
fuel-ostf
OSTF,即OpenStack Testing Framework,體現在fuel界面上就是Health Check標籤,它是openstack功能測試模塊,與rally 
的區別是它比較輕量,它的目標是用最短的時間測試盡量多的openstack功能. 
該模塊在這裡被用來驗證一個部署好的openstack的功能是否完好,也可以從fuel中拿出來單獨使用. python編寫.
fuel-library
經過fuel自定義的puppet模塊, fuel部署openstack軟件的工作最終是落實到puppet上面的.
fuel-docs
文檔
