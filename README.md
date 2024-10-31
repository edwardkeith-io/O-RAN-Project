# ORAN Project
A srsRAN+USRP Research Project for Auckland University of Technology

> [!NOTE]
> This is a final year capstone project, not currently intended for external application.
> <br>
> If you would like to collaborate on development please get in touch!

## Summary

Our project aims to emulate the implementation of an open-source 5G small cell base-station, leveraging Software Defined Radios (SDRs) and off-the-shelf hardware. The primary objective is to replicate the functionality described in the paper [T ARXIV.ORG: 2205.13178](research_paper.pdf), utilizing a standard laptop/PC and Ettus Laboratories [B-205-mini SDR](https://www.ettus.com/all-products/usrp-b205mini-i/) (Software Defined Radios) module with appropriate antennas. The secondary objective is the production of detailed documentation on the process and troubleshooting, to aid in further replication and improve usability. 

-   Scope: The project encompasses the emulation of both RAN and mobile core network functionalities using open-source software and SDR technology. We will focus on utilizing software components that replicate the functions of proprietary hardware, ensuring compatibility with existing network infrastructures.

We give thanks to the Computer Science and Software Engineering department at AUT, and specifically the following faculty members:

Prof. Edmund Lai<br>
Dr. Bobby Mee Loong Yang<br>
Prof. Nurul Sarkar<br>
Dr. Ramesh Lal<br>
Dr. Jing Ma<br>
## Background/Rationale

5G small cell (pico) base-stations can provide advanced features and standalone capabilities for private networks. There are two main parts of a mobile base-station – the radio access network (RAN) and the mobile core network. These parts are typically bundled and sold as proprietary hardware and software. However, the advance in virtualization technologies and commercial off-the-shelf (COTS) hardware has made it possible to implement such a base station using opensource software and Software Defined Radios (SDR).

Although the technology described by the [O-RAN Alliance](https://www.o-ran.org/) specifications has been defined and implemented successfully within research and development environments, there does not seem to be straightforward and exhaustive documentation on the technical deployment of a network following the O-RAN architecture.

All existing published research appears to be written for the purpose of dissemination within academic circles, assuming a highly specialised technical knowledge of its reader. We propose that the creation of a freely available and comprehensible record of the implementation of a 5G testbed following O-RAN standards will allow other researchers to access and replicate our project, laying the foundation for further groundwork for flexible, interoperable, and non-proprietary mobile networks.

In addition, we believe that the synthesis of such documentation will allow for easier circulation of the O-RAN architecture and its highly promising applications within amateur or hobbyist communities who might otherwise feel intimidated by a perceived high barrier of entry into this subject matter.
<br>

### Cost Effectiveness

Traditional 5G small cell base-stations rely heavily on proprietary hardware and software solutions, resulting in high deployment and maintenance costs for network operators. By developing an open-source emulation solution, the project aims to significantly reduce these costs, making 5G infrastructure more accessible and affordable for a wider range of organizations.

### Scalability and Flexibility

Proprietary solutions often lack the scalability and flexibility required to adapt to evolving network requirements. The open-source emulation approach allows for greater customization and adaptability, enabling network operators to tailor their infrastructure to specific needs and scale operations more efficiently.

### Compatibility and Interoperability

Existing proprietary systems may face compatibility issues with other network components or protocols, limiting interoperability and hindering seamless integration within larger network ecosystems. The open-source emulation solution aims to address these compatibility challenges by providing standardized interfaces and protocols, enhancing interoperability and facilitating smoother integration with existing network infrastructure.

### Innovation and Collaboration

Open-source projects foster innovation and collaboration within the industry, enabling developers and researchers to contribute to the advancement of 5G technology collectively. By embracing an open-source approach, the project encourages collaboration among industry stakeholders, driving innovation and accelerating the development of next-generation telecommunications solutions.

### Addressing Market Demand

There is a growing demand for cost-effective and flexible 5G infrastructure solutions, particularly in sectors such as smart cities, IoT (Internet of Things), and industrial automation. The project aims to capitalize on this market demand by offering an open-source emulation solution that meets the needs of diverse industry verticals, opening new opportunities for deployment and expansion.

### Overcoming Vendor Lock-In

Vendor lock-in is a common challenge faced by network operators, limiting their ability to switch providers or upgrade equipment without significant cost and disruption. The open-source emulation solution provides an alternative to vendor lock-in, empowering network operators to retain control over their infrastructure and reduce dependency on single vendors.

The project addresses critical issues within the telecommunications industry by offering a cost-effective, scalable, and interoperable solution for implementing 5G small cell base-stations. By embracing open-source principles and fostering collaboration, the project aims to drive innovation, address market demand, and overcome barriers to adoption, ultimately enabling a more accessible and efficient 5G infrastructure landscape.
<br>
<br>

## Results:

Host and receiver setup uses the O-RAN architecture to establish LTE connectivity  between the two devices and simulate real-world small cell operations. It consists of SRSeNB/SRSEPC running on one computer, transmitting LTE signals as the host. SRSUE acts as the receiver on another device.

Both setups operate on Ubuntu 22.04 connected to SDRs with compatible cellular antennas, ensuring accurate and reliable data transmission. Docker containers are pre-configured to enable primary connection with no adjustment by the user.

Connectivity was validated using ping tests, Apache2 web server access, OpenSSH, and SCP for file transfer between devices. Conducted physical distance testing to observe the testbed’s performance and stability in different environments.

![network](portfolio/master_doc_assets/srsRAN_topology.png)


