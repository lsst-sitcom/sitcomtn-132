
.. note::

   **This technote is a work-in-progress.**

##############################
TopBox Controller Improvements
##############################

Abstract
==========

This document describes the changes made by the NOIRLab team in the original Shutter Control Box (SCB) software.

Introduction
================

The NOIRLab team was asked to improve the SCB software in oder to avoid the "phantom dome closures", described in the https://jira.lsstcorp.org/browse/OBS-89. This document describes the improvements to the software.

Software Updates
================================================

New Control Software Commands
-------------------------------------

**MO**

The MO (**M** aintenance **O** n) command enables the Maintenance Mode.

**MF**

The MF (**M** aintenance O **F** f) command disables the Maintenance Mode.

**d OVE**

The OVE (**OV** erride **E** nable) command enables the override number “d”. The number “d” is defined as an 8-bit unsigned integer (U8). Check the List Of Override commands for more information.

**d OVD**

The OVD (**OV** erride **D** isable) command disables the override number “d”. The number “d” is defined as an 8-bit unsigned integer (U8). Check the List Of Override commands for more information.

**List Of Override**

* 0: stop-button-active
* 1: general-interlock-active
* 2: rain-snow-active
* 3: communication-fault
* 4: main-open-timeout
* 5: main-close-timeout
* 6: lower-down-timeout
* 7: lower-up-timeout
* 8: main-limits-incoherence
* 9: lower-limits-incoherence
* 10: main-encoder#1-incoherence
* 11: main-encoder#2-incoherence
* 12: lower-encoder#1-incoherence
* 13: lower-encoder#2-incoherence
* 14: main-encoder#1-error
* 15: main-encoder#2-error
* 16: lower-encoder#1-error
* 17: lower-encoder#2-error
* 18: main-opencoil-fault
* 19: main-closecoil-fault
* 20: lower-downcoil-fault
* 21: lower-upcoil-fault
* 22: IO-module1-fault
* 23: IO-module2-fault
* 24: IO-module3-fault
* 25: IO-module4-fault

**OV?**

Issuing the OV? (**OV** erride Status **?** ) command gives the full override status. It is intended for engineering setup purposes only.

**CF**

The CF (**C** lear **F** ault) command clears the faults in the FPGA.

**SY**

The SY (**SY** nc) command starts the Encoder-Limits Synchronization routine. This enables and executes a safe drive loop for synchronizing the encoder reading with the respective limit switch on each process. If this flag is set to TRUE both doors will begin to move. Please wait for SYNC-DONE to get TRUE. The STOP command can abort the process.

New Logging Feature
------------------------

This new feature will log (1) commands sent by either the host computer though telnet communication or the CSC, (2) internal software failures, and (3) system failures, like encoder reading incoherences, limit switch incoherences, timeouts, loss of communications between TopBox and MainBox controllers, etc.
This log can be read offline and is intended to be just for debugging purposes.

The commands will be logged with the `CMD` label before the command, the internal failures will have the `ERROR` label before the error information, and the system failures will have the `FAILURE` label before the data.

This logging information will be saved in the `log.txt` file places in the `/home/log/` directory.
If the log directory is not created, please do:

.. code-block:: bash
   :linenos:
      
      admin@NI-cRIO# mkdir /home/log/
      admin@NI-cRIO# chown -R lvuser:ni /home/log/

See the `Documenteer documentation <https://documenteer.lsst.io/technotes/index.html>`_ for tips on how to write and configure your new technote.
