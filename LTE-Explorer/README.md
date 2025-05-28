# LTE-M TELEMETRY DATA OF MLS/100EV

# 1. INTRODUCTION

> :information_source: **IMPORTANT!**
>
> Please read the **[first steps with MLS/100EV](https://github.com/SSV-embedded/MLS100EV/tree/main)** before you continue reading this document.

This document describes how to install and configure the **LTE-M explorer firmware** as well as the necessary **Node-RED flow** for receiving the LTE-M telemetry data of the MLS/100EV via MQTT and recording it in a CSV file.

The LTE-M telemetry data can be used to evaluate the connection quality and stability between the MLS/100EV as IoT data source and the workstation as IoT backend over a longer period of time.

It also describes how to visualize and analyze the telemetry data with Google Colab.

> :information_source: **Please note:**
>
> The description in this document refers to a workstation running on Windows with Node-RED installed.

![Application example for the MLS/100EV](https://ssv-embedded.de/bilder/github/mls100ev_lte_overview.png)

*Figure 1: Application example for the MLS/100EV*

## 1.1 Required Equipment

The following equipment is required to record data with the MLS/100EV:

+ A workstation with
    + Linux or Windows (version 10 or higher)
    + an Internet connection
    + a web browser
    + a USB port
    + installed Node-RED
    + installed Apache Newt Manager

+ A USB power supply with a stable output voltage of 5 VDC.

+ A [Google Colab](https://colab.research.google.com/) account (this is needed to visualize and analyze the measured data)

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

Connect the MLS/100EV to your workstation with the provided USB-C cable.

## 3.1 Installing the SSV/MLS LTE-M Explorer Firmware

In the **[MLS/100EV GitHub repository](https://github.com/SSV-embedded/MLS100EV)** you can download the **[LTE-M explorer firmware file](https://github.com/SSV-embedded/MLS100EV/lte_explorer/mls_lte_1.0.0.bin)**.

In the first steps document you can find a description on **[how to install a new firmware for the MLS/100EV](https://github.com/SSV-embedded/MLS100EV/tree/main?tab=readme-ov-file#5-firmware-update)**.

> :information_source: **Please note:**
>
> The LTE-M explorer firmware also changes the behaviour of the status LED.
> 
> + The **LED lights up permantly white** when the MLS/100EV is trying to establish a connection to the MQTT broker.
> + The **LED lights up permantly green** when the MLS/100EV has established a connection to the MQTT broker.
> + The **LED lights up permantly red** if the connection to the MQTT broker could not be established.
> + The **LED flashes blue** when data is being sent to the MQTT broker.

As the MLS/100EV automatically connects to the MQTT broker, the **status LED should light up green permanently** after installing the LTE-M-Explorer firmware.

> :information_source: **Please note:**
>
> Depending on the LTE-M signal strength, it may take a while for the MLS/100EV to establish a connection to the MQTT broker.

## 3.2 Importing and Configuring the SSV/MLS LTE-M Telemetry Data Node-RED Flow

Start Node-RED on your workstation and import the **[SSV/MLS LTE-M telemetry data flow](lte_csv_data_logger_flow.json)**. You will find this flow in the same repository as this document.

![Imported SSV/MLS LTE-M telemetry data flow](https://ssv-embedded.de/bilder/github/nodered_lte_flow.png)

*Figure 2: Imported SSV/MLS LTE-M telemetry data flow*

### 3.2.1 Choosing the CSV File Storage Location

The default storage location of the CSV-file is in the Node-RED working directory.

To change the location, double click the **mls_lte_data.csv** node to open its properties dialog.

In the **Filename** text field you can change the filename and location.

> :information_source: **Please note:**
>
> The filename should be an absolute path, otherwise it will be relative to the working directory of the Node-RED process.

![Properties of CSV file node](https://ssv-embedded.de/bilder/github/nodered_csv-node_edit.png)

*Figure 3: Properties of the CSV file node*

When everything is configured, click the `Deploy` button on the right in the header to save the changes.

## 3.3 Configuring the MQTT Node

> :information_source: **IMPORTANT!**
>
> **The MLS/100EV sends its messages to a public MQTT broker. This means that anyone is able to receive the data and with the knowledge of the telemetry data details to locate the position of your device within the mobile network.**

To configure the MQTT node, you first need to know the **IMEI** of the MLS/100EV. The IMEI can be found in the debug messages from the **debug serial** node.

To receive the debug messages, the **COM port** node must be configured.

Therefore double click the **COM port** node to open its properties dialog and click the **pencil icon** ![pencil button](https://ssv-embedded.de/bilder/github/nodered_but_pencil.png) to edit the serial port settings.

> :information_source: **Please note:**
>
> In the first steps document you can find a description on **[how to find the COM port of the MLS/100EV](https://github.com/SSV-embedded/MLS100EV/tree/main#34-connecting-the-mls100ev-to-the-workstation)**.

Enter the following settings:

+ **Serial Port:** Choose the COM port of the MLS/100EV, e.g. COM4.
+ **Baud rate:** 115200
+ **Data bits:** 8
+ **Parity:** None
+ **Stop bits:** 1

Click the `Update` button at the top and then the `Done` button to close the node configuration.

![Properties of COM port node](https://ssv-embedded.de/bilder/github/nodered_com-port-node_edit_edit.png)

*Figure 4: Properties of the COM port node*

Now click the **debug icon** ![debug button](https://ssv-embedded.de/bilder/github/nodered_but_debug.png) at the top of the sidebar to open the debug window. Please wait up to **2 minutes** until you see the first message in the debug window.

> :information_source: **Please note:**
>
> If another flow also uses the COM port of the MLS/100EV (e.g. the SSV/MLS data logger flow), this can lead to unexpected results. We therefore recommend disabling the other flows. To do this, right-click on the corresponding tab of the flow and click on **Disable flow**.
> 
> ![Disbale a flow](https://ssv-embedded.de/bilder/github/nodered_disable_flow.png)
> 
> *Figure 5: Disbale a flow*

Copy the IMEI; it is the **second number in the message** that comes directly after the timestamp.

![Debug window with incoming data](https://ssv-embedded.de/bilder/github/nodered_lte_copy_imei.png)

*Figure 6: Debug window with incoming data*

Now double click the **mls/imei** node to open its properties dialog, enter the IMEI in the **Topic text field** and click the `Done` button.

![Properties of MQTT node](https://ssv-embedded.de/bilder/github/nodered_mqtt_imei.png)

*Figure 7: Properties of the MQTT node*

When everything is configured, click the `Deploy` button.

The MQTT node receives now the LTE-M telemetry data of the MLS/100EV at **2-minutes intervals** from the MQTT broker. The incoming data can be seen in the debug window.

![Debug window with incoming LTE-M data](https://ssv-embedded.de/bilder/github/nodered_debug_lte_data.png)

*Figure 8: Debug window with incoming LTE-M data*

> :information_source: **Please note:**
>
> After the IMEI has been copied, the COM port flow is no longer required and can be deleted.

### 3.3.1 Start Recording Data

To start recording the data in the CSV file, draw a wire from the **time node** to the **CSV node** like shown in the following figure and click the `Deploy` button. 

![Connection between time node and CSV node](https://ssv-embedded.de/bilder/github/nodered_start_capture.png)

*Figure 9: Connection between time node and CSV node*

### 3.3.2 Stop Recording Data

To stop recording the data in the CSV file, select the wire between the **time node** and the **CSV node**, press the `Delete` key and then click the `Deploy` button.

Now open the CSV file to make sure the data was recorded. It should look like in the following figure.

![CSV file with recorded LTE-M telemetry data](https://ssv-embedded.de/bilder/github/csv_lte_data.png)

*Figure 10: CSV file with recorded LTE-M telemetry data*

# 4. OPERATION

Now you can place the MLS/100EV at your desired location and connect it to a 5 VDC USB power supply.

> :warning: **ATTENTION!**
>
> **The MLS/100EV is for indoor use only!**
> 
> **Make sure that the device and the USB power supply are positioned in such a way that they are secured against falling and no one can trip over them.**

## 4.1 Data Acquisition

When everything is configured and in place, you can start the data acquisition. To do this, draw a wire from the **time node** to the **CSV node** like shown in figure 9 and click the `Deploy` button.

**The MLS/100EV sends a telemetry data set at 2-minutes intervals.**

Record the data over a reasonable period of time. In order to make a meaningful evaluation of the LTE-M connection quality, we recommend recording the data for a few days.

To stop the data acquisition, select the wire between the **time node** and the **CSV node**, press the `Delete` key and then click the `Deploy` button.

## 4.2 Data Analysis

To analyze the recorded data, we recommend using [Google Colab](https://colab.research.google.com/). This requires a Google account.

You will also need the **[analyze_lte.ipynb](analyze_lte.ipynb)** Python notebook, which you can find in the same repository as this document.

> :information_source: **Please note:**
>
> In the first steps document you can find a **[more detailed description of the necessary steps in Colab](http://apps.terminal/wbu/wbu-playground/-/blob/master/mls100ev/mls100ev_first_steps.md#user-content-42-data-analysis)**.

Log in to Colab and upload `analyze_lte.ipynb`.

Then upload your CSV file, e.g. `mls_lte_data.csv`.

Now execute the Python scripts in the notebook.

> :information_source: **Please note:**
>
> If the filename of your CSV file is not `mls_lte_data.csv`, you must edit the filename in the Python code!  

Below the code cells, you will now see a diagram with the values of **rsrp_dbm** (Reference Signal Received Power in dBm).

![Diagram with the values of rsrp_dbm](https://ssv-embedded.de/bilder/github/colab_diagram_lte_signal.png)

*Figure 11: Diagram with the values of rsrp_dbm*

### 4.2.1 Meaning of the Values

| RSRP (dBm) | Signal Quality |
| ---:| --- |
| **> -80 dBm**	| Excellent |
| **-80 to -90 dBm** | Good |
| **-90 to -100 dBm** | Fair |
| **-100 to -110 dBm** | Poor |
| **< -110 dBm** | Very poor / unusable |

*Table 1: RSRP values correspond to signal quality*

### 4.2.2 Information about the Used Cell Tower

With the recorded data it is also possible to find some information about the used cell tower(s) of the LTE-M connection.

Go to **https://www.opencellid.org** and enter the following data points in the corresponding text fields on the map:

+ **plnm:** The first three digits are the **MCC** (Mobile Country Code), the remaining digits are the **MNC** (Moblie Network Code).

+ **tac:** Convert this hexadecimal number to a decimal number and enter it in the **LAC** (Location Area Code) text field.

+ **cell_id**

The following figure shows where you can find the correct data points in the CSV file:

![Data points for Open Cell ID in the CSV file](https://ssv-embedded.de/bilder/github/csv_opencellid.png)

*Figure 12: Data points for Open Cell ID in the CSV file*

![Map on Open Cell ID](https://ssv-embedded.de/bilder/github/mls100ev_map_opencellid.png)

*Figure 13: Map on Open Cell ID with cell tower information*

Find more information about [Open Cell ID on Wikipedia](https://en.wikipedia.org/wiki/OpenCellID).

# 5. DESCRIPTION OF THE LTE-M TELEMETRY DATA SET

In the following table, the individual data points of the LTE-M telemetry data set are described in the same order as they are sent by the MLS/100EV and saved in the CSV file.

| COLUMN | DESCRIPTION |
| --- | --- |
| **imei** | IMEI (International Mobile Equipment Identity) of the LTE-M modem. This number is used as topic name for the public MQTT broker to publish and subscribe the LTE-M telemetry data. |
| **vbat_mv** | Current battery voltage. |
| **ci_job_id** | SSV internal firmware build ID. |
| **rsrq** | Reference Signal Received Quality. |
| **rsrp** | Reference Signal Received Power. |
| **rsrq_db** | Reference Signal Received in dB. |
| **rsrp_dbm** | Reference Signal Received Power in dBm. |
| **full_name** | Operator name. |
| **plnm** | Mobile Country Code + Mobile Network Code, 3 digits each. |
| **tac** | Tracing Area Code. |
| **act** | 7: E-UT RAN / 9: E-UTRAN NB-S1 |
| **band** | Band as defined in 3GPP 36.101, 0 when current band information is not available. |
| **cell_id** | Hexadecimal string encoding the 4-byte E-UTRAN cell ID. |
| **phys_cell_id** | Physical cell ID. |
| **earfcn** | LTE-M carrier channel number for unique identification of LTE-M band and carrier frequency. |
| **snr** | Signal to Noise Ratio. |
| **nwp_edrx** | Extended rx networking configuration. |
| **active_time** | Indicates weather power saving (PSM) is available on the network, encodes the amount of time the modem may sleep for. |
| **periodic_tau_ext** | Interval during which the cell tower must be contacted during power save mode, allows for longer intervals than `periodic_tau`. |
| **periodic_tau** | see `periodic_tau_ext`. |
| **ping** | Monotonically increasing counter, increases with each status message. |

*Table 2: Description of all data points of the MLS/100EV LTE-M telemetry data set*

# 6. DESCRIPTION OF THE STATUS LED

| LED STATE | DESCRIPTION |
| --- | --- |
| **Lights up permantly white** | The MLS/100EV is trying to establish a connection to the MQTT broker. |
| **Lights up permantly green** | The MLS/100EV has established a connection to the MQTT broker. |
| **Lights up permantly red** | The connection to the MQTT broker could not be established. |
| **Flashes blue** | Data is being sent to the MQTT broker. |

*Table 3: Description of the status LED*

# 7. HELPFUL LITERATURE

+ [Node-RED](https://nodered.org)

+ [Google Colab](https://colab.research.google.com/)

+ [Apache Newt Manager](https://mynewt.apache.org/latest/os/modules/devmgmt/newtmgr.html)

+ [Open Cell ID on Wikipedia](https://en.wikipedia.org/wiki/OpenCellID)

---

*author: wbu // review: adi // 05-2025 // rev. 1.1*
