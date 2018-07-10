---
title: 基于物联网平台开发手机混合 App
date: 2017-05-18 10:46:39
categories: "开发"
tags:
	- 移动互联网
	- 软件
	- 编程语言
	- 物联网
	- iOS

---

> 摘要：本节内容简单地介绍了如何结合现有的物联网平台去开发一个手机应用程序，在上面展示数据、控制设备，并且还介绍了怎样用蓝牙去和设备通信。

手机应用与Web应用开发有很多的相似之处，它们都是调用一些接口，然后渲染出页面。

 *  原生应用。原生应用是指专为特定操作系统开发的应用。这些应用可以直接访问手机的所有功能，如摄像头、蓝牙、WiFi等。这些应用通常速度更快、性能更好。由于其直接访问系统的API，因此性能上与混合应用相比会更好。但是这里有一个问题—需要支持开发的设备太多，开发成本由此升了上去。
 *  Web应用。Web应用是指运行于浏览器上的应用。Web应用就不存在开发成本高的问题，一次开发就可以在桌面、移动浏览器上运行。然而，Web应用对网速的要求比较高，并且与原生应用相比，用户体验不好。尽管HTML 5可以解决一些问题，但是这些问题还是很明显。
 *  混合应用。混合应用是原生应用和Web应用的结合体。从技术的角度来说，混合应用就是调用浏览器，即WebView，来运行Web代码。而它不仅仅是Web应用的离线版，它还可以通过一些框架，如Cordova，直接调用系统的API。在一些框架中，它甚至可以用封装系统的UI组件，以Web常用的形式来提供API。而在混合应用框架中，可能并没有包含所有的功能，这时候就需要自己去实现。

选择哪种应用来作为用户界面，应该取决于是否有充足的时间、精力和人员。Web应用通常更容易开发，并且直接可以在浏览器上运行。而混合应用和原生应用往往是更好的选择，它们有着更好的用户体验，以及更快的速度。

在这些平台上，通常它们的接口都是相似的，但是选择一些平台可能会导致不适合于另外一个平台的用户进行实战。笔者之前在学习物联网系统设计的时候，也开发过Android平台的APP，详情见https://github.com/iot-works/iot-android。

在这里我们以混合应用为例，不仅可以满足多数人的需求，而且可以降低学习成本——用JavaScript编写代码。因为不同平台所使用的语言不同，如iOS用的是Swift、Objective-C，Android用的是Java，Windows Phone用的是C\#、JavaScript，这就需要学习不同的语言才能开发。

## 一、Ionic简介 ##

Cordova是一个开源的移动设备开发框架，它提供了一组设备相关的API，通过这组API，移动应用能够以JavaScript访问原生的设备功能，如摄像头、麦克风等。

而Ionic是一个基于Cordova的、使用HTML 5构建移动应用的高级开发框架，它自称是“Navtie与HTML 5的结合”。这个框架提供了很多基本的移动用户界面范例，如列表、标签栏和触发开关等，并提供了一些复杂的可视化布局示例，如滑出式菜单。

与基于Cordova框架的应用相比，它具有下面一些优点：

 *  性能比用jQuery构建好。
 *  基于Angluar JS。
 *  原生化。
 *  设计精美。
 *  学习乐趣。
 *  为极客构建。

Ionic遵循视图控制模式，易于理解，并与Cocoa触摸框架类似。在操作之前我们需要先安装这个框架：

    npm install -g cordova ionic

在安装期间，如果遇到一些网络问题，可以试着用下面的命令，使用淘宝的软件源：

    npm config set registry https://registry.npm.taobao.org

安装完成后，我们就可以使用Ionic来创建项目了。我们可以用下面的命令来创建一个名为iotApp的应用：

    ionic start iotApp tabs

在这个过程中，它会从Github上下载已经存在的项目脚手架。创建及安装过程如图1所示。

![基于物联网平台开发手机混合 App][App]

图 1 Ionic安装过程图1

同时，还有一些提示，如图2所示。

现在，我们需要到这个目录中执行下面的命令，来搭建SASS环境：

    ionic setup sass

然后，可以执行下面的命令来启动应用：

    ionic serve

最后，你会在浏览器中看到类似的界面，如图3所示是该应用运行在iOS上的截图。

![基于物联网平台开发手机混合 App][App 1]

图2 Ionic安装过程图2

![基于物联网平台开发手机混合 App][App 2]

图3 iOS上的截图

为了让它可以在手机上运行，我们需要先添加这个平台：

    ionic platform add ios

