[![hacs_badge](https://img.shields.io/badge/HACS-Default-41BDF5.svg)](https://github.com/hacs/integration)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/V7V51QQOL)

[Octopus.Energy 🐙](https://share.octopus.energy/wise-boar-813) referral code. You get £50 credit for joining and I get £50 credit.

# homeassistant-solax-modbus

**To use Integration 2023.01.x and later you need to be on HA 2023.01.1 or later.**

**For installs on earlier Home Assistant versions please use Integration 2022.12.x**

## 2022.12.1 adds in support for Ginlong Solis & Sofar Solar Inverters, some aspects are still in development.

Universal Solar Inverter over Modbus RS485 / TCP custom_component for Home Assistant

### Support Modbus over RS485 & TCP
**(Pocket LAN / Pocket WiFi v1 & v2 does not provide a Modbus connection in most situations, trouble shooting for Pocket WiFi will not be provided)**

**Pocket WiFi 3.0 with Firmware V3.004.03 and above is only [officially supported](https://kb.solaxpower.com/data/detail/ff8080818407e2a701840a22dec20032.html). SolaX only mention Gen4 Hybrid, other Inverter may work?**

(⚠I still don't recomend the PocketWiFi. If you loose all entites after normal operation, try power cycling your Inverter.

Another approach to fixing previously working PocketWiFi installs is to restart your rooter and then reload the integration in Home Assistant. If that doesn't work you can unplug PocketWifi usb for 30 seconds and plug it in again, then reload the integration. 

Updating / downgrading the Integration or Home Assitant wont help, you have lost the Internal Modbus connection between the Inverter and PocketWiFi and I am unable to assist with this issue.⚠)

**Please check the Wiki for [Compatible RS485 Adaptors](https://github.com/wills106/homeassistant-solax-modbus/wiki/Compatible-RS485-Adaptors)**

You can have multiple instances of this Integration, just change the default Prefix from SolaX to something else. Ie SolaX Main or SolaX Southwest

Supports:

Ginlong Solis
- RHI-nK-48ES-5G Single Phase (lowercase n indicates Inverter size, ie 6kW)
- RHI-3PnK-HVES-5G Three Phase (lowercase n indicates Inverter size, ie 10kW)
- Unknown Models

Growatt:

Untested Models:

 - AC Battery Storage:
   - SPA
  
 - Hybrid:
   - SPH - WIP
   - TL-XH - WIP
  
 - PV Only:
   - MAC
   - MAX
   - MID
   - TL-X


Sofar Solar
- HYDxxKTL-3P (plugin_sofar)
- HYDxxxxES (plugin_sofar_old)
- Unknown Models

SolaX Power
- Gen2 Hybrid
- Gen3 AC, Hybrid & Retrofit
- Gen4 Hybrid
  - Qcells Q.VOLT HYB-G3-3P
- X3 MIC / MIC PRO (Limited set of entities available)

Indepth Features

- <details><summary>SolaX Gen2, Gen3 & Gen4</summary>

  - Charge / Discharge rate of battery
  - Charger Start / End times
  - Charger Use Mode
</details>

- <details><summary>SolaX Gen2 & Gen3</summary>

  - Allow Grid Charge
  - Charge periods in Force Time Use Mode
  - Force Discharge into Grid
  - Min Battery Capacity
</details>

- <details><summary>SolaX Gen2, Gen3 & Gen4</summary>

  - Sensors support the "Energy" Dashboard in 2021.08.x and onwards.
</details>

- <details><summary>SolaX Gen3</summary>

  - ForceTime Period 1 Max Capacity
  - ForceTime Period 2 Max Capacity
</details>

- <details><summary>SolaX Gen4</summary>

  - Backup Discharge Min SOC
  - Backup Nightcharge Upper SOC
  - Charge / Discharge rate of battery
  - Charge and Discharge Period2 Enable
  - Discharger Start / End time
  - Export Control User Limit
  - Feedin Discharge Min SOC
  - Feedin Nightcharge Upper SOC
  - Manual Mode Select
  - Selfuse Discharge Min SOC
  - Selfuse Night Charge Enable
  - Selfuse Nightcharge Upper SOC
</details>


Untested:
Non Hybrid Models ie Solar PV only

# Documentation

For further Documentation please refer to the [Wiki](https://github.com/wills106/homsassistant-solax-modbus/wiki)

Some of the topics that will be covered (WIP):

- Gen2 & Gen3 Modes of Operation
- Gen4 Modes of Operation
- Gen4 Connection methods

# Installation

<B>Preferred Option</B>

You can add this custom_component directly through HACS, if you have HACS installed on your Home Assistant instance.

<B>Alternatively</B>

Download the zip / tar.gz from the release page.
- Extract the contents of solax_modbus into to your home-assistant config/custom_components folder.

If you manually clone the repository you may end up mid code update!

~~Copy the folder and contents of solax_modbus into to your home-assistant config/custom_components folder.~~


<B>Post Installation</B>

After reboot of Home-Assistant, this integration can be configured through the integration setup UI

From 0.6.0 the Configuration is split into separate views.

<img src="https://user-images.githubusercontent.com/18155231/200254318-265189d5-34e2-459e-9933-cdb05c05977b.png" width=40% height=40%>

TCP

<img src="https://user-images.githubusercontent.com/18155231/182889165-2b304b6d-f548-4551-a34c-d190ff510992.png" width=40% height=40%>

Serial

<img src="https://user-images.githubusercontent.com/18155231/182894989-e9767f7b-6c5e-482d-bc6e-8c2c7b8f9445.png" width=40% height=40%>



Any manual updates / HACS updates require a restart of Home Assistant to take effect.
- Any major changes might require deleting the Integration from the Integration page and adding again. If you name the Integration exactly the same including the Area if set, you should retain the same entity naming bar any name changes in the release. (Refer to the release notes for any naming change)

If the Integration fails to load with the following error in your log "unrecognized inverter type - serial number : {your_serial_number_here}"
Please see [Discussion #26](https://github.com/wills106/homsassistant-solax-modbus/discussions/26) providing the details asked for.

<B>Inverter never connected to the Cloud / possibly after a firmware update: (SolaX only)</B>
- If you can read values, but unable to adjust select / number you need to press the "Unlock Inverter" button.
- Might need performing again following a full Power Cycle

# Known Issues

1. You can only have one connection to the inverter, so you can't use this and one of my yaml [packages](https://github.com/wills106/homeassistant-config/tree/master/packages) at the same time for writing to registers.
2. Possible Warnings about blocking call in the event loop (in systems with serial modbus connection).

You can add the following lines to your configuration file:
```
logger:
  default: warning
  logs:
    homeassistant.util.async_: error
```
3. Please check the Todo List under discussions for other known issues and what's being worked on.
4. If your Inverter is asleep do not start this integration / restart HA as you will get the following error **"Modbus Error: [Connection] Failed to connect[Modbus"** You can't establish a connection if there is nothing to connect to.

# FAQ
1. How do I enable a disabled entitie?

Example given is for "Config Max Export"
>Go to the SolaX Integration page.
>
>Find "+xy entities not shown"
>
>Click those till you find "Config Max Export"
>
>Then Cog/Gear icon.
>
>There is an option to enable it and press "UPDATE"
