# Vitis-AI-DPU-TRD-for-ZCU106
Port of the DPU_TRD from the ZCU104 to ZCU106 Board with Vitis-AI Libray Support.
This has been tested on the Vitis-AI 1.1 relases of the tools, and Vitis 2019.2

## Step 1: Build ZCU106 Vitis Platform
The Vitis zcu106_dpu platform is based off the zcu104_dpu platform.
To Build the Patform:

Setup up Vitis, PetaLinux, and XRT

~~~bash
source <path to Vitis Install>/Vitis/2019.2/settings64.sh
source <path to Petalinux Install>Petalinux/2019.2/settings.sh
source /opt/xilinx/xrt/setup.sh
~~~

~~~bash
cd zcu106_dpu
make
~~~

When the build is complete the xpm file will be located at:

```zcu106_dpu/platform_repo/zcu106_dpu/export/zcu106_dpu/zcu106_dpu.xpfm```

## Step 2 Download DPU_TRD 
We will configure DPU_TRD to the same dpu config as the zcu104 version. It is has been tesetd of the 1.1 Release of Vitis-AI.

To build:
1. Clone the Vitis 1.1 Tag of the Vitis-AI Tools and go the  DPU_TRD directory

2.  Make the following edits to the makeFile in DPU/TRD/prj/makeFile
* Edit the SDX_PLATFORM line to point the zcu106_dpu.xpm file loaction in
* Change XOCC_OPTs to the following:
```XOCC_OPTS = -t ${TARGET} --platform ${SDX_PLATFORM} --save-temps --config ${DIR_PRJ}/config_file/prj_config_104_2dpu --xp param:compiler.userPostSysLinkTcl=${DIR_PRJ}/strip_interconnects.tcl ```

3. Make the follwing edits to DPU/TRD/prj/dpu/dpu_conf.vh

* Change the line with ``` \`define URAM_DISABLE to \`define URAM_Enable```
* Change the line with ``` \`define RAM_USAGE_LOW to `define RAM_USAGE_High```

~~~bash
cd DPU_TRD/prj/Vitis
make KERNEL=DPU DEVICE=zcu106
~~~

When the build completes the sd card files will be loacted at: DPU_TRD/prj/Vits/binary_contrainer_1/sd_card/

## Step 3 Prepare SD Card 
Run the following flash utility to create an .img file:

~~~bash
cp flash_sd_card.sh DPU-TRD/prj/Vitis/binary_container_1/sd_card/
cd DPU-TRD/prj/Vitis/binary_container_1/sd_card/
sudo bash flash_sd_card.sh
~~~

You select n when asked if you want to Flash the SD card.

The .img file will be created for which you can use a program like etcher to progam the SD Card (See UG1414 for more details)

You are now ready to boot the ZCU106 from the SD Card.

## Step 4 Install Vitis AI Run Time Libary
Follow the Instructionas at: https://github.com/Xilinx/Vitis-AI/tree/v1.1/VART

## Step 5 Install Vitis AI Libraries
Follow the instructions at: https://github.com/Xilinx/Vitis-AI/tree/v1.1/Vitis-AI-Library
* Install ZCU104 Models. 
* Install Vitis-AI LIbrary 1.1
* Install demo vido files
* Install demo image file



