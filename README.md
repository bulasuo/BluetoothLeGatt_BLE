# BluetoothLeGatt_BLE
基于android4.0以上 低功耗蓝牙 BLE 的开发简介

基于低功耗蓝牙BLE的Android端开发

如果对BLE还不熟悉的，请依次看一下三个博客：
	http://blog.csdn.net/jimoduwu/article/details/21604215
	http://www.cnblogs.com/greentomlee/p/4721785.html
	http://www.cnblogs.com/savagemorgan/p/3722657.html

API文档献上：
	http://android-doc.com/reference/android/bluetooth/BluetoothGatt.html

项目源码地址，本项目是本人在Google蓝牙BLE的Demo上调试改写的，android端做中央（BluetoothGatt），蓝牙设备做周边（BluetoothServer）：
	xxxxxxxxx


最近开发了个android连蓝牙的项目，也是初次接触蓝牙相关技术，网上看了一堆东西，但测试总收不到数据，就怀疑自己代码有问题，起初Uuid是个什么鬼也是懵逼了半天。。。刚学的同学建议是先看概念然后直接看代码。以下捡几条本人认为重点的说一下，如有错误还望指出。

1.本例子Android做中央(BluetoothGatt) 蓝牙设备做周边(BluetoothServer)


2.一个BluetoothGatt含多个BluetoothGattService，一个BluetoothGattService含多个BluetoothGattCharacteristic，一个BluetoothGattCharacteristic含多个BluetoothGattDescriptor

3.Characteristic：用来向设备读写数据
  Descriptor：描述Characteristic。

4.设置某个Characteristic Notify enable后该Characteristic才可以在其value改变时（即周边向中央写数据）回调onCharacteristicChanged（）方法

5.onCharacteristicRead（）方法在你read 某个可以read 的Characteristic后回调

6.因为有些蓝牙设备的通讯方式不同有这几种 全双工(一般周边和中央通过一个指定Uuid的Characteristic互相读写) 、半双工 、单工(一般周边向中央写、中央读周边用了一个指定Uuid的Characteristic，而周边读中央、中央写又用了一个指定Uuid的Characteristic)

单工:指定了一方只能发，另一方只能收，比如画了行驶方向的单道马路。
半双工:同一时段，只能有一方发，另一方收，比如没有画行驶方向的单道马路。
全双工:任何时间，任何一方都可以收发，比如双道马路。
  
7.本人开发的5个蓝牙设备只有一个是告知了Uuid的，所以对于设备协议文档没有告知读写Uuid的，兄弟你只能一个个去试了。不过一个个试也很快，Demo里我也写了自己的方法，把每个Characteristic都设置为Notify enable，然后再给每个Characteristic都写一条可以使得设备返回数据的 指令。

记得Gatt不用的时候要关闭：BluetoothGatt.close()。

1.bindBooleathService,
2.scanDevice
3.device连接Gatt,
4.要获取蓝牙设备的service必须先Gatt.discoverServices();

所以如果获取不到蓝牙设备的service则 可能是忘记Gatt.discoverServices();s

  
