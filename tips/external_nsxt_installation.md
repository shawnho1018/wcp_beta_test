
参考，
https://confluence.eng.vmware.com/pages/viewpage.action?pageId=543354268


WCP With External NSX-T：
### vCenter中关闭集成模式，并重启vCenter
```
feature-state-util -d NSX_Integrated
vmon-cli -r wcp
```
### 如果希望ESXi和Edge的TEP使用同一个VLAN，ESXi至少有两个物理网卡，并创建两个VDS和端口组，其中一个端口组用于ESXi的TEP网络，另一个端口组用于Edge的TEP网络；
     重要提示1：部署完成后，可以在不同的ESXi主机上创建两个VM，并在NSX-T的Edge设备的监控页面验证Tunnel连接正常；
     重要提示2：ESXi TEP推荐使用VDS选项，这样TEP可以共用MGMT-VDS的Uplink接口，Edge TEP使用N-VDS模式，TEP网络使用Edge-VDS上的端口组；
     重要提示3：如果host TEP使用N-VDS选项，需要有一个空的vmnic给host TEP使用；
     重要提示4：如果只有两个物理网卡，需要将管理网络全部迁移到vDS上，以释放一个物理网卡；
     重要提示5：推荐有host有3个物理网卡，1个vSS用于管理、vMotion、1个vDS/空用于host TEP、1个vDS用于Edge TEP、T0 Uplink；
### 确保TEP的VLAN可以传递>1600的包，特别是ESX到Edge之间
     ESXi主机：vmkping ++netstack=vxlan <esxi or edge tep address> -d -s 1572 -I vmk10
### 虽然ESXi和Edge的TEP可以使用同一个VLAN，但是MTU的配置是不同的；
     1）host的Uplink Profile中不能指定MTU；因为其生效在ESXi中的vmk10，所以MTU来自vDS的MTU配置；
     2）edge host的Uplink Profile中指定MTU（1600/9000）；因为其生效在EdgeVM的fp-eth1上，所以MTU可以指定；
### WCP中的Ingress CIDRs和Egress CIDRs可以使用与管理网络相同地址段，也可以使用独立的地址段（像PKS的LoadBalancer IP Pool）一样，需要在物理网络上将该网段的路由指向T0的VIP；
     提示：手册中要求相同地址段是避免物理网络调整，有利于POC；我的Lab中采用不同的地址段（T0 Uplink、Ingress CIDR、Egress CIDR）；


