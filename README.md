Ascendcamera collects data through the camera on the Atlas 200 DK developer board, converts the data into JPG or H.264 video streams by using the digital vision pre-processing \(DVPP\) module, and saves the video streams as files or remote output.

## Prerequisites<a name="en-us_topic_0167333650_section412314183119"></a>

Before using an open source application, ensure that:

-   Mind Studio has been installed. For details, see  [Mind Studio Installation Guide](https://www.huawei.com/minisite/ascend/en/filedetail_1.html).
-   The Atlas 200 DK developer board has been connected to Mind Studio, the cross compiler has been installed, the SD card has been prepared, and basic information has been configured. For details, see  [Atlas 200 DK User Guide](https://www.huawei.com/minisite/ascend/en/filedetail_2.html).

## Software Preparation<a name="en-us_topic_0167333650_section431629175317"></a>

Before running the application, obtain the source code package and configure the environment as follows.

1.  Obtain the source code package.

    Download all the code in the sample-facedetection repository at  [https://github.com/Ascend/sample-ascendcamera](https://github.com/Ascend/sample-ascendcamera)  to any directory on Ubuntu Server where Mind Studio is located as the Mind Studio installation user, for example,  _/home/ascend/sample-ascendcamera_.

2.  Log in to Ubuntu Server where Mind Studio is located as the Mind Studio installation user and set the environment variable  **DDK\_HOME**.

    **vim \~/.bashrc**

    Run the following commands to add the environment variables  **DDK\_HOME**  and  **LD\_LIBRARY\_PATH**  to the last line:

    **export DDK\_HOME=/home/XXX/tools/che/ddk/ddk**

    **export LD\_LIBRARY\_PATH=$DDK\_HOME/uihost/lib**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   **XXX**  indicates the Mind Studio installation user, and  **/home/XXX/tools**  indicates the default installation path of the DDK.  
    >-   If the environment variables have been added, skip this step.  

    Enter  **:wq!**  to save and exit.

    Run the following command for the environment variable to take effect:

    **source \~/.bashrc**


## Deployment<a name="en-us_topic_0167333650_section1378112174282"></a>

1.  Log in to the root directory of the Ascend camera application code as the Mind Studio installation user, for example,  _**/home/ascend/sample-ascendcamera**_.
2.  <a name="en-us_topic_0167333650_li08019112542"></a>Run the deployment script to prepare the project environment, including compiling and deploying the ascenddk public library, downloading the network model, and configuring Presenter Server.

    **bash deploy.sh** _host\_ip_ _model\_mode_

    -   _host\_ip_: this parameter indicates the IP address of the Atlas 200 DK developer board.
    -   _model\_mode_  indicates the deployment mode of the model file. The default setting is  **internet**.
        -   **local**: If the Ubuntu system where Mind Studio is located is not connected to the network, use the local mode. In this case, download the dependent common code library to the  **sample-ascendcamera/script**  directory by referring to the  [Downloading Dependency Code Library](#en-us_topic_0167333650_section193081336153717).
        -   **internet**: Indicates the online deployment mode. If the Ubuntu system where Mind Studio is located is connected to the network, use the Internet mode. In this case, download the dependency code library online.


    Example command:

    **bash deploy.sh 192.168.1.2 internet**

    When the message  **Please choose one to show the presenter in browser\(default: 127.0.0.1\):**  is displayed, enter the IP address used for accessing the Presenter Server service in the browser. Generally, the IP address is the IP address for accessing the Mind Studio service.

    Select the IP address used by the browser to access the Presenter Server service in  **Current environment valid ip list**, as shown in  [Figure 1](#en-us_topic_0167333650_fig184321447181017).

    **Figure  1**  Project deployment<a name="en-us_topic_0167333650_fig184321447181017"></a>  
    ![](doc/source/img/project-deployment.png "project-deployment")

3.  Start Presenter Server.

    Run the following command to start the Presenter Server program of the face detection application in the background:

    **python3 script/presenterserver/presenter\_server.py --app display &**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >**presenter\_server.py**  is located in the  **script/presenterserve**  directory. You can run the  **python3 presenter\_server.py -h**  or  **python3 presenter\_server.py --help**  command in this directory to view the usage method of  **presenter\_server.py**.  

    [Figure 2](#en-us_topic_0167333650_fig69531305324)  shows that the presenter\_server service is started successfully.

    **Figure  2**  Starting the Presenter Server process<a name="en-us_topic_0167333650_fig69531305324"></a>  
    ![](doc/source/img/starting-the-presenter-server-process.png "starting-the-presenter-server-process")

    Use the URL shown in the preceding figure to log in to Presenter Server, only the Chrome browser is supported. The IP address is that entered in  [2](#en-us_topic_0167333650_li08019112542)  and the default port number is  **7003**. The following figure indicates that Presenter Server is started successfully.

    **Figure  3**  Home page<a name="en-us_topic_0167333650_fig64391558352"></a>  
    ![](doc/source/img/home-page.png "home-page")


## Saving Media Information Offline<a name="en-us_topic_0167333650_section8538523291"></a>

1.  Log in to the Atlas DK developer board as the  **HwHiAiUser**  user in SSH mode on Ubuntu Server where Mind Studio is located.

    **ssh HwHiAiUser@192.168.1.2**

2.  Go to the path of the executable file of Ascend camera.

    **cd \~/HIAI\_PROJECTS/ascend\_workspace/ascendcamera/out**

3.  Run  **ascendcamera**  to save the media information offline.
    -   Example 1: Obtain an image from the camera and save it as a .jpg file. If a file with the same name already exists, overwrite it.

        **./ascendcamera -i -c 1 -o  **_/localDirectory/filename.jpg_** **--overwrite**

        -   **-i**: Indicates that a JPG image is obtained.
        -   **-c**: Indicates the channel to which a camera belongs to. This parameter can be set to  **0**  or  **1**. The value  **0**  corresponds to  **Camera1**, and the value  **1**  corresponds to  **Camera2**. If this parameter is not set, the default value  **0**  is used.
        -   **-o**: Indicates the file storage location.  **localDirectory**  is the name of a local folder.  **filename.jpg**  is the name of a saved image, which can be user-defined.

            >![](doc/source/img/icon-note.gif) **NOTE:**   
            >The  **HwHiAiUser**  user must have the read and write permissions on the path.  

        -   **--overwrite**: Overwrites the existing file with the same name.

        For other parameters, run the  **./ascendcamera**  command or the  **./ascendcamera --help**  command. For details, see the help information.


    -   Example 2: Obtain a video from the camera and save it as a H.264 file. If a file with the same name already exists, overwrite it.

        **./ascendcamera -v -c 1 -t  _60_  --fps  _15_  -o   _/localDirectory/filename.h264_  --overwrite**

        -   **-v**: Indicates that a video clip in H.264 format is obtained.
        -   **-c**: Indicates the channel to which a camera belongs to. This parameter can be set to  **0**  or  **1**. The value  **0**  corresponds to  **Camera1**, and the value  **1**  corresponds to  **Camera2**. If this parameter is not set, the default value  **0**  is used.
        -   **-t**: Indicates that a video file lasting 60 seconds is obtained. If this parameter is not specified, the video file is obtained until the application exits.
        -   **--fps**: Indicates the frame rate of a saved video. The value range is 1–20. The default video frame rate is 10 fps.
        -   **-o**: Indicates the file storage location.** localDirectory**  is the name of a local folder.  **filename.h264**  is the name of a saved video, which can be user-defined.

            >![](doc/source/img/icon-note.gif) **NOTE:**   
            >The  **HwHiAiUser**  user must have the read and write permissions on the path.  

        -   **--overwrite**: Overwrites the existing file with the same name.

        For other parameters, run the  **./ascendcamera**  command or the  **./ascendcamera --help**  command. For details, see the help information.

        >![](doc/source/img/icon-note.gif) **NOTE:**   
        >-   This file can be played using the VLC media player. For details about how to download and use VLC, see  [https://www.videolan.org/vlc/download-sources.html](https://www.videolan.org/vlc/download-sources.html).  
        >-   You are advised to set the frame rate to be the same as that set when a video is saved.  



## Playing a Real-Time Video Through Presenter Server<a name="en-us_topic_0167333650_section65735410292"></a>

1.  Log in to the Atlas DK developer board as the  **HwHiAiUser**  user in SSH mode on Ubuntu Server where Mind Studio is located.

    **ssh HwHiAiUser@192.168.1.2**

2.  Go to the path of the executable file of Ascend camera.

    **cd \~/HIAI\_PROJECTS/ascend\_workspace/ascendcamera/out**

3.  Run the following command to transmit the video captured by the camera to Presenter Server:

    **./ascendcamera -v -c  _1_  -t  _60_ **--fps  _20_**  -w  _704_  -h  _576_  -s  _10.10.10.1_:7002/**_**presenter\_view\_app\_name**_

    -   **-v**: Indicates that a video clip in H.264 format is obtained.
    -   **-c**: Indicates the channel to which a camera belongs to. This parameter can be set to  **0**  or  **1**. The value  **0**  corresponds to  **Camera1**  in, and the value  **1**  corresponds to  **Camera2**  in. If this parameter is not set, the default value  **0**  is used.
    -   **-t**: Indicates that a video file lasting 60 seconds is obtained. If this parameter is not specified, the video file is obtained until the application exits.
    -   **--fps**: Indicates the frame rate of a saved video. The value range is 1–20. The default video frame rate is 10 fps.
    -   **-w**: Indicates the width of a saved video.
    -   **-h**: Indicates the height of a saved video.
    -   _10.10.10.1_  is the IP address of Presenter Server \(that is, the IP address for accessing Presenter Server entered in  [2](#en-us_topic_0167333650_li08019112542)\). The default port number of Presenter Server corresponding to the Ascend camera application is  **7002**.
    -   _presenter\_view\_app\_name_: Indicates  **View Name**  displayed on the Presenter Server page, which is user-defined.

    For other parameters, run the  **./ascendcamera**  command or the  **./ascendcamera --help**  command. For details, see the help information.

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   The Presenter Server of the Ascend camera application supports a maximum of 10 channels at the same time \(each  _presenter\_view\_app\_name_  parameter corresponds to a channel\).  
    >-   Due to hardware limitations, the maximum frame rate supported by each channel is 20fps,  a lower frame rate is automatically used when the network bandwidth is low.  


## Playing a Real-Time Video Through VLC<a name="en-us_topic_0167333650_section16698185019310"></a>

1.  Prerequisites.
    -   GStreamer has been installed on the Atlas 200 DK developer board.
        -   If the Atlas 200 DK developer board is connected to the network, run the following commands to install GStreamer:

            ```
            su - root
            ```

            ```
            apt-get install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools
            ```

        -   If the Atlas 200 DK developer board is not connected to the network, obtain the installation package of the related source code and install GStreamer.


    -   The VLC media player has been installed on the host and can communicate with the Atlas 200 DK developer board properly.

        >![](doc/source/img/icon-note.gif) **NOTE:**   
        >-   GStreamer is open-source third-party software that is not developed by Huawei. For details about how to deploy and use GStreamer, see  [https://gstreamer.freedesktop.org/](https://gstreamer.freedesktop.org/).  
        >-   You can use the server where Mind Studio is located or any host that can communicate with the Atlas 200 DK developer board properly.  
        >-   VLC is third-party software that is not developed by Huawei.  


2.  <a name="en-us_topic_0167333650_li38395471836"></a>Transmit the video streams obtained by the Ascend camera to any unoccupied port of the host through GStreamer.
    1.  Log in to the Atlas DK developer board as the  **HwHiAiUser**  user in SSH mode.

        **ssh HwHiAiUser@192.168.1.2**

    2.  Go to the path of the executable file of Ascend camera.

        **cd \~/HIAI\_PROJECTS/ascend\_workspace/ascendcamera/out**

    3.  Run the following command to transmit the video streams obtained by the Ascend camera to any unoccupied port of the host through GStreamer.

        **./ascendcamera -v -w  _1280_  -h  _720_  --fps  _10_  -o - |  _gst-launch-1.0_  -e -v fdsrc ! "video/x-h264,width=1280,height=720,framerate=10/1" ! h264parse ! flvmux ! tcpserversink host=_192.168.1.2_  port=_5000_**

        Description of configuration parameters:

        -   **-v**: Indicates that a video clip in H.264 format is obtained.
        -   **-w**: Indicates the width of a saved video.
        -   **-h**: Indicates the height of a saved video.
        -   **--fps**: Indicates the frame rate of a saved video. The value range is 1–20. The default video frame rate is 10 fps.
        -   **-o**:  **-**  indicates that the video streams are transmitted to standard output \(stdout\).
        -   **|**: Indicates the pipe character. The standard output is transmitted to GStreamer.

        -   _gst-launch-1.0_  -e -v fdsrc ! "video/x-h264,width=_1280_,height=_720_,framerate=_10/1_" ! h264parse ! flvmux ! tcpserversink host=_192.168.1.2_  port=_5000_:

            Indicates that GStreamer transmits the video streams received from stdout to port 5000 of the host whose IP address is  **192.168.1.2**. The video width and height are 1280 and 720 pixels, respectively, and the frame rate is 10 fps.

            In GStreamer,  **host**  indicates the IP address of the Atlas 200 DK developer board, and  **port**  indicates the ID of an occupied port on the Atlas 200 DK developer board.

            >![](doc/source/img/icon-note.gif) **NOTE:**   
            >The values of width, height, and frame rate of the GStreamer tool  **gst-launch-1.0**  must be the same as the values of  **-w**,  **-h**, and  **--fps**  in the Ascend camera application.  
            >For details about the parameters used by GStreamer, see  [https://gstreamer.freedesktop.org/](https://gstreamer.freedesktop.org/).  



3.  Use VLC to receive video streams.

    In the VLC media player on the host, choose  **Media \> Open Network Stream**, enter  _tcp://__192.168.1.2__:__5000_/ in the  **Please enter a network URL**  text box, set the playback parameters, and watch the real-time video.

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >The IP address and port number configured in VLC are those to which GStreamer sends videos in  [2](#en-us_topic_0167333650_li38395471836). The frame rate configured in VLC must be the same as that configured in  [2](#en-us_topic_0167333650_li38395471836).  
    >For details about how to use VLC, see  [https://www.videolan.org/vlc/download-sources.html](https://www.videolan.org/vlc/download-sources.html).  


## Follow-up Operations<a name="en-us_topic_0167333650_section856641210261"></a>

The Presenter Server service is always in the running state after being started. To stop the Presenter Server service of the Ascend camera application, perform the following operations:

Run the following command to check the process of the Presenter Server service corresponding to the Ascend camera application as the Mind Studio installation user:

**ps -ef | grep presenter | grep display**

```
ascend@ascend-HP-ProDesk-600-G4-PCI-MT:~/sample-ascendcamera$ ps -ef | grep presenter | grep display
ascend 5758 20313 0 14:28 pts/24?? 00:00:00 python3 presenterserver/presenter_server.py --app display
```

In the preceding information,  _5758_  indicates the process ID of the Presenter Server service corresponding to the Ascend camera application.

To stop the service, run the following command:

**kill -9** _5758_

## Downloading Dependency Code Library<a name="en-us_topic_0167333650_section193081336153717"></a>

Download the dependent software libraries to the **sample-ascendcamera/script** directory.

**Table  1**  Download the dependent software library

<a name="en-us_topic_0167333650_table141761431143110"></a>
<table><thead align="left"><tr id="en-us_topic_0167333650_row18177103183119"><th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.1"><p id="en-us_topic_0167333650_p8177331103112"><a name="en-us_topic_0167333650_p8177331103112"></a><a name="en-us_topic_0167333650_p8177331103112"></a>Module Name</p>
</th>
<th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.2"><p id="en-us_topic_0167333650_p1317753119313"><a name="en-us_topic_0167333650_p1317753119313"></a><a name="en-us_topic_0167333650_p1317753119313"></a>Module Description</p>
</th>
<th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.3"><p id="en-us_topic_0167333650_p1417713111311"><a name="en-us_topic_0167333650_p1417713111311"></a><a name="en-us_topic_0167333650_p1417713111311"></a>Download Address</p>
</th>
</tr>
</thead>
<tbody><tr id="en-us_topic_0167333650_row19177133163116"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="en-us_topic_0167333650_p2017743119318"><a name="en-us_topic_0167333650_p2017743119318"></a><a name="en-us_topic_0167333650_p2017743119318"></a>EZDVPP</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="en-us_topic_0167333650_p52110611584"><a name="en-us_topic_0167333650_p52110611584"></a><a name="en-us_topic_0167333650_p52110611584"></a>Encapsulates the dvpp interface and provides image and video processing capabilities, such as color gamut conversion and image / video conversion</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="en-us_topic_0167333650_p31774315318"><a name="en-us_topic_0167333650_p31774315318"></a><a name="en-us_topic_0167333650_p31774315318"></a><a href="https://github.com/Ascend/sdk-ezdvpp" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-ezdvpp</a></p>
<p id="en-us_topic_0167333650_p1634523015710"><a name="en-us_topic_0167333650_p1634523015710"></a><a name="en-us_topic_0167333650_p1634523015710"></a>After the download, keep the folder name <span class="filepath" id="en-us_topic_0167333650_filepath1324864613582"><a name="en-us_topic_0167333650_filepath1324864613582"></a><a name="en-us_topic_0167333650_filepath1324864613582"></a><b>sdk-ezdvpp</b></span>。</p>
</td>
</tr>
<tr id="en-us_topic_0167333650_row101773315313"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="en-us_topic_0167333650_p217773153110"><a name="en-us_topic_0167333650_p217773153110"></a><a name="en-us_topic_0167333650_p217773153110"></a>Presenter Agent</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="en-us_topic_0167333650_p19431399359"><a name="en-us_topic_0167333650_p19431399359"></a><a name="en-us_topic_0167333650_p19431399359"></a><span>API for interacting with the Presenter Server</span>.</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="en-us_topic_0167333650_p16684144715560"><a name="en-us_topic_0167333650_p16684144715560"></a><a name="en-us_topic_0167333650_p16684144715560"></a><a href="https://github.com/Ascend/sdk-presenter/tree/master/presenteragent" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-presenter/tree/master/presenteragent</a></p>
<p id="en-us_topic_0167333650_p82315442578"><a name="en-us_topic_0167333650_p82315442578"></a><a name="en-us_topic_0167333650_p82315442578"></a>After the download, keep the folder name <span class="filepath" id="en-us_topic_0167333650_filepath19800155745817"><a name="en-us_topic_0167333650_filepath19800155745817"></a><a name="en-us_topic_0167333650_filepath19800155745817"></a><b>presenteragent</b></span>.</p>
</td>
</tr>

<tr id="en-us_topic_0167333650_row101773315313"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="en-us_topic_0167333650_p217773153110"><a name="en-us_topic_0167333650_p217773153110"></a><a name="en-us_topic_0167333650_p217773153110"></a>Presenter Server</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="en-us_topic_0167333650_p19431399359"><a name="en-us_topic_0167333650_p19431399359"></a><a name="en-us_topic_0167333650_p19431399359"></a><span></span>Display the reasoning result pushed by the Presenter Agent, which can be accessed through a browser.</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="en-us_topic_0167333650_p16684144715560"><a name="en-us_topic_0167333650_p16684144715560"></a><a name="en-us_topic_0167333650_p16684144715560"></a><a href="https://github.com/Ascend/sdk-presenter/tree/master/presenterserver" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-presenter/tree/master/presenterserver</a></p>
<p id="en-us_topic_0167333650_p82315442578"><a name="en-us_topic_0167333650_p82315442578"></a><a name="en-us_topic_0167333650_p82315442578"></a>After the download, keep the folder name <span class="filepath" id="en-us_topic_0167333650_filepath19800155745817"><a name="en-us_topic_0167333650_filepath19800155745817"></a><a name="en-us_topic_0167333650_filepath19800155745817"></a><b>presenterserver</b></span>.</p>
</td>
</tr>

<tr id="en-us_topic_0167333650_row101773315313"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p>tornado (5.1.0)</p><p>protobuf (3.5.1)</p><p>numpy (1.14.2)</P>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="en-us_topic_0167333650_p19431399359"><a name="en-us_topic_0167333650_p19431399359"></a><a name="en-us_topic_0167333650_p19431399359"></a><span>Python libraries that Presenter Server depends on.</span>.</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="en-us_topic_0167333650_p16684144715560">Search for related sources and install them.</p>
</td>
</tr>
</tbody>
</table>

