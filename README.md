<div id="top"></div>
<!-- Note: https://www.markdownguide.org/basic-syntax/#reference-style-links -->

<!-- PROJECT SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/ericdmoore/solar-pi">
    <img src="images/SolarPi.svg" alt="Solar Pi Project Logo" width="155" height="155" />
  </a>

<h3 align="center">Solar Pi</h3>

  <p align="center">
    Monitor Your Solar Power Equipment using a Raspberry Pi + other sensors.
    <br />
    <a href="https://github.com/ericdmoore/solar-pi"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/ericdmoore/solar-pi">View Demo</a> |
    <a href="https://github.com/ericdmoore/solar-pi/wiki">Review Wiki</a> |
    <a href="https://github.com/ericdmoore/solar-pi/issues">Request Feature</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#what-it-does">What It Does</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#architecture-overview">Architecture Overview</a></li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <!-- <li><a href="#acknowledgments">Acknowledgments</a></li> -->
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

[![Product Name Screen Shot][product-screenshot]](https://solarpi.link/u/edm)

### What It Does:

Senses, Saves, & Shows Solar Power Time-series Metrics via a web interface

- MPPT / Battery 
  - Voltage
  - State of Charge %
  - Lifetime Cycles
  - Lifetime kWH output
- Panels (Before MPPT)
  - Voltages
  - Amp
  - Power
- Load
  - instant Power Consumption (Watts)
  - Historical Net-Power Consumption/Generation
- Environmental
  - Indoor Temp
  - Outdoor Temp
  - Light/Irridescence Score
- Lifetime Solar Power Generated
- Total solar PV generation
- Total current, voltage, power, and power factor values
- See Victron Stats

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

This project is a collection of components that make up a larger ecosystem. You may you use them independently, or collecively. TFor Example, there is software to run on a raspberry pi, or perhaps you have a sensor network already deployed, then look at the cloud-based software to render the data on screen.

### Prerequisites

This is an electronics project. As such there are some hardware requirements for this project. You can buy what I bought ( the link might be to what I whish I bought) in the links below (note: they are  affiliate links).

Or perhaps you already have a solar power system and just need monitoring via `Solar-Pi`, Great! then you just need to look at the sections starting with installing your sensors.

### Installation

Secton Coming Soon!

- Raspberry Pi Install
- Cloud Install
- Alexa Install

### Architecture Overview

Basic(Unmonitored) Hardware / Power Connections:

```mermaid
  graph TD;
      Panels(Solar Panels)-->cb1(Circuit Breaker)
      cb1(Circuit Breaker)-->MPPT
      MPPT-->Batteries
      Batteries-->LVD(Low Voltage Disconnect)
      LVD-->cb2(Circuit Breaker)
      cb2(Circuit Breaker)-->24vFuseBox/Load
      LVD-->VReg(Voltage Regulator)
      VReg-->cb3(Circuit Breaker)
      cb3(Circuit Breaker)-->12vFuseBox/Load
```

#### Basic Software Components

```mermaid
  graph TD;
      subgraph sensors [Sensor Layer]
        Sensor1(Temperature)-->HWIntf
        Sensor2(Voltage)-->HWIntf
        Sensor3(Amps)-->HWIntf
        Sensor4(Humidity)-->HWIntf
        Sensor5(Light)-->HWIntf
        Sensor6(Motion)-->HWIntf
        Sensor7(UV)-->HWIntf
        Sensor8(Wind)-->HWIntf
        Sensor9(etc)-->HWIntf 
        HWIntf(Pi HW Interface)-->solDataWrangle(Solar Pi Data Wrangling)
      end

      subgraph data [Data Layer]
        solDataWrangle==>solLocalStorage[(Solar Pi HDD/SQLite)]
        solLocalStorage-. B: Do A or B -.->solCloudStorage
        solDataWrangle-. A: Do A or B -.->solCloudStorage[(Solar Pi Cloud Storage)]
      end

      subgraph api [API Layer]
        solLocalStorage-- Only On Local Network   ---ApiIntf1(λ:HTTP/WS/JSON Data API)
        solCloudStorage==>ApiIntf1
        solLocalStorage-- Only On Local Network ---ApiIntf2(λ:MQQT Data API)
        solCloudStorage==>ApiIntf2
        solLocalStorage-- Only On Local Network ---ApiIntfN(...Other Data λ:APIs)
        solCloudStorage==>ApiIntfN
      end

      subgraph apps [Application Layer]
        ApiIntf1==>solVizApp(Solar Pi Data Viz Application)
        ApiIntf1-.->alexaApp(WIP: Alexa Integration)
        ApiIntf2-.->homeAssistant(WIP: Home Assistant Integration)
        ApiIntfN-.->zmq(WIP: ZeroMQ Integration, etc)
      end

      subgraph notifications [Notification Layer]
        ApiIntf1-->email(Email Notifications)
        ApiIntf1-->slack(Slack Notifications)
        ApiIntf1-->rss(Rss Alert Log)
      end  
```

<p align="right">(<a href="#top">back to top</a>)</p>

### Proposed Bill Of Materials
- Power Components
  - [Solar Panels](//amzn.to/3IFrQN2) - Creates the power from the Sun but your voltage and power can be all over the place
  - [MPPT](//amzn.to/3AVXMLm) - This boxes "resizes the power" to useful levels that make your battery happy - and keep it at a healthy charge (EPEVER: Supported)
  - [MPTT Data Cord](//amzn.to/3PER3JJ) - Send your MPPT data to the SolarPi over USB
  - [Batteries (Recommended 2x for a 24 system)](//amzn.to/3PzTaP8) - holds the power for later, duh
  - [Voltage Regulators](//amzn.to/3yNEipz) - Takes a noisy 24volts down to a VERY stable 12v
  - [Raspberry Pi](//amzn.to/3yJGrCt) - Hopefully you already have one - because prices seem silly right now.
  - [Low Voltage Disconnect](//amzn.to/3PqLwpS) - So that you dont run your batteries down too far
  - [DC Circuit Breakers](//amzn.to/3uRjqfY) - to isolate parts of your circuit
  - [Fusebox For Load](//amzn.to/3Od4Y8O) - to keep things isolated, safe, and tidy
- Your Stuff that Needs Power (DC)
  - [Lights](//amzn.to/3yNEipz) - Because if you learn one thing from Matt From [DIY Perks](//www.youtube.com/playlist?list=PLOJU8YJjFwGN0hMRewz2_u2IefV-vipsk) - it's that LED strips are incredible, and how is instantly imporved vision not at the top of your list?
  - [PhotoCell Switch](//amzn.to/3z6KE4C) - Because some stuff only runs at night, & you have better stuff to do than to flip them on/off EVERYDAY
  - [USB Sockets](//amzn.to/3RFZ6rm) - Because sometimes your phone has a low battery
  - [Water Pumps](//amzn.to/3Pt5ZKX) - Because sometimes you want to start a green house with all this stuff
  - [12v Digital Timer](//amzn.to/3aFPw7g) - Because you are not a knuckle dragger who wants to turn the pump on and off EVERYDAY for the rest of your life
  - [Space Fans](//amzn.to/3RQ7FQP) - Because sometimes you're hot
  - [Vent Fans](//amzn.to/3OkwFwi) - Because batteries prefer roughly the same temps as people
  - [Thermal Relays](//amzn.to/3chmFXo) - Because you don't want to turn your vent fans on when it get's too hot, EVERYDAY for the rest of your life
  - [AC Inverter](//amzn.to/3AXgKkA) - Because some stuff just needs AC power. And 2000 Watts w/ 2 outlets is almost always enough.
  - [DC Garage Door Openers](#)
  - [DC Ceiling Fans](#)
  - [POE based Home Security](#)
  - [Sprinkler Systems](#)
- Sensors
  - [Current Sensor](//www.sparkfun.com/products/164080)
  - [Voltage Sensor](//www.sparkfun.com/products/16408)
  - [Humidity Sensor](//www.adafruit.com/product/3721)
  - [Temperature Sensors](//www.adafruit.com/product/1782)
  - [Light Sensors](//www.adafruit.com/product/1980)
  - [UV Light Sensor](//www.adafruit.com/product/4831)
  - [Motion Sensor](//www.adafruit.com/product/189)
- Tools & Misc
  - [Large Gauge Wire](//amzn.to/3yRpaYc)
  - [Wire Lug Terminals w/ Heat Shrink](//amzn.to/3yRYAOu)
  - [Wire Lug Terminal Crimpers + Cutters](//amzn.to/3RG40Vn)
  - [MC4 Crimpers](//amzn.to/3AZva3U)
  - [MC4 Connectors](//amzn.to/3yRYoyD) 
  - [Automatic Wire Strippers](//amzn.to/3oeLKoO)

<!-- USAGE EXAMPLES -->
## Usage

1. Buy Hardware.
2. Connect the parts.
3. Configure Solar-Pi.
4. Sun Handles Everything Else From There (TSTCOEFT) SHEEFT
  (except the internet conection)
5. Energy Independence: Checked
5. Prepper Status: Checked

For more examples, please refer to the [Documentation](//solarpi.link/u/edm)
<p align="right">(<a href="#top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap
- [Configuration](//github.com/ericdmoore/solar-pi/wiki/Configuration) Model per Node
  - What would a MultiNode Setup do/look like?
- Support via Data Source for Grafana/Prometheus
- Alerts Log
  - Temperature Battery Protection
    - Low Voltage Charging Protection
    - High Temp Discharging
  - [Notifications](//github.com/ericdmoore/solar-pi/wiki/Notifications)
    - [Email Notification](https://github.com/ericdmoore/solar-pi/wiki/Notifications:Email)
    - [Slack Notifications](https://github.com/ericdmoore/solar-pi/wiki/Notifications:Slack)
    - [SMS Notifications](https://github.com/ericdmoore/solar-pi/wiki/Notifications:SMS)
    - RSS Feed?
- Relay Drivers for Terraced Batteries
  - aka: Water Heating with "Excess Energy"
  - "Battery" Interface
    - Status/Progress Signal
    - Fill Circuit
- Direct Support For Other Vendors
  - Explore Renogy Charge Controllers
- Amazon Alexa Integration
- Alternative Network Interfaces
  - LoRa
  - 3G / 5G Cellular
  - BLE ([access in browser is currently experimental](https://developer.mozilla.org/en-US/docs/Web/API/Bluetooth/getDevices))
- Alternative Data API Flavors
  - Raw Streaming
    - Websocket/JSONND
    - MQTT
    - ZMQ
  - Filtered Streaming
    - Registered Query - then subscribed for results
  - Query
    - GraphQL
    - RESTful (HTTP/JSON.GZ)
- Explore `NodeRed` Options for User Friendly Programming
- Explore Solar Tracker Motor Control

See the [open issues](https://github.com/ericdmoore/solar-pi/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>

## Inspirations
- [RPI Power Meter](//github.com/David00/rpi-power-monitor)
- [Powerwall Monitor](//github.com/mihailescu2m/powerwall_monitor)
- [Solarpi](https://github.com/Tafkas/solarpi)
- [Solar Thing](https://github.com/wildmountainfarms/solarthing)
- [Solar Observatory](https://github.com/petercable/solar-observatory)
- [Solar PV Monitoring](https://github.com/lewei50/Solar-PV-Monitoring)
- [Solarshed](https://github.com/corbinbs/solarshed)
- [Olen/solar-monitor](https://github.com/Olen/solar-monitor)
- [EPEVER](https://github.com/glitterkitty/EpEverSolarMonitor)
- [Solar Mon](https://github.com/Elwell/solarmon)
- [Solar Energy Grafana](https://github.com/glfp/SolarEnergyMonitorInfluxGrafanaDocker)
- [EPEVER Modbus](https://github.com/rosswarren/epevermodbus)
- [Solar Log](https://github.com/lewismoten/solar-log)

<!-- LICENSE -->
## License

Distributed under the MIT License. See [`LICENSE`](./LICENSE) for more information.

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

Eric D Moore - [@ericdmoore](https://twitter.com/ericdmoore) - eric [𝓪𝓽] mooore [𝓭𝓸𝓽] cc

Project Link: [https://github.com/ericdmoore/solar-pi][project-url]

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
<!-- ## Acknowledgments -->

<!-- * []() -->
<!-- * []() -->
<!-- * []() -->

<!-- <p align="right">(<a href="#top">back to top</a>)</p> -->

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[project-url]:https://github.com/ericdmoore/solar-pi
[contributors-shield]: https://img.shields.io/github/contributors/ericdmoore/solar-pi.svg?style=for-the-badge
[contributors-url]: https://github.com/ericdmoore/solar-pi/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/ericdmoore/solar-pi.svg?style=for-the-badge
[forks-url]: https://github.com/ericdmoore/solar-pi/network/members
[stars-shield]: https://img.shields.io/github/stars/ericdmoore/solar-pi.svg?style=for-the-badge
[stars-url]: https://github.com/ericdmoore/solar-pi/stargazers
[issues-shield]: https://img.shields.io/github/issues/ericdmoore/solar-pi.svg?style=for-the-badge
[issues-url]: https://github.com/ericdmoore/solar-pi/issues
[license-shield]: https://img.shields.io/github/license/ericdmoore/solar-pi.svg?style=for-the-badge
[license-url]: https://github.com/ericdmoore/solar-pi/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/ericmoore

[product-screenshot]: https://i.redd.it/09q1rdtno2241.png 
<!-- The product-screen shot is still a placeholders screenshot -->