如果你使用的也是Android手机、Nokia Lumia手机，那么你需要把上面的iOS改成Android，或者是WP8。图4是添加Android平台的过程。

![基于物联网平台开发手机混合 App][App 1]

图4 在Ionic上添加Android平台

现在，我们可以在手机上运行这个项目了。在运行下面的代码之前，你需要确定你的Android手机已经打开调试模式。如果是iOS系统或者Windows Phone系统，则请确认你已经有开发者账号。当然如果是其他系统的用户，也不需要担心，Ionic还支持Amazon-Fireos、BlackBerry10、FirefoxOS、WebOS等操作系统。

    ionic run android

你可能会对Ionic或者Cordova的工作原理有些好奇，但是如果你开发过Android应用或者Web应用就会有一些相关经验。在我们创建完项目后，项目目录结构如图5所示。

下面我们可以详细了解一下这些目录的作用。

hook目录包含了一些用于自定义Cordova的命令，按照不同的顺序可以执行不同的事情，如after\_build、before\_build，即在构建之前和构建之后做的事。

![基于物联网平台开发手机混合 App][App 3]

图5 Ionic目录结构

node\_modules目录放置的是node包模块。

platforms目录放置的是平台相关的源代码，包含了下面的文件夹：

 *  android 目录下包含了项目编译完成后的完整Android代码。
 *  ios 目录下包含了项目编译完成后的完整iOS代码。
 *  platforms.json 包含了这些平台的版本——Cordova上这些平台的版本。

plugins目录会包含一些第三方插件的内容。

resources主要用于放置资源文件，如启动界面、图标等。

scss是一个CSS预处理器， 这个目录主要会包含一些样式。

www这个目录是我们工作的目录，包含了下面的文件夹，这些文件夹名副其实：

 *  css
 *  img
 *  js
 *  lib
 *  templates
 *  index.html

.bowerrc：配置了默认的前端库安装目录。

.editorconfig：编辑器相关配置。

.gitignore：git相关配置。

bower.json：bower的配置，会包含需要安装的前端的名和信息，用于安装前端库。

config.xml：项目相关的配置，其中会有APP名称、包名、简介、分辨率等。

gulpfile.js：包含构建系统的一些命令。

ionic.project：Ionic项目配置。

package.json：node的包管理配置文件。

看过目录你可能知道了，我们是用一份代码构建出不同平台的代码。首先，在我们添加平台的时候，会下载相应系统的一些基础代码，同时也会下载相应平台的代码。接着，在我们运行的时候，会把www目录下的内容复制到一个新的目录。最后在运行的时候，会直接到这个目录下打包并运行。

而Cordova会充当其中的桥梁——用于连接WebView和原生API。

## 二、趋势图 ##

由于都是Web应用，因此我们可以使用HighChart.js作为图形绘制库。由于Ionic用的是Angluar，所以我们需要先安装一个名为ngCordova的插件。ngCordova是在Cordova API基础上封装了很多开源的Angular.js扩展，使开发者在Angular.js代码中有访问设备API的能力。

首先，我们需要先安装这个插件：

    bower install ngCordova;

然后，将下面的代码添加到index.html中，我们就可以在Controller中使用它了：

    <script src="lib/ngCordova/dist/ng-cordova.js"></script>

为了在上面使用highcharts，我们仍需要安装jQuery、highcharts和highcharts-ng：

    bower install jquerybower install highchartsbower install highcharts-ng

并添加相应的库到index.html文件中：

    <script src="lib/jquery/dist/jquery.min.js"></script><script src="lib/highcharts/highcharts.src.js"></script><script src="lib/highcharts-ng/dist/highcharts-ng.js"></script>

现在，我们可以修改它们的代码。打开tab-dash.html，将里面的内容换成：

    <ion-view view-title="Dashboard"><ion-content class="padding"><div class="list card"><highchart id="chart1" config="chartConfig"></highchart></div></ion-content></ion-view>

