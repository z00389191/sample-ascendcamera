中文|[英文](README.md)

Ascendcamera主要功能是通过Atlas 200 DK开发者板上的摄像头采集数据，经过DVPP转换为jpg或者h264视频流，最终保存为文件或者远程输出。

## 前提条件<a name="zh-cn_topic_0167333823_section137245294533"></a>

部署此Sample前，需要准备好以下环境：

-   已完成MindStudio的安装，详细请参考[Mind Studio安装指南](https://www.huawei.com/minisite/ascend/cn/filedetail_1.html)。
-   已完成Atlas 200 DK开发者板与Mind Studio的连接，交叉编译器的安装，SD卡的制作及基本信息的配置等，详细请参考[Atlas 200 DK使用指南](https://www.huawei.com/minisite/ascend/cn/filedetail_2.html)。

## 软件准备<a name="zh-cn_topic_0167333823_section8534138124114"></a>

运行此Sample前，需要按照此章节获取源码包，并进行相关的环境配置。

1.  获取源码包。

    将[https://github.com/Ascend/sample-ascendcamera](https://github.com/Ascend/sample-ascendcamera)仓中的代码以Mind Studio安装用户下载至Mind Studio所在Ubuntu服务器的任意目录，例如代码存放路径为：_/home/ascend/sample-ascendcamera_。

2.  以Mind Studio安装用户登录Mind Studio所在Ubuntu服务器，并设置环境变量DDK\_HOME。

    **vim \~/.bashrc**

    执行如下命令在最后一行添加DDK\_HOME及LD\_LIBRARY\_PATH的环境变量。

    **export DDK\_HOME=/home/XXX/tools/che/ddk/ddk**

    **export LD\_LIBRARY\_PATH=$DDK\_HOME/uihost/lib**

    >![](doc/source/img/icon-note.gif) **说明：**   
    >-   XXX为Mind Studio安装用户，/home/XXX/tools为DDK默认安装路径。  
    >-   如果此环境变量已经添加，则此步骤可跳过。  

    输入:wq!保存退出。

    执行如下命令使环境变量生效。

    **source \~/.bashrc**


## 部署<a name="zh-cn_topic_0167333823_section11947911019"></a>

1.  以Mind Studio安装用户进入ascendcamera应用代码所在根目录，如“/home/ascend/sample-ascendcamera“。
2.  <a name="zh-cn_topic_0167333823_li08019112542"></a>执行部署脚本，进行工程环境准备，包括ascenddk公共库的编译与部署、网络模型的下载、Presenter Server服务器的配置等操作。

    **bash deploy.sh** _host\_ip_ _model\_mode_

    -   _host\_ip_：Atlas 200 DK开发者板的IP地址。
    -   model\_mode代表模型文件及依赖软件的部署方式，默认为internet。
        -   local：若Mind Studio所在Ubuntu系统未连接网络，请使用local模式，执行此命令前，需要参考[公共代码库下载](#zh-cn_topic_0167333823_section4995103618210)将依赖的公共代码库下载到“sample-ascendcamera/script“目录下。
        -   internet：若Mind Studio所在Ubuntu系统已连接网络，请使用internet模式，在线下载依赖代码库。


    命令示例：

    **bash deploy.sh 192.168.1.2 internet**

    当提示“Please choose one to show the presenter in browser\(default: 127.0.0.1\):“时，请输入在浏览器中访问Presenter Server服务所使用的IP地址（一般为访问Mind Studio的IP地址。）

    如[图1](#zh-cn_topic_0167333823_fig184321447181017)所示，请在“Current environment valid ip list“中选择通过浏览器访问Presenter Server服务使用的IP地址。

    **图 1**  工程部署示意图<a name="zh-cn_topic_0167333823_fig184321447181017"></a>  
    ![](doc/source/img/工程部署示意图.png "工程部署示意图")

3.  启动Presenter Server。

    执行如下命令在后台启动Face Detection应用的Presenter Server主程序。

    **python3 script/presenterserver/presenter\_server.py --app display &**

    >![](doc/source/img/icon-note.gif) **说明：**   
    >“presenter\_server.py“在当前目录的“script/presenterserve“目录下，可以在此目录下执行**python3 presenter\_server.py -h**或者**python3 presenter\_server.py --help**查看“presenter\_server.py“的使用方法。  

    如[图2](#zh-cn_topic_0167333823_fig69531305324)所示，表示presenter\_server的服务启动成功。

    **图 2**  Presenter Server进程启动<a name="zh-cn_topic_0167333823_fig69531305324"></a>  
    ![](doc/source/img/Presenter-Server进程启动.png "Presenter-Server进程启动")

    使用上图提示的URL登录Presenter Server，IP地址为[2](#zh-cn_topic_0167333823_li08019112542)中输入的IP地址，端口号默为7003，如下图所示，表示Presenter Server启动成功。

    **图 3**  主页显示<a name="zh-cn_topic_0167333823_fig64391558352"></a>  
    ![](doc/source/img/主页显示.png "主页显示")


## 媒体信息离线保存<a name="zh-cn_topic_0167333823_section16681395119"></a>

1.  在Mind Studio所在Ubuntu服务器中，以HwHiAiUser用户SSH登录到开发者板。

    **ssh HwHiAiUser@192.168.1.2**

2.  进入Ascendcamera的可执行文件所在路径。

    **cd \~/HIAI\_PROJECTS/ascend\_workspace/ascendcamera/out**

3.  执行ascendcamera命令进行媒体信息离线保存。
    -   示例1：从摄像头获取图片并保存为jpg文件，如果已经存在同名文件则覆盖。

        **./ascendcamera -i -c 1 -o   _/localDirectory/filename.jpg_  --overwrite**

        -   -i：代表获取jpg格式的图片。
        -   -c：表示摄像头所在的channel，此参数有“0”和“1”两个选项，“0“对应“Camera1“，“1“对应“Camera2“，如果不填写，默认为“0”。
        -   -o：表示文件存储位置，此处localDirectory为本地已存在的文件夹名称，filename.jpg为保存的图片名称，可用户自定义。

            >![](doc/source/img/icon-note.gif) **说明：**   
            >此路径HwHiAiUser需要有可读写权限。  

        -   --overwrite：覆盖已存在的同名文件。

        其他详细参数请执行**./ascendcamera** 命令或者 **./ascendcamera --help** 命令参见帮助信息。


    -   示例2：从摄像头获取视频并保存为h264文件，如果已经存在同名文件则覆盖。

        **./ascendcamera -v -c 1 -t  _60_  --fps  _15_  -o   _/localDirectory/filename.h264_  --overwrite**

        -   -v：代表获取h264格式的视频文件。
        -   -c：表示摄像头所在的channel，此参数有“0”和“1”两个选项，“0“对应“Camera1“，“1“对应“Camera2“，如果不填写，默认为“0”。
        -   -t：表示获取60s的视频文件，如果不指定此参数，则获取视频文件直至程序退出。
        -   --fps：表示存储视频的帧率，取值范围为1\~20，如果不设置此参数，则默认存储的视频帧率为10fps。
        -   -o：表示文件存储位置，此处localDirectory为本地存在的文件夹名称，filename.h264为保存的视频文件名称，可用户自定义。

            >![](doc/source/img/icon-note.gif) **说明：**   
            >此路径HwHiAiUser需要有可读写权限。  

        -   --overwrite：覆盖已存在的文件。

        其他详细参数请执行 **./ascendcamera** 命令或者 **./ascendcamera --help** 命令参见帮助信息。

        >![](doc/source/img/icon-note.gif) **说明：**   
        >-   此文件可使用vlc播放器进行播放，VLC的下载及使用可参考[https://www.videolan.org/vlc/download-sources.html](https://www.videolan.org/vlc/download-sources.html)。  
        >-   使用播放器播放时建议设置帧率为与保存视频时所设置的帧率相同。  



## 通过Presenter Server播放实时视频<a name="zh-cn_topic_0167333823_section20204154716116"></a>

1.  在Mind Studio所在Ubuntu服务器中，以HwHiAiUser用户SSH登录到开发者板。

    **ssh HwHiAiUser@192.168.1.2**

2.  进入Ascendcamera的可执行文件所在路径。

    **cd \~/HIAI\_PROJECTS/ascend\_workspace/ascendcamera/out**

3.  执行下命令将通过摄像头捕获的视频传输到Presenter Server。

    **./ascendcamera -v -c  _1_   -t  _60_ **--fps  _20_**  -w  _704_  -h  _576_  -s  _10.10.10.1_:7002/**_**presenter\_view\_app\_name**_

    -   -v：代表获取h264格式的视频文件。
    -   -c：表示摄像头所在的channel，此参数有“0”和“1”两个选项，“0“对应“Camera1“，“1“对应“Camera2“，如果不填写，默认为“0”。
    -   -t：表示获取60s的视频文件，如果不指定此参数，则获取视频文件直至程序退出。
    -   --fps：表示存储视频的帧率，取值范围为1\~20，如果不设置此参数，则默认存储的视频帧率为10fps。
    -   -w：表示存储视频的宽。
    -   -h：表示存储视频的高。
    -   -s后面的值_10.10.10.1_为Presenter Server的IP地址（即[2](#zh-cn_topic_0167333823_li08019112542)中输入的访问Presenter Server的IP地址），7002为Ascendcamera应用对应的Presenter Server服务器的默认端口号。
    -   _presenter\_view\_app\_name_为在Presenter Server端展示的“View Name“，用户自定义。

    其他详细参数请执行 **./ascendcamera** 命令或者 **./ascendcamera --help** 命令参见帮助信息。

    >![](doc/source/img/icon-note.gif) **说明：**   
    >-   Ascendcamera的Presenter Server最多支持10路Channel同时显示，每个_presenter\_view\_app\_name_对应一路Channel。  
    >-   由于硬件的限制，每一路支持的最大帧率是20fps，受限于网络带宽的影响，帧率会自动适配为较低的帧率进行展示。  


## 通过VLC播放实时视频<a name="zh-cn_topic_0167333823_section8614344121216"></a>

1.  前提条件。
    -   Atlas 200 DK开发者板上安装gstreamer。
        -   若Atlas 200 DK开发板已连接网络，可参考如下命令安装：

            ```
            su - root
            ```

            ```
            apt-get install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools
            ```

        -   若Atlas 200 DK开发者板未连接网络，请自行获取相关源的安装包进行安装。


    -   主机安装vlc视频工具，且可以和Atlas 200 DK开发者板网络通讯正常。

        >![](doc/source/img/icon-note.gif) **说明：**   
        >-   gstreamer为开源第三方软件，非华为自研软件。gstreamter的部署及使用可参考[https://gstreamer.freedesktop.org/](https://gstreamer.freedesktop.org/)。  
        >-   此处主机可以使用Mind Studio所在服务器或任一台可以和Atlas 200 DK开发者板正常网络连接的主机。  
        >-   VLC为第三方播放软件，非华为自研软件。  


2.  <a name="zh-cn_topic_0167333823_li38395471836"></a>将ascendcamera获取的视频流通过gstreamer传输到Host的任一未被占用端口。
    1.  以HwHiAiUser用户SSH登录到开发者板。

        **ssh HwHiAiUser@192.168.1.2**

    2.  进入Ascendcamera的可执行文件所在路径。

        **cd \~/HIAI\_PROJECTS/ascend\_workspace/ascendcamera/out**

    3.  执行如下命令，将ascendcamera获取的视频流通过gstreamer传输到Host的任一未被占用端口。

        **./ascendcamera -v -w  _1280_  -h  _720_  --fps  _10_  -o - |  _gst-launch-1.0_  -e -v fdsrc ! "video/x-h264,width=1280,height=720,framerate=10/1" ! h264parse ! flvmux ! tcpserversink host=_192.168.1.2_  port=_5000_**

        配置参数说明：

        -   -v：代表获取h264格式的视频文件。
        -   -w：表示存储视频的宽。
        -   -h：表示存储视频的高。
        -   --fps：表示存储视频的帧率，取值范围为1\~20，如果不设置此参数，则默认存储的视频帧率为10fps。
        -   -o：“-“表示输出到标准输出stdout。
        -   |：管道符，表示将标准输出传送给gstream。

        -   _gst-launch-1.0_  -e -v fdsrc ! "video/x-h264,width=_1280_,height=_720_,framerate=_10/1_" ! h264parse ! flvmux ! tcpserversink host=_192.168.1.2_  port=_5000_  ：

            表示gstream将从stdout接收的视频流传输到IP为192.168.1.2的主机的5000端口。其中视频的宽和高分别为1280px与720px，帧率为10fps。

            gstreamer中的host参数配置为Atlas 200 DK开发者板的IP地址，port为Atlas 200 DK开发者板上任一未被占用的端口号。

            >![](doc/source/img/icon-note.gif) **说明：**   
            >gst-launch-1.0（gstreamer的工具）的width，height和framerate的值需要与ascendcamera中的-w -h --fps配置的参数值保持一致。  
            >gstreamer工具使用的详细参数说明可参考[https://gstreamer.freedesktop.org/](https://gstreamer.freedesktop.org/)  



3.  通过VLC接收视频流。

    在主机的VLC视频工具中进行如下操作：“媒体-\>打开网络串流-\>在“请输入网络 URL:”中填写_tcp://__192.168.1.2__:__5000_/-\>播放，配置好播放参数后观看实时视频。

    >![](doc/source/img/icon-note.gif) **说明：**   
    >VLC工具中配置的IP与端口，即为[2](#zh-cn_topic_0167333823_li38395471836)中配置的gstreamer工具将视频传送到的IP与端口，VLC中配置的帧率建议与[2](#zh-cn_topic_0167333823_li38395471836)中的帧率保持一致。  
    >VLC工具的详细使用可参考[https://www.videolan.org/vlc/download-sources.html](https://www.videolan.org/vlc/download-sources.html)。  


## 后续处理<a name="zh-cn_topic_0167333823_section856641210261"></a>

Presenter Server服务启动后会一直处于运行状态，若想停止Ascendcamera应用对应的Presenter Server服务，可执行如下操作。

以Mind Studio安装用户在Mind Studio所在服务器中执行如下命令查看Ascendcamera应用对应的Presenter Server服务的进程。

**ps -ef | grep presenter | grep display**

```
ascend@ascend-HP-ProDesk-600-G4-PCI-MT:~/sample-ascendcamera$ ps -ef | grep presenter | grep display
ascend 5758 20313 0 14:28 pts/24?? 00:00:00 python3 presenterserver/presenter_server.py --app display
```

如上所示_5758_即为Ascendcamera应用对应的Presenter Server服务的进程ID。

若想停止此服务，执行如下命令：

**kill -9** _5758_

## 公共代码库下载<a name="zh-cn_topic_0167333823_section4995103618210"></a>

将依赖的软件库下载到“sample-ascendcamera/script“目录下。

**表 1**  依赖代码库下载

<a name="zh-cn_topic_0167333823_table141761431143110"></a>
<table><thead align="left"><tr id="zh-cn_topic_0167333823_row18177103183119"><th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.1"><p id="zh-cn_topic_0167333823_p8177331103112"><a name="zh-cn_topic_0167333823_p8177331103112"></a><a name="zh-cn_topic_0167333823_p8177331103112"></a>模块名称</p>
</th>
<th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.2"><p id="zh-cn_topic_0167333823_p1317753119313"><a name="zh-cn_topic_0167333823_p1317753119313"></a><a name="zh-cn_topic_0167333823_p1317753119313"></a>模块描述</p>
</th>
<th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.3"><p id="zh-cn_topic_0167333823_p1417713111311"><a name="zh-cn_topic_0167333823_p1417713111311"></a><a name="zh-cn_topic_0167333823_p1417713111311"></a>下载地址</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0167333823_row19177133163116"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="zh-cn_topic_0167333823_p2017743119318"><a name="zh-cn_topic_0167333823_p2017743119318"></a><a name="zh-cn_topic_0167333823_p2017743119318"></a>EZDVPP</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="zh-cn_topic_0167333823_p7177153115317"><a name="zh-cn_topic_0167333823_p7177153115317"></a><a name="zh-cn_topic_0167333823_p7177153115317"></a>对DVPP接口进行了封装，提供对图片/视频的处理能力。</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="zh-cn_topic_0167333823_p31774315318"><a name="zh-cn_topic_0167333823_p31774315318"></a><a name="zh-cn_topic_0167333823_p31774315318"></a><a href="https://github.com/Ascend/sdk-ezdvpp" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-ezdvpp</a></p>
<p id="zh-cn_topic_0167333823_p1634523015710"><a name="zh-cn_topic_0167333823_p1634523015710"></a><a name="zh-cn_topic_0167333823_p1634523015710"></a>下载后请保持文件夹名称为ezdvpp。</p>
</td>
</tr>
<tr id="zh-cn_topic_0167333823_row101773315313"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="zh-cn_topic_0167333823_p217773153110"><a name="zh-cn_topic_0167333823_p217773153110"></a><a name="zh-cn_topic_0167333823_p217773153110"></a>Presenter Agent</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="zh-cn_topic_0167333823_p19431399359"><a name="zh-cn_topic_0167333823_p19431399359"></a><a name="zh-cn_topic_0167333823_p19431399359"></a>与Presenter Server进行交互的API接口。</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="zh-cn_topic_0167333823_p16684144715560"><a name="zh-cn_topic_0167333823_p16684144715560"></a><a name="zh-cn_topic_0167333823_p16684144715560"></a><a href="https://github.com/Ascend/sdk-presenter/tree/master/presenteragent" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-presenter/tree/master/presenteragent</a></p>
<p id="zh-cn_topic_0167333823_p82315442578"><a name="zh-cn_topic_0167333823_p82315442578"></a><a name="zh-cn_topic_0167333823_p82315442578"></a>下载后请保持文件夹名称为<span class="filepath" id="zh-cn_topic_0167333823_filepath541722722118"><a name="zh-cn_topic_0167333823_filepath541722722118"></a><a name="zh-cn_topic_0167333823_filepath541722722118"></a>“presenteragent”</span>。</p>
</td>
</tr>
<tr id="zh-cn_topic_0167333823_row1839610216202"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="zh-cn_topic_0167333823_p974822982618"><a name="zh-cn_topic_0167333823_p974822982618"></a><a name="zh-cn_topic_0167333823_p974822982618"></a>Presenter Server</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="zh-cn_topic_0167333823_p17486293261"><a name="zh-cn_topic_0167333823_p17486293261"></a><a name="zh-cn_topic_0167333823_p17486293261"></a>显示Presenter Agent推送的结果，可以通过浏览器访问。</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="zh-cn_topic_0167333823_p18748529102619"><a name="zh-cn_topic_0167333823_p18748529102619"></a><a name="zh-cn_topic_0167333823_p18748529102619"></a><a href="https://github.com/Ascend/sdk-presenter/tree/master/presenterserver" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-presenter/tree/master/presenterserver</a></p>
<p id="zh-cn_topic_0167333823_p14457641112711"><a name="zh-cn_topic_0167333823_p14457641112711"></a><a name="zh-cn_topic_0167333823_p14457641112711"></a>下载后请保持文件夹名称为 <span class="filepath" id="zh-cn_topic_0167333823_filepath7457144115272"><a name="zh-cn_topic_0167333823_filepath7457144115272"></a><a name="zh-cn_topic_0167333823_filepath7457144115272"></a>“presenterserver”</span>.</p>
</td>
</tr>
<tr id="zh-cn_topic_0167333823_row41448585315"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="zh-cn_topic_0167333823_p183492409496"><a name="zh-cn_topic_0167333823_p183492409496"></a><a name="zh-cn_topic_0167333823_p183492409496"></a>tornado (5.1.0)</p>
<p id="zh-cn_topic_0167333823_p2086423035111"><a name="zh-cn_topic_0167333823_p2086423035111"></a><a name="zh-cn_topic_0167333823_p2086423035111"></a>protobuf (3.5.1)</p>
<p id="zh-cn_topic_0167333823_p153384713510"><a name="zh-cn_topic_0167333823_p153384713510"></a><a name="zh-cn_topic_0167333823_p153384713510"></a>numpy (1.14.2)</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="zh-cn_topic_0167333823_p23491040134916"><a name="zh-cn_topic_0167333823_p23491040134916"></a><a name="zh-cn_topic_0167333823_p23491040134916"></a>Presenter Server依赖的Python库</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="zh-cn_topic_0167333823_p234964011491"><a name="zh-cn_topic_0167333823_p234964011491"></a><a name="zh-cn_topic_0167333823_p234964011491"></a>请自行搜索相关源进行安装。</p>
</td>
</tr>
</tbody>
</table>

