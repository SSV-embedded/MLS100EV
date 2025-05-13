# FIRST STEPS WITH MLS/100EV

# 1. INTRODUCTION

This document describes the first steps for using your new soft sensor evaluation device MLS/100EV to record machine condition data.

> :information_source: **Please note:**
>
> The description in this document refers to a workstation running on Windows.

![Application example for the MLS/100EV](https://ssv-embedded.de/bilder/github/mls100ev_overview.png)

*Figure 1: Application example for the MLS/100EV*

## 1.1 Checklist

Please compare the content of your MLS/100EV package with the checklist below.

+ 1x MLS/100EV
+ 1x USB-C cable
+ 1x Double-sided tape
+ 1x Screw driver
+ 1x Instruction leaflet

Contact SSV if any item is missing or appears to be damaged.

## 1.2 Required Equipment

The following equipment is required to record data with the MLS/100EV:

+ A workstation
    + with Linux or Windows (version 10 or higher)
    + an Internet connection
    + a web browser
    + a USB port

+ A [Google Colab](https://colab.research.google.com/) account (this is needed to visualize the measured data)

# 2. SAFETY GUIDELINES

**Please read the following safety guidelines carefully! In case of property or personal damage by not paying attention to this document and/or by incorrect handling, we do not assume liability. In such cases any warranty claim expires.**

> :warning: **ATTENTION!**
>
> **OBSERVE PRECAUTIONS FOR HANDLING – ELECTROSTATIC SENSITIVE DEVICE!**

+ **The device is for indoor use only!**
+ **The power supply should be in immediate proximity to the device.**
+ **The power supply must provide a stable output voltage of 5 VDC.**
+ **Please pay attention that the power cord or other cables are not squeezed or damaged in any way when you set up the device.**
+ **Discharge yourself electrostatically before working with the device, e.g. by touching a metal radiator, to avoid damages.**
+ **Stay grounded while working with the device, to avoid damage through electrostatic discharge.**

# 3. PREPARATIONS

## 3.1 Installing Node-RED

To record the data of the MLS/100EV, the free browser-based flow programming tool **Node-RED** is required.

Here you will find a quick start guide for installing Node-RED on a Windows workstation:

+ **[Installing Node-RED on Windows](https://nodered.org/docs/getting-started/windows)**

## 3.2 Installing the Node-RED Dashboard

To visualize the data of the MLS/100EV, the **Node-RED dashboard** must be installed.

### 3.2.1 Installing via Command Line

Open a command line interface, change to the Node-RED working directory and enter following command:

```
cd ~/.node-red
npm install node-red-dashboard
```
### 3.2.2 Installing via Palette Manager

Open Node-RED, click the **menu icon** in the top right corner and click **Manage palette**.

Click on the **Install** tab in the palette dialog and enter **dashboard** in the search bar.

![Palette search results](https://ssv-embedded.de/bilder/github/nodered_palette_install_dashboard.png)

*Figure 2: Palette search results*

Look for **node-red-dashboard** in the search results and click the `Install` button.  Confirm the installation in the upcoming message and click the `Close` button.

> :bulb: **TIP!**
> 
> Here you can find more information on how to change the settings, layout etc. of the dashboard:
> 
> **https://flows.nodered.org/node/node-red-dashboard**

## 3.3 Installing the "serialport" Node

After the successful installation of Node-RED, the **serialport node** must be installed.

### 3.3.1 Installing via Command Line

Open a command line interface, change to the Node-RED working directory and enter following command:

```
cd ~/.node-red
npm install node-red-node-serialport
```

### 3.3.2 Installing via Palette Manager

Open Node-RED, click the **menu icon** in the top right corner and click **Manage palette**.

Click on the **Install** tab in the palette dialog and enter **serial** in the search bar.

![Palette search results](https://ssv-embedded.de/bilder/github/nodered_palette_search.png)

*Figure 3: Palette search results*

Look for **node-red-node-serialport** in the search results and click the `Install` button.  Confirm the installation in the upcoming message and click the `Close` button.

## 3.4 Connecting the MLS/100EV to the Workstation

Now connect the MLS/100EV with the provided USB-C cable to the workstation. The LED starts flashing green to indicate that data is being generated.

Windows recognizes the new device and installs an appropriate driver. This may take a moment.

The MLS/100EV can then be addressed via a **virtual COM port** (serial interface). This virtual COM port is needed for communication and data transfer.

> :bulb: **TIP!**
>
> To find out which virtual COM port is connected to the MLS/100EV, open the **device manager** and click **Ports (COM & LPT)**. The last entry in the list should be the desired virtual COM port and should have a name like **USB Serial Port (COM16)**.
> 
> To make sure that it is the correct virtual COM port, just disconnect and reconnect the USB cable to see which list item disappears and then reappears.

## 3.5 Importing the SSV/MLS CSV Data Logger Flow

To record the data coming from the MLS/100EV, the **[SSV/MLS CSV Data Logger](mls_csv_data_logger_flow.json)** flow must be imported and configured in Node-RED. You will find this flow in the same repository as this document.

Download the flow to the workstation. Then go to Node-RED and click the **menu icon** in the top right corner and click **Import**. In the next dialog select the SSV/MLS CSV Data Logger flow and click the `Import` button at the bottom.

![Importing flow](https://ssv-embedded.de/bilder/github/nodered_import_flow.png)

*Figure 4: Importing flow*

The **SSV/MLS CSV Data Logger** flow should appear on the workspace. If the workspace is still empty, click on the corresponding tab at the top of the workspace to see the new flow.

![Imported SSV/MLS CSV data logger flow](https://ssv-embedded.de/bilder/github/nodered_flow.png)

*Figure 5: Imported SSV/MLS CSV data logger flow*

> :information_source: **Please note:**
> 
> If the flow shows any errors, please make sure that you have installed the **Node-RED dashboard** (see [chapter 3.2](#32-installing-the-node-red-dashboard)) as well as the **serialport node** (see [chapter 3.3](#33-installing-the-serialport-node))!

### 3.5.1 Configuring the Serial Port

To receive data from the MLS/100EV, the **Serial In MLS100EV** node of the flow must be configured.

Therefore double click the **Serial In MLS100EV** node to open its properties dialog and click the **pencil icon** ![pencil button](https://ssv-embedded.de/bilder/github/nodered_but_pencil.png) to edit the serial port settings.

Enter the following settings:

+ **Serial Port:** Choose the COM port of the MLS/100EV, e.g. COM4.
+ **Baud rate:** 115200
+ **Data bits:** 8
+ **Parity:** None
+ **Stop bits:** 1

Click the `Update` button at the top and then the `Done` button to close the node configuration.

![Properties of serialport node](https://ssv-embedded.de/bilder/github/nodered_serialport-node_edit_edit.png)

*Figure 6: Properties of the serialport node*

### 3.5.2 Choosing the CSV File Storage Location

The default storage location of the CSV-file is in the Node-RED working directory.

To change the location, double click the **mls100ev_data.csv** node to open its properties dialog.

In the **Filename** text field you can change the filename and location.

> :information_source: **Please note:**
>
> The filename should be an absolute path, otherwise it will be relative to the working directory of the Node-RED process.

![Properties of CSV file node](https://ssv-embedded.de/bilder/github/nodered_csv-node_edit.png)

*Figure 7: Properties of the CSV file node*

When everything is configured, click the `Deploy` button on the right in the header to save the changes.

## 3.6 Testing the Connection

To test the connection between Node-RED and the MLS/100EV, click the **debug icon** ![debug button](https://ssv-embedded.de/bilder/github/nodered_but_debug.png) at the top of the sidebar to open the debug window. You should now see the incoming data in the debug window.

![Debug window with incoming data](https://ssv-embedded.de/bilder/github/nodered_test_debug.png)

*Figure 8: Debug window with incoming data*

### 3.6.1 View Data in Dashboard

Click the **arrow down icon** ![arrow down button](https://ssv-embedded.de/bilder/github/nodered_but_arrow_down.png) in the sidebar and select **Dashboard**.

![Sidebar menu](https://ssv-embedded.de/bilder/github/nodered_sidebar_menu_dashboard.png)

*Figure 9: Sidebar menu*

Then click the **external link icon** ![external link button](https://ssv-embedded.de/bilder/github/nodered_but_external.png). This opens a new browser tab with the Node-RED dashboard, which displays the live data from the MLS/100EV.

![Node-RED dashboard with incoming data](https://ssv-embedded.de/bilder/github/nodered_dashboard_diagram.png)

*Figure 10: Node-RED dashboard with incoming data*

### 3.6.2 Start Recording Data

To start recording the data in the CSV file, draw a wire from the **trim node** to the **CSV node** like shown in figure 11 and click the `Deploy` button. 

![Connection between trim node and CSV node](https://ssv-embedded.de/bilder/github/nodered_csv-node_wired.png)

*Figure 11: Connection between trim node and CSV node*

### 3.6.3 Stop Recording Data

To stop recording the data in the CSV file, select the wire between the **trim node** and the **CSV node**, press the `Delete` key and then click the `Deploy` button.

Now open the CSV file to make sure the data was recorded. It should look like in figure 12.

![CSV file with recorded data](https://ssv-embedded.de/bilder/github/csv.png)

*Figure 12: CSV file with recorded data*

## 3.7 Attaching the Device to a Machine

Select a machine whose condition you want to record. We recommend something with rotating components like a washing machine, a fan, or a heat pump.

Attach the MLS/100EV with the provided double-sided tape to the machine.

> :information_source: **IMPORTANT!**
>
> Do not place the double-sided tape over the nameplate of the MLS/100EV.

> :warning: **ATTENTION!**
>
> **The MLS/100EV is for indoor use only!**
> 
> **Make sure that the workstation and the USB cable are positioned in such a way that they are secured against falling and no one can trip over them.**

# 4. OPERATION

## 4.1 Data Acquisition

When everything is configured and in place, you can start the data acquisition. To do this, draw a wire from the **trim node** to the **CSV node** like shown in figure 11 and click the `Deploy` button.

Record the data over a reasonable period of time. If you are monitoring a washing machine, for example, it would make sense to record the entire program.

To stop the data acquisition, select the wire between the **trim node** and the **CSV node**, press the `Delete` key and then click the `Deploy` button.

## 4.2 Data Analysis

To analyze the recorded machine data, we recommend using [Google Colab](https://colab.research.google.com/). This requires a Google account.

You will also need the **[analyze.ipynb](analyze.ipynb)** Python notebook, which you can find in the same repository as this document.

Log in to Colab, click in the **Open notebook** window on **Upload** and upload `analyze.ipynb`.

![Colab start window](https://ssv-embedded.de/bilder/github/colab_new.png)

*Figure 13: Colab "Open notebook" window*

![Colab analyze notebook](https://ssv-embedded.de/bilder/github/colab_analyze_notebook.png)

*Figure 14: Analyze Python notebook*

Click the **file icon** on the left to open the file explorer. Make a right-click in the file explorer below the **sample_data** entry, click **Upload** and upload your CSV file, e.g. `mls100ev_data.csv`.

![Upload the CSV file to Colab](https://ssv-embedded.de/bilder/github/colab_upload.png)

*Figure 15: Upload the CSV file to Colab*

Now you can execute the Python scripts in the notebook.

You can do this for each code cell in turn by clicking the **play button** to the left of the code cell and waiting each time until a **small green tick** appears next to the play button. Then click the play button for the next code cell and so on.

> :bulb: **TIP!**
>
> You can execute all code cells at once by clicking on **Runtime** in the menu bar and then clicking **Run all**. You can also use the shortcut `Ctrl` + `F9`.

![Run all code cells](https://ssv-embedded.de/bilder/github/colab_run_all.png)

*Figure 16: Run all code cells*

> :information_source: **Please note:**
>
> If the filename of your CSV file is not `mls100ev_data.csv`, you must edit the filename in the Python code!  

Below the code cells, you will now see a diagram of your recorded machine data.

![Diagram of the recorded data](https://ssv-embedded.de/bilder/github/colab_diagram.png)

*Figure 17: Diagram of the recorded data*

# 5. FIRMWARE UPDATE

In the **[MLS/100EV GitHub repository](https://github.com/SSV-embedded/MLS100EV)** you can download firmware files (`*.bin`) that support different Mikroe click board™ sensor modules for various use cases.

For the following description we will use the [`mls_blue_led_1.0.0.bin`](mls_blue_led_1.0.0.bin).

> :information_source: **Please note:**
>
> The ex-factory firmware file of the MLS/100EV is called [`mls_imu_1.0.0.bin`](mls_imu_1.0.0.bin).

## 5.1 Installing the Apache Newt Manager

To load a new firmware into the MLS/100EV, the command line tool **Newt Manager** from Apache is needed. Here you can **[download Newt Manager for Windows](https://dlcdn.apache.org/mynewt/apache-mynewt-1.13.0/apache-mynewt-newtmgr-bin-windows-1.13.0.tgz)**.

The downloaded archive contains an executable file called `newtmgr.exe`. Place it in a directory of your choice. 

### 5.1.1 Adding a Serial Connection

Open a command line interface, change to the directory with the `newtmgr.exe` and enter following command:

```
.\newtmgr conn add serial type=serial connstring="dev=COM4,baud=115200"
```

This setup will be stored at `C:\users\your_user_name\.newtmgr.cp.json`. This command therefore only needs to be executed once, unless the COM port of the MLS/100EV changes.

> :information_source: **Please note:**
>
> The used COM port (COM4) in this command is only an example! You must enter the COM port connected to the MLS/100EV. The [chapter 3.4](#34-connecting-the-mls100ev-to-the-workstation) describes how to find the correct COM port.

## 5.2 Opening the MLS/100EV

> :warning: **ATTENTION!**
>
> **To avoid damage through electrostatic discharge, discharge yourself electrostatically before working with the device, e.g. by touching a metal radiator, and stay grounded while working with the device.**

To start the **device firmware update (DFU) mode**, the housing must be unscrewed. You can use the screwdriver supplied for this purpose.

> :warning: **ATTENTION!**
>
> **Be careful when lifting the lid and make sure that you do not tear the antenna cable off the board or the lid! Place the parts in front of you like shown in the following figure.**

![Open MLS/100EV in DFU mode](https://ssv-embedded.de/bilder/github/mls100ev_open_update_mode.jpg)

*Figure 18: Open MLS/100EV in DFU mode*

## 5.3 Uploading New Firmware

> :information_source: **Please note:**
>
> Before starting the upload, make sure to disable the Node-RED flow or terminate Node-RED. Otherwise, the COM port is blocked and cannot be used to transfer the firmware file.

Connect the MLS/100EV to the workstation if it is not already connected.

Now push and hold the **BE button**, push and release the **RST button** and then release the BE button. The status LED lights up permantly red to indicate the DFU mode.

To transfer the firmware file, open a command line and enter the following command:

```
.\newtmgr -c serial image upload .\mls_blue_led_1.0.0.bin
```
> :information_source: **Please note:**
>
> In this example the firmware file is placed in the same directory as the `newtmgr.exe`. If this is not the case, you must enter the correct path to the storage location of the firmware file.

The file transfer may take a while.

After the successful upload, enter the following command or simply push the **RST button**, to exit the DFU mode:

```
.\newtmgr -c serial reset
```

The status LED starts flashing blue.

> :information_source: **Please note:**
>
> If the LED does not start flashing blue after the reset, simply repeat the reset.

To restore the MLS/100EV to the factory settings, simply repeat the steps from chapters 5.2 and 5.3 with the firmware file `mls_imu_1.0.0.bin`.

# 6. TECHNICAL DATA

+ **Processor**
    + Arm Cortex M33 application MCU for AI-based sensor data analysis
    ---
+ **Storage**
    + 1 MB Flash
    + 256 KB RAM
    ---
+ **Interfaces**
    + 1x USB-C interface for power supply and communication
    + 2x Internal mikroBUS™ connector for sensor module extensions
    + 1x Internal qwiic connector for I2C-based extensions
    + 1x Internal debug connector plus DFU mode buttons (RST and BE)
    + 1x Internal embedded LTE-M antenna
    ---
+ **Special functions**
    + 1x Internal nano SIM card holder (Fourth Form Factor 4FF)
    + Mobile Virtual Network Operator (MVNO) SIM card with IoT data plan pre-installed (usable worldwide)
    + IoT protocol stack with (D)TLS support for Internet radio connections
    + Firmware updates and configuration settings via USB
    + Optional firmware variants for normal and testbed operation
    ---
+ **LTE-M modem**
    + RFSoC-based LTE-M modem (pre-certified)
    + Frequency range: 700 - 2.200 MHz
    + Supported international LTE-M bands: B1-B5, B8, B12, B13, B14, B17-B20, B25, B26, B28, B66
    + Max. transfer rates: 300 Kbps downlink / 375 kbps uplink
    + 3GPP AT commands according to TS 27.007 plus extensions
    ---
+ **Displays / control elements**
    + 1x Status LED (RGB)
    ---
+ **Electrical characteristics**
    + Supply voltage: 5 VDC
    + Power consumption: 500 mA
    ---
+ **Mechanical characteristics**
    + Protection class: IP20
    + Dimensions: 110 x 57 x 27 mm
    + Operating temperature: 0 - 70 °C

## 6.1 Orientation of the Sensor Axes

![Orientation of the MLS/100EV sensor axes](https://ssv-embedded.de/bilder/github/mls100ev_orientation_axes.png)

*Figure 19: Orientation of the MLS/100EV sensor axes*

## 6.2 Board Layout

![Board Layout of the MLS/100EV](https://ssv-embedded.de/bilder/github/mls100ev_igel.png)

*Figure 20: Board Layout of the MLS/100EV*

## 6.3 Description of the CSV Data Set

| COLUMN | DESCRIPTION |
| --- | --- |
| **time** | Detection time in milliseconds since the last reset of the MLS/100EV. |
| **x_rms** | IMU x-axis: Square mean of all x-axis measured values. |
| **x_freq_0** | IMU x-axis: Frequency of the highest peak in the FFT of the x-axis. |
| **x_mag_0** | IMU x-axis: Amplitude of the highest peak in the FFT of the x-axis. |
| **x_freq_1** | IMU x-axis: Frequency of the second highest peak in the FFT of the x-axis. |
| **x_mag_1** | IMU x-axis: Amplitude of the second highest peak in the FFT of the x-axis. |
| **x_freq_2** | IMU x-axis: Frequency of the third highest peak in the FFT of the x-axis. |
| **x_mag_2** | IMU x-axis: Amplitude of the third highest peak in the FFT of the x-axis. |
| **y_rms** | IMU y-axis: Square mean of all y-axis measured values. |
| **y_freq_0** | IMU y-axis: Frequency of the highest peak in the FFT of the y-axis. |
| **y_mag_0** | IMU y-axis: Amplitude of the highest peak in the FFT of the y-axis. |
| **y_freq_1** | IMU y-axis: Frequency of the second highest peak in the FFT of the y-axis. |
| **y_mag_1** | IMU y-axis: Amplitude of the second highest peak in the FFT of the y-axis. |
| **y_freq_2** | IMU y-axis: Frequency of the third highest peak in the FFT of the y-axis. |
| **y_mag_2** | IMU y-axis: Amplitude of the third highest peak in the FFT of the y-axis. |
| **z_rms** | IMU z-axis: Square mean of all z-axis measured values. |
| **z_freq_0** | IMU z-axis: Frequency of the highest peak in the FFT of the z-axis. |
| **z_mag_0** | IMU z-axis: Amplitude of the highest peak in the FFT of the z-axis. |
| **z_freq_1** | IMU z-axis: Frequency of the second highest peak in the FFT of the z-axis. |
| **z_mag_1** | IMU z-axis: Amplitude of the second highest peak in the FFT of the z-axis. |
| **z_freq_2** | IMU z-axis: Frequency of the third highest peak in the FFT of the z-axis. |
| **z_mag_2** | IMU z-axis: Amplitude of the third highest peak in the FFT of the z-axis. |

*Table 1: Description of all 22 data elements of the MLS/100EV CSV data set*

# 7. HELPFUL LITERATURE


+ [Node-RED](https://nodered.org)

+ [Node-RED Dashboard](https://flows.nodered.org/node/node-red-dashboard)

+ [Google Colab](https://colab.research.google.com/)

+ [Mikroe Click Boards™](https://www.mikroe.com/click-boards)

+ [Apache Newt Manager](https://mynewt.apache.org/latest/os/modules/devmgmt/newtmgr.html)

---

*author: wbu // review: adi // 05-2025 // rev. 1.1*
