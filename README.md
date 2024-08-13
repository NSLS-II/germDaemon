This repository contains an EPICS IOC for Germanium detector control and UDP data receiving. The Phoebus screen for the controller is in `opi/` directory.

This repository was derived from [https://github.com/solofrain/germ_agent](https://github.com/solofrain/germ_agent).

# 1. Build

The software was created under EPICS framework in Linux, and configured per NSLS2 environment. To build on non-NSLS2 computers, redefine the location of EPICS base (`EPICS_BASE`) in `configure/RELEASE`.

# 2. Install

- Define beamline specific parameters (e.g., network environment, PV prefix, and number of detector pixels (96, 192, or 384)) in `iocBoot/iocgermDaemon/unique.cmd`. Load this `unique.cmd` in `iocBoot/iocgermDaemon/st.cmd`.

- This software can be installed like a regular EPICS IOC using `systemd-softioc`/`manage-iocs` package. Refer to [https://github.com/NSLS-II/systemd-softioc](https://github.com/NSLS-II/systemd-softioc) for details on how to install the package and the IOC.

- The Phoebus screen `opi/GeRM.bob` is the user interface to the detector, for which the following macros must be defined:

  - `$(Sys)`. It should be identical to the value defined in `iocBoot/iocgermDaemon/unique.cmd`.
    
  - `$(Dev)`. It should be identical to the value defined in `iocBoot/iocgermDaemon/unique.cmd`.
  
  - `$(NELM)`. It depends on the number of detector pixels: 96, 192, or 384.
  
  See `opi/Open-GeRM.bob` for examples.

# 3. Use

The detector can be configured and calibrated on the Phoebus screen. In addition to that, it also provides interfaces to enable/disable test pulses, enable/disable channels, and set UDP related parameters.

## 3.1 Enable/disable test pulses

Turning on/off the test pulses is done by changing `$(Sys)$(Dev).TSEN`. From Tab2 of the screen, choose `Enable`, `Enable All`, `Disable`, or `Disable All` for `Test Pulses`, then click `Set`, the LED willl be lit. Once `$(Sys)$(Dev).TSEN` is set by the UDP daemon, the LED will be off.

## 3.2 Enable/disable channels

Turning on/off the channels is done by changing `$(Sys)$(Dev).CHEN`. From Tab2 of the screen, choose `Enable`, `Enable All`, `Disable`, or `Disable All` under `Channels`, then click `Set`, the LED willl be lit. Once `$(Sys)$(Dev).CHEN` is set by the UDP daemon, the LED will be off.
- Assign values to the following PVs to use UDP data streaming:

## 3.3 UDP

  - `$(Sys)$(Dev).IPADDR`
    
    IP address of the UDP data connection.
  
  - `$(Sys)$(Dev):FNAM`
    
    Name of data file, include the directory where the data files are to be saved. The UDP daemon will append run number and segment number to construct the full names of data files.

  - `$(Sys)$(Dev):FSIZ`

    Maximum size of a UDP data file.

Make sure the UDP daemon is running before starting counting. The status is indicated by the `UDP` LED at the bottom of the screen.

