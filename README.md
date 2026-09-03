# CoreLight

CoreLight is an open-source DIY lighting controller built around an STM32H743.

The project is designed to manage stage lighting systems with up to **13 DMX universes**, Ethernet protocols such as **Art-Net and sACN**, timecode synchronization, show playback and system monitoring.

The goal is not to recreate a grandMA3 console. CoreLight is intended to be a hardware platform for a future open-source lighting-control system.

## Main features

- Up to 13 DMX outputs
- DMX512 and RS-485 communication
- Art-Net and sACN support
- Planned RDM support
- LTC and MTC timecode
- Show recording and playback
- Standalone operation
- DMX routing and merging
- Failover and redundancy
- Ethernet and OSC control
- Internal web interface
- Temperature, power and fan monitoring
- MicroSD and Flash storage
- USB-C for programming and updates
- RTC with battery backup
- Custom 4-layer PCB
- 3D-printed rack-mount enclosure

Most of these features are still under development. The current version should be considered a hardware prototype rather than a finished lighting controller.

---

## Architecture

The STM32H743 is the central processor of the system. It handles network communication, timecode, DMX processing, show playback and system monitoring.

The controller can also receive LTC, MTC and external event inputs. Lighting data is processed by the STM32 and sent to the required DMX outputs.

---

## Hardware

The current design uses the following main components:

| Component | Function |
|---|---|
| **STM32H743ZIT6** | Main microcontroller |
| **W5500** | Ethernet interface |
| **MAX3485** | DMX / RS-485 transceivers |
| **CD74HC4067M** | Signal routing and multiplexing |
| **IS42S16400J-7TL** | External SDRAM |
| **W25Q128JVSIQ** | SPI Flash |
| **DS3231M+** | Real-time clock |
| **INA226AIDGST** | Power monitoring |
| **TMP117AIDRVR** | Temperature monitoring |
| **SM712** | DMX transient protection |
| **USBLC6-2SC6** | USB ESD protection |

The complete component list is available in [BOM.csv](BOM.csv).

---

## Processing and memory

The STM32H743 was selected because the project requires more processing power and memory bandwidth than a basic microcontroller-based DMX interface.

It is responsible for:

- Art-Net, sACN and OSC communication
- DMX universe management
- RDM and DMX monitoring
- Show playback
- Timecode processing
- Failover logic
- Fan and thermal management
- System logs and diagnostics

### External SDRAM

The external SDRAM is used for temporary runtime data such as:

- DMX buffers
- Network packets
- Show playback data
- Large processing structures

### SPI Flash

The W25Q128JVSIQ Flash can be used for firmware, configuration and persistent system data.

### MicroSD card

The microSD card is intended for user-accessible files:

```text
/show
/config
/logs
/firmware
```

Shows, configuration files, logs and firmware updates can be stored on the card.

---

## DMX

CoreLight is designed to support up to **13 DMX universes**.

Each output uses a MAX3485 RS-485 transceiver and includes transient protection. A DMX universe contains up to 512 channels, giving a theoretical total of:

\[
13 \times 512 = 6656 \text{ DMX channels}
\]

Standard DMX wiring is used

The DMX data rate is 250 kbit/s.

Planned DMX features include:

- Flexible universe routing
- DMX repeater mode
- RDM
- DMX monitoring
- Signal-loss detection
- DMX merging and failover

For example, one network universe could be sent to several physical outputs

---

## Art-Net, sACN and Ethernet

Ethernet communication is handled by the W5500.

The network interface is intended to support:

- Art-Net
- sACN
- OSC
- Web-based configuration and monitoring

Network universes can be mapped to any of the physical DMX outputs instead of being permanently assigned to a single port.

---

## Timecode

CoreLight includes an LTC input and is designed to support MTC.

Timecode can be used to trigger lighting events or control show playback:

Example events could include scene changes, effect triggers or the start of a complete lighting sequence.

The RTC is used for system time, logs and scheduled events. It is separate from LTC, which is used for show synchronization.

---

## Standalone playback

A show can be uploaded to the microSD card and played back without a permanent computer connection.

A show file may contain:

- Scenes
- Cues
- DMX values
- Timing information
- Events
- Timecode positions

During playback, the STM32 loads the required data and generates the DMX output in real time.

---

## Failover and DMX merging

One of the main objectives is to improve system reliability during shows.

CoreLight is intended to detect problems such as:

- Loss of network data
- Software or hardware failure
- Missing DMX input
- Communication errors

Depending on the final firmware, the controller could switch to a backup source or continue playback using a locally stored show.

DMX merging is also planned, allowing several sources to contribute to the same output:

The merging rules and failover behavior are still being designed.

---

## Monitoring and cooling

The system includes temperature and power monitoring using a TMP117 and an INA226.

The firmware will be able to control the cooling fan and warn the user in case of excessive temperature or abnormal power consumption.

Available hardware includes:

- Temperature sensor
- Power monitor
- Fan output
- Status LEDs
- Buzzer

---

## Power supply

The controller is designed for a 24 V DC input.

The regulated voltages power the STM32, Ethernet interface, DMX transceivers, sensors and other logic circuits.

---

## User interface and web interface

The initial local interface consists of:

- Rotary encoder
- Push buttons
- Status LEDs
- Buzzer

A future web interface will provide access to:

- System status
- DMX monitoring
- Network settings
- Show management
- Logs
- Temperature and power data
- Firmware updates

The controller is intended to be configurable from a browser without requiring a dedicated screen.

---

## PCB and enclosure

The PCB was designed in KiCad and uses four layers to simplify routing and provide dedicated ground and power planes.

The board contains:

- STM32H743
- External SDRAM
- SPI Flash
- Ethernet
- 13 DMX interfaces
- LTC input
- USB-C
- MicroSD
- RTC
- Power regulation
- Monitoring circuits
- User interface connections

The current design contains approximately 192 footprints.

The enclosure is a custom 3D-printed rack-mount case with openings for the DMX outputs, Ethernet, USB-C, LTC, power, fan and user controls.

![CoreLight enclosure](Images/image-17.png)

Assembly file: [coreLightAssembly.step](cad/coreLightAssembly.step)

CAD files are available in:

```text
/cad
```

The complete KiCad schematic is available in:

```text
/scheme
```

---

## Estimated cost

The current estimate is:

```text
Components:  $66.01
PCB:         $36.99
Total:      ~$103 USD
```

This does not include shipping, assembly, enclosure parts or future design changes.

---

## Project journal

The development process is documented in [JOURNAL.md](JOURNAL.md).

It includes the PCB design, STM32CubeMX configuration, KiCad work, enclosure design, BOM creation and the problems encountered during development.

---

## Inspiration

CoreLight was inspired by professional lighting hardware such as the grandMA3 Processing Unit and Replay Unit.

The project is independent and is not affiliated with MA Lighting.

- [MA Lighting grandMA3 Processing Unit](https://www.malighting.com/product/grandma3-processing-unit-m-4010510/)
- [MA Lighting grandMA3 Replay Unit](https://www.malighting.com/product/grandma3-replay-unit-4010507/)

---
