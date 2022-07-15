<div id="top"></div>

<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->



<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
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
    <img src="images/SolarPi.svg" alt="Solar Pi Project Logo" width="80" height="80" />
  </a>

<h3 align="center">Solar Pi</h3>

  <p align="center">
    Monitor Your Solar Power with a Raspberry Pi
    <br />
    <a href="https://github.com/ericdmoore/solar-pi"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/ericdmoore/solar-pi">View Demo</a>
    ·
    <a href="https://github.com/ericdmoore/solar-pi/issues">Report Bug</a>
    ·
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
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

[![Product Name Screen Shot][product-screenshot]](https://solarpi.link)

### What It Does:

Senses, Saves, and Shows Solar Metrics via web interface

- Lifetime Solar Power Generated

- MPPT / Battery 
  - Voltages
  - State of Charge %
  - Lifetime Charge
  - Lifetime Discharge
- Panels (Before MPPT)
  - Voltages
  - Amp
  - Power
- Load
  - instant Consumption (Watts)
  - Daily Consumption
- Environmental
  - Indoor Temp
  - Outdoor Temp
  - Lum

- Total solar PV generation
- Net home consumption
- Net home generation
- Total current, voltage, power, and power factor values
- Individual current transformer readings
- Harmonics inspection through a built in snapshot/plotting mechanism.


<p align="right">(<a href="#top">back to top</a>)</p>

### Built With

* [Next.js](https://nextjs.org/)
* [React.js](https://reactjs.org/)
* [Svelte](https://svelte.dev/)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Prerequisites

This is an electronics project. As such there are some hardware requirements for this project. You can buy what I bought, or whish I would have boght based on the links below (note: affiliate links).

Or perhaps you already have a setup and - just want to add the monitoring `Solar-Pi`, Great! then you just need to look at the sections starting with installing your sensors.

### Bill Of Materials

- [Solar Panels](https://amzn.to/3IFrQN2) - Creates the power from the Sun but your voltage and power can be all over the place
- [EPEVER MPPT](https://amzn.to/3AVXMLm) - Squashes the Power to levels that make your battery happy - and keep it at a healthy charge  
- [Batteries (Recommended 2x for a 24 system)](https://amzn.to/3PzTaP8) - holds the power for later, duh
- [Voltage Regulators](//amzn.to/3yNEipz) - Takes the 24 +- noise voltage down to a VERY stable 12v
- [Raspberry Pi](//amzn.to/3yJGrCt) - Hopefully you already have one - because prices seem silly right now.
- [Low Voltage Disconnect](//amzn.to/3PqLwpS) - So that you dont run your batteries down too far
- [DC Circuit Breakers](//amzn.to/3uRjqfY) - to isolate parts of your circuit
- [Fusebox For Load](https://amzn.to/3Od4Y8O) - to keep things isolated, safe, and tidy
- Stuff You want to Power
  - [Lights](//amzn.to/3yNEipz) - Because if you learn one thing from Matt From [DIY Perks](//www.youtube.com/playlist?list=PLOJU8YJjFwGN0hMRewz2_u2IefV-vipsk) - it's that LED strips are incredible, and how is instantly imporved vision not at the top of your list?
  - [USB Sockets](//amzn.to/3RFZ6rm) - Because sometimes your phone is low
  - [Water Pumps](//amzn.to/3Pt5ZKX) - Because sometimes you want to start a green house with all this stuff
  - [12v Digital Timer](//amzn.to/3aFPw7g) - Because you are not a knuckle dragger who wants to turn the pump on and off EVERYDAY for the rest of your life
  - [Fans](//amzn.to/3RQ7FQP) - Because sometimes it's hot
  - [Thermal Relays](//amzn.to/3chmFXo) - Because you don't want to turn your fans on when it get's too hot, EVERYDAY for the rest of your life

### Architecture

Basic(Unmonitored) Power Connections:

a `*` in front of the word is a recommend spot for a circuit breaker

```mermaid
  graph TD;
      Panels(Solar Panels)-->*MPPT
      *MPPT-->Batteries
      Batteries-->LVD(Low Voltage Disconnect)
      LVD-->*24vFuseBox/Load
      LVD-->VReg(Voltage Regulator)
      VReg-->*12vFuseBox/Load
```

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.
_For more examples, please refer to the [Documentation](https://solarpi.link)
<p align="right">(<a href="#top">back to top</a>)</p>


<!-- ROADMAP -->
## Roadmap

- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3
    - [ ] Nested Feature

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



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Eric D Moore - [@ericdmoore](https://twitter.com/ericdmoore) - eric [at] mooore .cc

Project Link: [https://github.com/ericdmoore/solar-pi](https://github.com/ericdmoore/solar-pi)

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->
<!-- ## Acknowledgments -->

<!-- * []() -->
<!-- * []() -->
<!-- * []() -->

<!-- <p align="right">(<a href="#top">back to top</a>)</p> -->

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
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
[linkedin-url]: https://linkedin.com/in/eric.moore

[product-screenshot]: https://i.redd.it/09q1rdtno2241.png 
<!-- The product-screen shot is still a placeholders screenshot -->