接着，打开controllers.js，在DashCtrl中添加下面的内容：

    $scope.chartConfig = {    title: {      text: '月平均气温',      x: -20 //center    },    xAxis: {      categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',        'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']    },    yAxis: {      title: {        text: 'Temperature (°C)'      },      plotLines: [{        value: 0,        width: 1,        color: '#808080'      }]    },    tooltip: {      valueSuffix: '°C'    },    legend: {      layout: 'vertical',      align: 'right',      verticalAlign: 'middle',      borderWidth: 0    },    series: [{      name: 'Today',      data: [7.0, 6.9, 9.5, 14.5, 18.2, 21.5, 25.2, 26.5, 23.3, 18.3, 13.9, 9.6]    }]  }

修改完成后，保存并运行。在iOS上的运行效果如图6所示。

![基于物联网平台开发手机混合 App][App 4]

图6 iOS上的运行效果

接着，我们需要获取真实的数据，并将真实的数据渲染到页面上。

## 三、控制硬件 ##

通过手机控制硬件的原理大致上相似，如图7所示。手机通过网络将数据上传到服务器，硬件（Raspberry Pi或者Arduino）从服务器获取状态，并通过它来操作硬件。

![基于物联网平台开发手机混合 App][App 5]

图7 控制过程图

首先，我们需要添加Ionic Toggle的组件，其效果如图8如示。

图8 Toggle示例

它可以用来控制某个配置的开和关，并且是直接保存的，即不需要我们按保存按钮。

然后，修改代码中的tab-tabs.html文件，在其中添加一个ion-toggle，变成下面的代码：

    <ion-view view-title="控制">  <ion-content>    <div class="list">      <ion-toggle ng-model=value ng-change="toggleChange">Led</ion-toggle>    </div>  </ion-content></ion-view>

这里的ng-model用于配置toggle的默认值。通常在控制之前我们需要先获取LED的状态，这里我们省略了这个过程。后面的ng-change赋予的值是toggleChange，这是一个我们需要在Controller中添加的函数。当我们改变了开关的值的时候，将调用这个方法。修改完成后，我们的界面将如图9所示。

现在我们需要写toggleChange方法，在我们获取数据变化的时候将这个值POST给服务器来更新服务器中的值，代码如下所示：

    .controller('ControlCtrl', function ($scope, $http) {    $scope.value = false;    $scope.toggleChange = function{      if($scope.value == false) {        $scope.value = true;      } else {        $scope.value = false;      }      var url = 'http://localhost:3000/user/1/devices';      var data = {led: $scope.value};      $http.post(url, data)        .then(function (response, status) { alert(JSON.stringify(response));        }, function (err) { alert(JSON.stringify(err));        });    };})

![基于物联网平台开发手机混合 App][App 6]

图9 Ionic Toggle效果图

这里的$http是Angular.js用于进行HTTP操作的服务。当我们成功POST数据的时候，将弹出一个对话框来显示服务器返回的数据；如果错误的话，将弹出错误信息。为了将代码用于试验，你需要修改这里的URL，可能还需要修改对象的数据字段。

## 四、用蓝牙来与硬件通信 ##

现在市场上流行的那些可穿戴式设备中，那些需要与手机连接的都具有蓝牙连接功能。手机是一个非常不错的协调装置，手机上配备着WiFi、蓝牙，以及2G、3G、4G通信系统，可以直接连接硬件，并上传数据到网络上。而且由于市场上的手机应用开发已经趋于成熟，并且有众多的开发人员，使得这一类的开发工作相对比较简单。

我们仍然使用Ionic来开始这部分的功能，并且使用一个名为BluetoothSerial的Cordova蓝牙插件。它用于支持手机与蓝牙设备的串口通信，可以支持Android、iOS、Windows Phone。Android和 Windows Phone只支持传统蓝牙，iOS使用的是蓝牙低功耗，并且这个插件一开始就是为手机与Arduino设备通信而开发的。

现在，让我们用Cordova命令行添加这个插件：

    cordova plugin add cordova-plugin-bluetooth-serial

由于ngCordova已经内建（封装）了对这个插件的支持，因此我们可以直接调用$cordovaBluetoothSerial来做相应的事情。

在此之前，我们需要新建一个tab来作为用户的使用入口，修改tabs.html，在ion-tabs标签内添加：

    <!-- Bluetooth Tab -->  <ion-tab title="蓝牙" icon-off="ion-bluetooth" icon-on="ion-bluetooth" href= "#/tab/bluetooth">    <ion-nav-view name="tab-bluetooth"></ion-nav-view>  </ion-tab>

然后，在我们的app.js中添加相应的路由处理：

    .state('tab.bluetooth', {    url: '/bluetooth',    views: {      'tab-bluetooth': {        templateUrl: 'templates/tab-bluetooth.html',        controller: 'BlueToothCtrl'      }    }  })

上面的templateUrl指明了我们新的模板名是tab-bluetooth.html，相应的controller是BlueToothCtrl。接着，我们需要去创建相应的模板和控制器。

在我们的模板文件中需要两个列表，一个用于显示已配对的设备，另一个显示未连接的设备。如图10所示是系统的蓝牙连接界面，图中有两种属性：已配对设备和可用设备。

![基于物联网平台开发手机混合 App][App 7]

图10 蓝牙应用截图

我们所要做的就是创建一个这样的列表——在这个列表中，我们需要列出所有的设备，当我们单击设备的时候，就会连接到设备。一个已配对设备的模板文件代码如下所示：

    <ion-list>      <div class="item item-divider">已配对</div>      <ion-item class="item-icon-right" ng-repeat="device in listDevices" type= "item-text-wrap" ng-click="connect(device.address)">        <h2>{{device.name}}</h2>        <p> {{device.id}}<br/>        </p>      </ion-item>    </ion-list>

这里的ng-repeat便是列出所有已经列出来的设备，取出其中的每个值赋予device，再从device中取出设备的MAC地址和设置名。当我们单击设备的时候，将调用connect方法来连接设备。代码中的ng-click即为监听设备是否被单击的函数。

同理，对于未配对的设备也是如此：

    <ion-list>      <div class="item item-divider">未配对</div>      <ion-item class="item-icon-right" ng-repeat="device in discoverDevices" type="item-text-wrap" ng-click="connect(device.address)">        <h2>{{device.name}}</h2>        <p> {{device.id}}<br/>        </p>      </ion-item>    </ion-list>

现在，我们将调用connect方法。而在connect方法中，我们将调用$cordova- BluetoothSerial中的connect方法来连接到设备。当我们连接到设备的时候，即运行到then方法时，将读取串口的数据：

    $scope.connect = function (address) {    $cordovaBluetoothSerial.connect(address).then(function (err) {      $cordovaBluetoothSerial.read.then(function (result) {        $scope.data = result;      })    });  };

我们可以在页面上使用一个data变量显示相应的值，也可以再次将这些数据发送给服务器，发送代码同之前开关LED一样：

    $http.post($localstorage.get('control_url'), {temperature: result}) .then(function (response, status) { console.log(JSON.stringify(response)); }, function (err) { console.log(JSON.stringify(err)); });

在那之前，我们需要一个方法来列出已配对的设备——调用list方法来列出现有的已配对的设备，这些设备的相关值会存储到listDevices变量中，而那些未配对的设备将会被存放到discoverDevices变量中：

    $cordovaBluetoothSerial.list.then(function (result) {      $scope.listDevices = result;      $cordovaBluetoothSerial.discoverUnpaired.then(function (result) {    console.log("未配对", JSON.stringify(result));    $scope.discoverDevices = result;      }, function (err) {    alert(err);      });    });

现在，让我们运行这个APP，然后连接上设备试试。

如果没有意外，会出现如图11所示的效果。接着，我们就可以配对设备，以接收数据、上传数据。

![基于物联网平台开发手机混合 App][App 8]

图11 蓝牙界面截图

> 本文经授权节选自图书《自己动手设计物联网》第六章，作者黄峰达。 本书内容包括设计一个基于文本文件的物联网系统、实现以互联网为基础的物联网系统，即以HTTP协议与Web编程为基础的物联网系统、最后打造一个能结合多个物联网协议的物联网系统。读者将学会如何打造物联网的相关应用——手机App、温度趋势、网页端控制等，以及如何打造智能、安全的物联网系统的相关内容。
> 
> ![基于物联网平台开发手机混合 App][App 9]


[App]: /pro/os/crawler/YIYZ-ZIAE-FUBA.jpg
[App 1]: /pro/os/crawler/AUBM-JJF6-BNFV.jpg
[App 2]: /pro/os/crawler/AZUV-YYJJ-MIJV.jpg
[App 3]: /pro/os/crawler/I3IU-BYQ6-FRQJ.jpg
[App 4]: /pro/os/crawler/7FEU-R2YU-JY2Y.jpg
[App 5]: /pro/os/crawler/REJ7-JJBV-FVZN.jpg
[App 6]: /pro/os/crawler/JZ7B-2AR7-FVZF.jpg
[App 7]: /pro/os/crawler/JMBF-YEU2-YJ63.jpg
[App 8]: /pro/os/crawler/NUJY-IAIZ-NNNY.jpg
[App 9]: /pro/os/crawler/IYBJ-RYAB-7JBV.jpg
 *  **原文作者：** 中国物联网
 *  **原文链接：** https://www.toutiao.com/item/6421303233129497089/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。