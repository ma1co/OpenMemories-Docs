Hardware
========
This is a collection of information about Sony's camera hardware.
In some places, it may differ from what you can find elsewhere.
However, we believe our information to be correct, as countless hours have been spent disassembling firmware and analyzing service manuals.

BIONZ Image Processor
---------------------
This system-on-chip (SoC) is the heart of the camera.
On its ARM cores, it runs both Linux and the AV real time operating system (µITRON RTOS).

+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| Chip     | CPU Cores         | Name              | Released  | Cameras                                                  |
+==========+===================+===================+===========+==========================================================+
| CXD4105  | 2x ARM926EJ       | Arex              | Jul. 2006 | DSC-G1, HDR-SR1, HDR-UX1, ...                            |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD4108  | 2x ARM926EJ       | Prius             | Feb. 2007 | DSC-T100, DSC-G3, ...                                    |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| MB8AC102 | 2x ARM926EJ       | Gaia              | Jan. 2009 | DCR-SR67, DCR-SX60, DCR-DVD850, ...                      |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD4115  | 2x ARM11 MPCore   | Ortus / Elvis?    | Feb. 2009 | DSC-T90, DSC-HX5, NEX-3, SLT-A33, ...                    |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD4120  | 3x ARM11 MPCore   | Baccarat          | Jul. 2009 | HDR-AX2000, HXR-NX5, ...                                 |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD4132  | 3x ARM11 MPCore   | Opal / Avip?      | Jan. 2011 | DSC-TX100, DSC-RX100, NEX-6, ...                         |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD90014 | 4x ARM Cortex-A5  | Musashi / Kojiro? | Oct. 2013 | ILCE-7, ILCE-6000, DSC-RX100M3, ILCE-7M2, ILCE-6500, ... |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD90045 | 4x ARM Cortex-A5  | Astra             | Apr. 2017 | ILCE-9, ILCE-7M3, DSC-RX100M6, ILCE-6600, ...            |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+
| CXD90057 | 4x ARM Cortex-A35 | Mira              | Jul. 2020 | ILCE-7SM3, ...                                           |
+----------+-------------------+-------------------+-----------+----------------------------------------------------------+

CXD90014 and CXD90045 are called "BIONZ X" by Sony, CXD90057 is called "BIONZ XR".

The DRAM and NAND flash are usually combined in the same package (multi-chip package, MCP) and then stacked on top of the SoC (package-on-package, PoP).
This is why the SoC package itself is never visible in disassembly photos.

SA DSP
^^^^^^
The SA DSP core is included on-chip and shares memory with the ARM cores.
It executes "sabin" programs.
This seems to be a 32-bit CPU architecture featuring 16 registers and a custom instruction set.

Power IC
--------
The "power IC" is responsible for starting up the BIONZ processor.
It also manages the real time clock.

+-------------+----------+--------------------+------------------------+
| Chip        | Name     | Programmable Core  | Cameras                |
+=============+==========+====================+========================+
| MB89083LGA  |          |                    | DSC-T100, ...          |
+-------------+----------+--------------------+------------------------+
| MB95005ABGL | Charon   |                    | DSC-HX1, ...           |
+-------------+----------+--------------------+------------------------+
| SC901572VOR |          |                    | DSC-G3, ...            |
+-------------+----------+--------------------+------------------------+
| AN30230AAVB | Gordon   |                    | DSC-HX5, ...           |
+-------------+----------+--------------------+------------------------+
| 19A44FDAXBG | CA       | TX19A MIPS core    | NEX-3, SLT-A33, ...    |
+-------------+----------+--------------------+------------------------+
| MB44C031PW  | Hibari   |                    | DSC-RX100, ...         |
+-------------+----------+--------------------+------------------------+
| BU76381GUW  | Piroshki |                    | DSC-RX100, ...         |
+-------------+----------+--------------------+------------------------+
| MB9AF004BGL | Darwin   | ARM Cortex-M3 core | ILCE-7, ILCE-6000, ... |
+-------------+----------+--------------------+------------------------+

Hibari and Piroshki are used together in some cases.

Front-End (DFE)
---------------
The DFE is optional and is placed between the image sensor and the SoC.

+----------+---------+-------------------------+
| Chip     | Name    | Cameras                 |
+==========+=========+=========================+
| CXD9974  | Pegasus | NEX-3, SLT-A33, ...     |
+----------+---------+-------------------------+
| CXD90009 | Sirius  | NEX-FS700, PXW-FS5, ... |
+----------+---------+-------------------------+
| CXD90016 | Mobius  | DSC-RX1, NEX-6, ...     |
+----------+---------+-------------------------+
| CXD90027 | Regulus | ILCE-7, ILCE-6000, ...  |
+----------+---------+-------------------------+
| CXD900?? | Leo     | ILCE-7M3, ...           |
+----------+---------+-------------------------+

Starting with CXD90016, the DFE has an integrated ARM core (firmware in dfe.bin).

In some cases, the DFE has a private DRAM.
With Leo, the DRAM is even stacked on top (PoP).

Note: The RX10M2 includes a DFE (CXD90027), while the RX100M4 does not.
However, they share the same image sensor and almost exactly the same capabilities, except for thermals.

Codec
-----
The optional codec IC is used to boost the SoC's video capabilities.

+---------+--------+-----------------------------+
| Chip    | Name   | Cameras                     |
+=========+========+=============================+
| CXD4113 | Annecy | DSC-HX5, SLT-A33, ...       |
+---------+--------+-----------------------------+
| CXD4236 | Beaune | DSC-RX100M4, ILCE-7RM2, ... |
+---------+--------+-----------------------------+

These chips contain MIPS CPUs and software is developed using eSOL eBinder.
There are always two binaries: "ipl" is the loader, which decompresses the actual application contained in the second binary.

The following SoC / codec combinations exist:

+-------------------+-----------------+
| SoC / Codec       | Max. Resolution |
+===================+=================+
| CXD4108           | 480p (30fps)    |
+-------------------+-----------------+
| CXD4115           | 720p (30fps)    |
+-------------------+-----------------+
| CXD4115 + Annecy  | 1080p (30fps)   |
+-------------------+-----------------+
| CXD4132           | 1080p (60fps)   |
+-------------------+-----------------+
| CXD90014          | 1080p (60fps)   |
+-------------------+-----------------+
| CXD90014 + Beaune | 4K (30fps)      |
+-------------------+-----------------+

On the CXD90045, Beaune seems to be always present according to the firmware, but does not show up in the schematic, so it might be on-chip or in-package.

Image Stabilization
-------------------
+------------+--------+-------------------+--------------------------+
| Chip       | Name   | Programmable Core | Cameras                  |
+============+========+===================+==========================+
| MB91F233LA | AS     | Fujitsu FR core   | SLT-A33, ...             |
+------------+--------+-------------------+--------------------------+
| R2J30503LG | Bonobo | Renesas H8 core   | DSC-RX100, ILCE-7M2, ... |
+------------+--------+-------------------+--------------------------+

HDMI Processor
--------------
+-----------+-------+-----------------------+--------------+
| Chip      | Name  | Details               | Cameras      |
+===========+=======+=======================+==============+
| XC6SLX25T | Furud | Xilinx Spartan-6 FPGA | PXW-FS5, ... |
+-----------+-------+-----------------------+--------------+

Genlock
-------
+-------------+---------+--------------------+------------------------+
| Chip        | Name    | Programmable Core  | Cameras                |
+=============+=========+====================+========================+
| STM32F031E6 | Genlock | ARM Cortex-M0 core | DSC-RX0, ILCE-7M3, ... |
+-------------+---------+--------------------+------------------------+
