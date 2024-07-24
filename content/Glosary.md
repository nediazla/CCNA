**NUMERICS**

**3G/4G Internet**: Una tecnología de acceso a Internet que utiliza señales de radio inalámbricas para comunicarse a través de torres de telefonía móvil, utilizada con mayor frecuencia por teléfonos móviles, tabletas y algunos otros dispositivos móviles.

**802.1 Q**: El protocolo estandarizado IEEE para enlace troncal VLAN.          

**A**

**AAA**: Autenticación, autorización y contabilidad. La autenticación confirma la identidad del usuario o dispositivo. La autorización determina lo que el usuario o dispositivo puede hacer. La contabilidad registra información sobre intentos de acceso, incluidas solicitudes inapropiadas.        

**Access Control Entry (ACE)**: Una línea en una lista de control de acceso (ACL).           

**access interface**     A LAN network design term that refers to a switch interface connected to end-user devices, configured so that it does not use VLAN trunking.

**access layer**            In a campus LAN design, the switches that connect directly to endpoint devices (servers, user devices), and also connect into the distribution layer switches.

**access link**              In Frame Relay, the physical serial link that connects a Frame Relay DTE device, usually a router, to a Frame Relay switch. The access link uses the same physical layer standards as do point-to-point leased lines.

**access link (WAN)** A physical link between a service provider and its customer that provides access to the SP’s network from that customer site. **access rate**             The speed at which bits are sent over an access link. **accounting**        In security, the recording of access attempts. _See also_ AAA.

**ACI**        _See_ Application Centric Infrastructure (ACI).

**ACL**         Access control list. A list configured on a router to control packet flow through the router, such as to prevent packets with a certain IP address from leaving a particular interface on the router.

**Active Directory**     A popular set of identity and directory services from Microsoft, used in part to authenticate users.

**administrative distance**         In Cisco routers, a means for one router to choose between multiple routes to reach the same subnet when those routes are learned by different routing protocols. The lower the administrative distance, the more preferred the source of the routing information.

**agent**      Generally, an additional software process or component running in a computing device for some specific purpose; a small and specific software service.

**agent-based architecture**      With configuration management tools, an architecture that uses a software agent inside the device being managed as part of the functions to manage the configuration.

**agentless architecture**           With configuration management tools, an architecture that does not need a software agent inside the device being managed as part of the functions to manage the configuration, instead using other mainstream methods like SSH and NETCONF.

**amplification attack**               A reflection attack that leverages a service on the reflector to generate and reflect huge volumes of reply traffic to the victim.

**analog modem**       _See_ modem.

**Ansible**   A popular configuration management application, which can be used with or without a server, using a push model to move configurations into devices, with strong capabilities to manage network device configurations.

**Ansible inventory** Device host names along with information about each device, like device roles, so Ansible can perform functions for subsets of the inventory.

**Ansible playbook**           Files with actions and logic about what Ansible should do.

**Ansible template**   A text file, written in Jinja2 language, that lists configuration but with variable names substituted for values, so that Ansible can create standard configurations for multiple devices from the same template.

**anti-replay**             Preventing a man in the middle from copying and later replaying the packets sent by a legitimate user, for the purpose of appearing to be a legitimate user.

**antivirus** Software that monitors files transferred by any means, for example, web or email, to look for content that can be used to place a virus into a computer.

**APIC**         _See_ Application Policy Infrastructure Controller.

**APIC-EM**           _See_ Application Policy Infrastructure Controller—Enterprise Module.

**Application Centric Infrastructure (ACI)**              Cisco’s data center SDN solution, the concepts of defining policies that the APIC controller then pushes to the switches in the network using the OpFlex protocol, with the partially distributed control plane in each switch building the forwarding table entries to support the policies learned from the controller. It also supports a GUI, a CLI, and APIs.

**Application Policy Infrastructure Controller—Enterprise Module (APIC-EM)**               The software that plays the role of controller in an enterprise network of Cisco devices, in its first version as of the publication of this book, which leaves the distributed routing and switching control plane as is, instead acting as a management and automation platform. It provides robust APIs for network automation and uses CLI (Telnet and SSH) plus SNMP southbound to control the existing routers and switches in an enterprise network.

**Application Policy Infrastructure Controller (APIC)**            The software that plays the role of controller, controlling the flows that the switches create to define where frames are forwarded, in a Cisco data center that uses the Application Centric Infrastructure (ACI) approach, switches, and software.

**application programming interface (API)**            A software mechanism that enables software components to communicate with each other.

**application signature**            With Network Based Application Recognition (NBAR), the definition of a combination of matchable fields that Cisco has identified as being characteristic of a specific application, so that NBAR can be configured by the customer to match an application, while IOS then defines the particulars of that matching.

**Application Visibility and Control (AVC)**              A firewall device with advanced features, including the ability to run many related security features in the same firewall device (IPS, malware detection, VPN termination), along with deep packet inspection with Application Visibility and Control (AVC) and the ability to perform URL filtering versus data collected about the reliability and risk associated with every domain name.

**application-specific integrated circuit (ASIC)**      An integrated circuit (computer chip) designed for a specific purpose or application, often used to implement the functions of a networking device rather than running a software process as part of the device’s OS that runs on a general-purpose processor.

**AR** _See_ access rate.

**ARP**         Address Resolution Protocol. An Internet protocol used to map an IP address to a MAC address. Defined in RFC 826.

**ARP ACL** A configuration feature on Cisco LAN switches that define MAC and IP address pairs that can be used directly for filtering, as well as to be referenced by the Dynamic ARP Inspection feature.

**ARP reply** An ARP message used to supply information about the sending (origin) host’s hardware (Ethernet) and IP addresses as listed in the origin hardware and origin IP address fields. Typically sent in reaction to receipt of an ARP request message.

**ARP request** An ARP message used to request information from another host located on the same data link, typically listing a known target IP address but an all-zero target hardware address, to ask the host with that target IP address to identify its hardware address in an ARP reply message.

**ARP table** A list of IP addresses of neighbors on the same VLAN, along with their MAC addresses, as kept in memory by hosts and routers.

**ASAv**       A Cisco ASA firewall software image that runs as a virtual machine rather than on Cisco hardware, intended to be used as a consumer-controlled firewall in a cloud service or in other virtualized environments.

**ASIC**        _See_ application-specific integrated circuit.

**Assured Forwarding (AF)**       The name of a grid of 12 DSCP values and a matching grid of per-hop behavior as defined by DiffServ. AF defines four queuing classes and three packet drop priorities within each queuing class. The text names of the 12 DSCP values follow a format of AFXY, where X is the queuing class, and Y is the drop priority.

**authentication**               In security, the verification of the identity of a person or a process. _See also_ AAA.

**authentication, authorization, and accounting (AAA) server**            A server that holds security information and provides services related to user login, particularly authentication (is the user who he says he is), authorization (once authenticated, what do we allow the user to do), and accounting (tracking the user).

**Authoritative DNS server**      The DNS server with the record that lists the address that corresponds to a domain name (the A Record) for that domain.

**authorization**         In security, the determination of the rights allowed for a particular user or device. _See also_ AAA. **autonomous system (AS)**      An internetwork that is managed by one organization.

**autonomous system number (ASN)**     A number used by BGP to identify a routing domain, often a single enterprise or organization. As used with EIGRP, a number that identifies the routing processes on routers that are willing to exchange EIGRP routing information with each other.

**AutoQoS** In Cisco switches and routers, an IOS feature that configures a variety of QoS features with useful settings as defined by the Cisco reference design guide documents.

**B**

**bandwidth**         The speed at which bits can be sent and received over a link.

**bandwidth profile** In Metro Ethernet, a contractual definition of the amount of traffic that the customer can send into the service and receive out of the service. Includes a concept called the committed information rate (CIR), which defines the minimum amount of bandwidth (bits per second) the SP will deliver with the service.

**Brownfield**             A term that refers to the choice to add new configuration to hardware and software that are already in use, rather that adding new hardware and software specifically for a new project.

**brute-force attack** An attack where a malicious user runs software that tries every possible combination of letters, numbers, and special characters to guess a user’s password. Attacks of this scale are usually run offline, where more computing resources and time are available.

**buffer overflow attack**           An attack meant to exploit a vulnerability in processing inbound traffic such that the target system’s buffers overflow; the target system can end up crashing or inadvertently running malicious code injected by the attacker.

**C**

**cable Internet**        An Internet access technology that uses a cable TV (CATV) cable, normally used for video, to send and receive data.

**cacheable**               For resources that might be repeatedly requested over time, an attribute that means that the requesting host can keep in storage (cache) a copy of the resource for a specified amount of time.

**candidate config**    With configuration management tools like Ansible, Puppet, and Chef, an updated configuration for a device as it exists in the management tool before the tool has moved the configuration into the device.

**carrier Ethernet**     Per MEF documents, the term for what was formerly called Metro Ethernet, generally referring to any WAN service that uses Ethernet links as the access link between the customer and the service provider.

**CDP**         Cisco Discovery Protocol. A media- and protocol-independent device-discovery protocol that runs on most Cisco-manufactured equipment, including routers, access servers, and switches. Using CDP, a device can advertise its existence to other devices and receive information about other devices on the same LAN or on the remote side of a WAN.

**CDP neighbor**         A device on the other end of some communications cable that is advertising CDP updates.

**central office (CO)** A term used by telcos to refer to a building that holds switching equipment, into which the telco’s cable plant runs so that the telco has cabling from each home and business into that building.

**centralized control plane**      An approach to architecting network protocols and products that places the control plane functions into a centralized function rather than distributing the function across the networking devices.

**Chef**        A popular configuration management application, which uses a server and a pull model with in-device agents.

**Chef client** Any device whose configuration is being managed by Chef.

**Chef Cookbook**      A set of recipes about the same kinds of work, grouped together for easier management and sharing.

**Chef Recipe**            The Chef logic applied to resources to determine when, how, and whether to act against the resources—analogous to a recipe in a cookbook.

**Chef Runlist** An ordered list of recipes that should be run against a given device.

**Chef server**             The Chef software that collects all the configuration files and other files used by Chef from different Chef users and then communicates with Chef clients (devices) so that the Chef clients can synchronize their configurations.

**CIDR**       Classless interdomain routing. An RFC-standard tool for global IP address range assignment. CIDR reduces the size of Internet routers’ IP routing tables, helping deal with the rapid growth of the Internet. The term _classless_ refers to the fact that the summarized groups of networks represent a group of addresses that do not conform to IPv4 classful (Class A, B, and C) grouping rules.

**Cisco Access Control Server (ACS)** A legacy Cisco product that acts as a AAA server.

**Cisco AnyConnect Secure Mobility Client** Cisco software product used as client software on user devices to create a client VPN. Commonly referred to as the Cisco VPN client.

**Cisco Open SDN Controller (OSC)** A former commercial SDN controller from Cisco that is based on the OpenDaylight controller.

**Cisco Prime** Graphical user interface (GUI) software that utilizes SNMP and can be used to manage your Cisco network devices. The term _Cisco Prime_ is an umbrella term that encompasses many different individual software products.

**Cisco Prime Infrastructure (PI)**             The name of Cisco’s long-time enterprise network management application.

**Cisco Talos Intelligence Group**              A part of the Cisco Systems company that works to perform security research on an ongoing basis, in part to supply up-to-date data, like virus signatures, that Cisco security products can frequently download.

**Cisco VPN client**          _See_ Cisco AnyConnect Secure Mobility Client.

**Class of Service (CoS)**            The informal term for the 3-bit field in the 802.IQ header intended for marking and classifying Ethernet frames for the purposes of applying QoS actions. Another term for Priority Code Point (PCP).

**Class Selector (CS)** The name of eight DSCP values that all end with binary 000, for the purpose of having eight identifiable DSCP values whose first 3 bits match the eight values used for the older IP Precedence field. Originally used for backward compatibility with IP Precedence, but today the values are often used as just more values to use for packet marking.

**classification**          The process of examining various fields in networking messages in an effort to identify which messages fit into certain predetermined groups (classes).

**classless addressing**              A concept in IPv4 addressing that defines a subnetted IP address as having two parts: a prefix (or subnet) and a host.

**client VPN**              A VPN for which one endpoint is a user device, like a phone, tablet, or PC. Also called a remote access VPN. **clock rate**               The speed at which a serial link encodes bits on the transmission medium.

**clock source**           On serial links, the device to which the other devices on the link adjust their speed when using synchronous links. With NTP, the external device or NTP server on which a device bases its time.

**clocking** The process of supplying a signal over a cable, either on a separate pin on a serial cable or as part of the signal transitions in the transmitted signal, so that the receiving device can keep synchronization with the sending device.

**Clos network**          A term for network topology that represents an ideal for a switch fabric and named after Charles Clos, who formalized the definition. Also called a spine-leaf network. **cloud services catalog**          A listing of the services available in a cloud computing service.

500  Cloud Services Router (CSR)

**Cloud Services Router (CSR)** A Cisco router software image that runs as a virtual machine rather than on Cisco hardware, intended to be used as a consumer-controlled router in a cloud service or in other virtualized environments.

**code integrity** A software security term that refers to how likely that the software (code) being used is the software supplied by the vendor, unchanged, with no viruses or other changes made to the software.

**collapsed core design** A campus LAN design in which the design does not use a separate set of core switches in addition to the distribution switches—in effect collapsing the core into the distribution switches.

**confidentiality (privacy)** Preventing anyone in the middle of the Internet (a.k.a. man in the middle) from being able to read the data.

**configuration drift** A phenomenon that begins with the idea that devices with similar roles can and should have a similar standard configuration, so when one device’s configuration is changed to be different, its configuration is considered to have moved away (drifted) from the standard configuration for a device in that role. **configuration enforcement** Another term for configuration monitoring.

**configuration management** A component of network management focused on creating, changing, removing, and monitoring device configuration.

**configuration management tool** A class of application that manages data about the configuration of servers, network devices, and other computing nodes, providing consistent means of describing the configurations, moving the configurations into the devices, noticing unintended changes to the configurations, and troubleshooting by easily identifying changes to the configuration files over time.

**configuration monitoring** With configuration management tools like Ansible, Puppet, and Chef, a process of comparing over time a device’s on-device configuration (running-config) versus the text file showing the ideal device configuration listed in the tool’s centralized configuration repository. If different, the process can either change the device’s configuration or report the issue.

**configuration provisioning** With configuration management tools like Ansible, Puppet, and Chef, the process of configuring a device to match the configuration as held in the configuration management tool.

**configuration template** With configuration management tools like Ansible, Puppet, and Chef, a file with variables, for the purpose of having the tool substitute different variable values to create the configuration for a device.

**congestion window** With TCP, a calculation each TCP receiver does that limits the window it grants to the receiver by shrinking the window in response to the loss of TCP segments.

**connection establishment** The process by which a connection-oriented protocol creates a connection. With TCP, a connection is established by a three-way transmission of TCP segments.

**control plane** Functions in networking devices and controllers that directly control how devices perform data plane forwarding, but excluding the data plane processes that work to forward each message in the network.

**controller-based networking** A style of building computer networks that use a controller that centralizes some features and provides application programming interfaces (APIs) that allow for software interactions between applications and the controller (northbound APIs) and between the controller and the network devices (southbound APIs).

**core**        In computer architecture, an individual processing unit that can execute instructions of a CPU; modern server processors typically have multiple cores, each capable of concurrent execution of instructions.

**core design**            A campus LAN design that connects each access switch to distribution switches, and distribution switches into core switches, to provide a path between all LAN devices.

**core layer**               In a campus LAN design, the switches that connect the distribution layer switches, and to each other, to provide connectivity between the various distribution layer switches.

**CRUD**      In software development, an acronym that refers to the four most common actions taken by a program: Create, Read, Update, and Delete.

**customer edge (CE)**               A term used by service providers, both generally and also specifically in MPLS VPN networks, to refer to the customer device that connects to the SP’s network and therefore sits at the edge of the SP’s network.

**customer premises equipment (CPE)**   A telco term that refers to equipment on site at the telco customer site (the enterprise’s site) that connects to the WAN service provided by the telco.

**D**

**data integrity** Verifying that the packet was not changed as the packet transited the Internet. **data model**      A set of variables and their structures, like lists and dictionaries.

**data modeling language**           Another term for data serialization language.

**data plane**              Functions in networking devices that are part of the process of receiving a message, processing the message, and forwarding the message.

**data serialization language**   A language that includes syntax and rules that provides a means to describes the variables inside applications in a text format, for the purpose of sending that text between applications over a network or storing the data models in a file. **data structure**                Another term for data model.

**declarative policy model**       A term that describes the approach in an intent-based network (IBN) in which the engineer chooses settings that describe the intended network behavior (the declared policy) but does not command the network with specific configuration commands for each protocol (as would be the case with an imperative policy model).

**decrypt/decryption**               The ability to receive encrypted data and process it to derive the original unencrypted data.

**default gateway/default router**           On an IP host, the IP address of some router to which the host sends packets when the packet’s destination address is on a subnet.

**delay**      In QoS, the amount of time it takes for a message to cross a network. Delay can refer to one-way delay (the time required for the message to be sent from the source host to the destination host) or two-way delay (the delay from the source to the destination host and then back again).

**demilitarized zone (DMZ)**     In an Internet edge design at an enterprise, one or more subnets set aside as a place to locate servers that should allow users in the Internet to initiate connections to those servers. The devices in the DMZ typically sit behind a firewall.

**denial-of-service (DoS) attack**              An attack that tries to deplete a system resource so that systems and services crash or become unavailable. **deny**     An action taken with an ACL that implies that the packet is discarded.

**DevNet**   Cisco’s community and resource site for software developers, open to all, with many great learning resources; [https://developer.cisco.com.](https://developer.cisco.com/)

**DHCP**      Dynamic Host Configuration Protocol. A protocol used by hosts to dynamically discover and lease an IP address, and learn the correct subnet mask, default gateway, and DNS server IP addresses.

**DHCP attack**           Any attack that takes advantage of DHCP protocol messages.

**DHCP binding table**               A table built by the DHCP snooping feature on a switch when it sees messages about a new DHCP lease, with the table holding information about legitimate successful DHCP leases, including the device’s IP address, MAC address, switch port, and VLAN.

**DHCP chaddr**          Client hardware address. The original DHCP header field used to identify the DHCP clients; typically includes the client MAC address.

**DHCP client** Any device that uses DHCP protocols to ask to lease an IP address from a DHCP server or to learn any IP settings from that server.

**DHCP client identifier**           A DHCP header field used to identify a DHCP client, used as a more flexible alternative to the DHCP chaddr field.

**DHCP giaddr**           Gateway IP address. In DHCP, a header field used to identify a router on a subnet, typically an IP address on the DHCP relay agent, so that the DHCP server knows an address to which to send messages in reply to the client.

**DHCP option 82**     Optional DHCP header fields, as defined in RFC 3046, that provide useful features of use to a device that acts as a DHCP relay agent. The fields allow better relay agent operation and also help prevent various types of DHCP-based attacks.

**DHCP relay agent**   The name of the router IOS feature that forwards DHCP messages from client to servers by changing the destination IP address from 255.255.255.255 to the IP address of the DHCP server.

**DHCP server**           Software that waits for DHCP clients to request to lease IP addresses, with the server assigning a lease of an IP address as well as listing other important IP settings for the client.

**DHCP Snooping**      A switch security feature in which the switch examines incoming DHCP messages and chooses to filter messages that are abnormal and therefore might be part of a DHCP attack.

**DHCP Snooping binding table**              When using DHCP Snooping, a table that the switch dynamically builds by analyzing the DHCP messages that flow through the switch. DHCP Snooping can use the table for part of its filtering logic, with other features, such as Dynamic ARP Inspection and IP Source Guard also using the table.

**dictionary attack**    An attack where a malicious user runs software that attempts to guess a user’s password by trying words from a dictionary or word list.

**dictionary variable**                In applications, a single variable whose value is a list of other variables with values, known as key:value pairs.

**Differentiated Services (DiffServ)**         An approach to QoS, originally defined in RFC 2475, that uses a model of applying QoS per classification, with planning of which applications and other traffic types are assigned to each class, with each class given different QoS per-hop behaviors at each networking device in the path.

**Differentiated Services Code Point (DSCP)**          A field existing as the first 6 bits of the ToS byte, as defined by RFC 2474, which redefined the original IP RFC’s definition for the IP header ToS byte. The field is used to mark a value in the header for the purpose of performing later QoS actions on the packet.

**Digital Subscriber Line (DSL)**                 A public network technology that delivers high bandwidth over conventional telco local-loop copper wiring at limited distances. Typically used as an Internet access technology, connecting a user to an ISP.

**distributed control plane**      An approach to architecting network protocols and products that places some control plane functions into each networking device rather than centralizing the control plane functions in one or a few devices. An example is the use of routing protocols on each router which then work together so that each router learns Layer 3 routes.

**distributed denial-of-service (DDoS) attack**        A DoS attack that is distributed across many hosts under centralized control of an attacker, all targeting the same victim.

**distribution layer**   In a campus LAN design, the switches that connect to access layer switches as the most efficient means to provide connectivity from the access layer into the other parts of the LAN.

**DNA**        Digital Network Architecture—Cisco’s software-oriented approach to networking and intent-based networking products and services.

**DNA Center**            Cisco software, delivered by Cisco on a server appliance, that acts as a network management application as well as a being the control for Cisco’s software-defined access (SDA) offering.

**DNS**        Domain Name System. An application layer protocol used throughout the Internet for translating host names into their associated IP addresses.

**DNS reply**               In the Domain Name System (DNS), a message sent by a DNS server to a DNS client in response to a DNS request, identifying the IP address assigned to a particular hostname or fully qualified domain name (FQDN).

**DNS request**           In the Domain Name System (DNS), a message sent by a DNS client to a DNS server, listing a hostname or fully qualified domain name (FQDN), asking the server to discover and reply with the IP address associated with that host name or FQDN.

**DNS server**             An application acting as a server for the purpose of providing name resolution services per the Domain Name System (DNS) protocol and worldwide system.

**domain-specific language**     A generic term that refers to an attribute of different languages within computing, for languages created for a specific purpose (domain) rather than a generalpurpose language like Python or JavaScript.

**DSL**         Digital subscriber line. Public network technology that delivers high bandwidth over conventional telco local-loop copper wiring at limited distances. Usually used as an Internet access technology connecting a user to an ISP.

**DSL modem**            A device that connects to a telephone line and uses DSL standards to transmit and receive data to/from a telco using DSL.

**Dynamic ARP Inspection (DAI)**             A security feature in which a LAN switch filters a subset of incoming ARP messages on untrusted ports, based on a comparison of ARP, Ethernet, and IP header fields to data gathered in the IP DHCP Snooping binding table and found in any configured ARP ACLs.

**E**

**egress tunnel router (ETR)** With LISP, a node at the end of a tunnel that receives an encapsulated message and then de-encapsulates the message.

**E-LAN**     A specific carrier/Metro Ethernet service defined by MEF [(MEF.net)](http://MEF.net/) that provides a service much like a LAN, with two or more customer sites connected to one E-LAN service in a full mesh so that each device in the E-LAN can send Ethernet frames directly to every other device.

**E-Line**     A specific carrier/metro Ethernet service defined by MEF [(MEF.net)](http://MEF.net/) that provides a point-to-point topology between two customer devices, much as if the two devices were connected using an Ethernet crossover cable.

**enable mode**          A part of the Cisco IOS CLI in which the user can use the most powerful and potentially disruptive commands on a router or switch, including the ability to then reach configuration mode and reconfigure the router.

**enable password**   A reference to the password configured on the **enable password** _passvalue_ command, which defines the password required to reach enable (privileged) mode if the **enable secret** _pass-value_ command does not exist.

**enable secret**         A reference to the password configured on the **enable secret** _pass-value_ command, which defines the password required to reach enable (privileged) mode.

**encrypt/encryption**               The ability to take data and send the data in a form that is not readable by someone who intercepts this data.

**encryption key**       A secret value used as input to the math formulas used by an encryption process.

**End of Row (EoR) switch** In a traditional data center design with servers in multiple racks and the racks in multiple rows, a switch placed in a rack at the end of the row, intended to be cabled to all the Top of Rack (ToR) switches in the same row, to act as a distribution layer switch for the switches in that row.

**endpoint group** In ACI, a set (group) of VMs, containers, physical servers, or other endpoints in an ACI data center that should receive the same policy treatment.

**Endpoint ID (EID)** With LISP, a number that identifies the endpoint.

**err-disable recovery** Cisco switches can place ports in a nonworking state called “errdisabled” in reaction to a variety of events, and by default, to leave the port in the nonworking err-disabled state until the engineer takes action to recover from the issue. The err-disable recovery configuration feature includes settings to direct the switch to automatically revert away from the err-disabled state, back to a working state, after a period of time.

**error detection** The process of discovering whether a data-link level frame was changed during transmission. This process typically uses a Frame Check Sequence (FCS) field in the datalink trailer.

**error disabled (err-disable)** An interface state on LAN switches that can be the result of one of many security violations.

**error recovery** The process of noticing when some transmitted data was not successfully received and resending the data until it is successfully received.

**Ethernet access link** A WAN access link (a physical link between a service provider and its customer) that happens to use Ethernet.

**Ethernet LAN Service** Another term for E-LAN; _see also_ E-LAN.

**Ethernet Line Service**          Another term for E-Line; _see_ _also_ E-Line.

**Ethernet Tree Service**          Another term for E-Tree; _see_ _also_ E-Tree.

**Ethernet Virtual Connection (EVC)**       A concept in carrier/Metro Ethernet that defines which customer devices can send frames to each other over the Ethernet WAN service; includes E-Line, E-LAN, and E-Tree EVCs.

**Ethernet WAN**        A general and informal term for any WAN service that uses Ethernet links as the access link between the customer and the service provider.

**E-Tree**     A specific carrier/metro Ethernet service defined by MEF [(MEF.net)](http://MEF.net/) that provides a rooted multipoint service, in which the root site can send frames directly to all leaves, but the leaf sites can send only to the root site.

**Expedited Forwarding (EF)**   The name of a particular DSCP value, as well as the term for one per-hop behavior as defined by DiffServ. The value, decimal 46, is marked for packets to which the networking devices should apply certain per-hop behaviors, like priority queuing. **exploit**     A means of taking advantage of a vulnerability to compromise something.

**extended access list**              A list of IOS **access-list** global configuration commands that can match multiple parts of an IP packet, including the source and destination IP address and TCP/ UDP ports, for the purpose of deciding which packets to discard and which to allow through the router.

**F**

**fabric**      In SDA, the combination of overlay and underlay that together provide all features to deliver data across the network with the desired features and attributes.

**fabric border node**                In SDA, a switch that connects to devices outside SDA’s control—for example, switches that connect to the WAN routers or to an ACI data center.

**fabric control node**               In SDA, a switch that performs special functions for the underlay (LISP), requiring more CPU and memory. **fabric edge node**    In SDA, a switch that connects to endpoint devices.

**fiber Internet**         A general term for any Internet access technology that happens to use fiberoptic cabling. It often uses Ethernet protocols on the fiber link.

**filter**       Generally, a process or a device that screens network traffic for certain characteristics, such as source address, destination address, or protocol. This process determines whether to forward or discard that traffic based on the established criteria.

**firewall**   A device that forwards packets between the less secure and more secure parts of the network, applying rules that determine which packets are allowed to pass and which are not.

**First Hop Redundancy Protocol (FHRP)**               A class of protocols that includes HSRP, VRRP, and GLBP, which allows multiple redundant routers on the same subnet to act as a single default router (first-hop router).

**flash memory**        A type of read/write permanent memory that retains its contents even with no power applied to the memory, and uses no moving parts, making the memory less likely to fail over time.

**flow control**           The process of regulating the amount of data sent by a sending computer toward a receiving computer. Several flow control mechanisms exist, including TCP flow control, which uses windowing.

**forward acknowledgment**    A process used by protocols that do error recovery, in which the number that acknowledges data lists the next data that should be sent, not the last data that was successfully received.

**forwarding plane**          A synonym for data plane. _See_ _also_ data plane.

**FTP**          File Transfer Protocol. An application protocol, part of the TCP/IP protocol stack, used to transfer files between network nodes. FTP is defined in RFC 959.

**FTP active mode**    One of two modes of operation for FTP connections (the other being passive mode) that dictates how the FTP data mode connection is established. In active mode, the FTP client listens on a port, it identifies that port to the server, and the server initiates the TCP connection.

**FTP client**               An application that can connect to an FTP server for the purpose of transferring copies of files to and from the server.

**FTP control connection**         A TCP connection initiated by an FTP client to an FTP server for the purpose of sending FTP commands that direct the activities of the connection.

**FTP data connection**              A TCP connection created by an FTP client and server for the purpose of transferring data.

**FTP over TLS**           An FTP standard defined by RFC 4217, also known as FTP Secure (FTPS), which adds a variety of security features to the somewhat insecure original FTP standard (RFC 957), including the addition of the encryption of all data as well as username/password information using Transport Layer Security (TLS).

**FTP passive mode** One of two modes of operation for FTP connections (the other being active mode) that dictates how the FTP data mode connection is established. In passive mode, the FTP client declares the use of passive mode, causing the server to choose and identify a new listening port, with the client establishing a TCP connection to that port.

**FTP server**              An application that runs and waits for FTP clients to connect to it over TCP port 21 to support the client’s commands to transfer copies of files to and from the server.

**FTPS**        FTP Secure. Common term for FTP over TLS.

**full mesh** From a topology perspective, any topology that has two or more devices, with each device being able to send frames to every other device.

**G**

**Gateway Load Balancing Protocol (GLBP)** A Cisco-proprietary protocol that allows two (or more) routers to share the duties of being the default router on a subnet, with an active/ active model, with all routers actively forwarding off-subnet traffic for some hosts in the subnet.

**Generic Routing Encapsulation (GRE)** A protocol, defined in RFC 2784, that defines the headers used when creating a site-to-site VPN tunnel. The protocol defines the use of a normal IP header, called the Delivery Header, and a GRE header that the endpoints use to create and manage traffic over the GRE tunnel.

**Git**           An open-source version control application, widely popular for version control in software development and for other uses, like managing network device configurations.

**GitHub**         A software-as-a-service application that implements Git.

**gratuitous ARP**       An ARP Reply not sent as a reaction to an ARP request message, but rather as a general announcement informing other hosts of the values of the sending (origin) host’s addresses.

**GRE tunnel**             A site-to-site VPN idea, in which the endpoints act as if a point-to-point link (the tunnel) exists between the sites, while actually encapsulating packets using GRE standards.

**greenfield**               A term that refers to the installation of new equipment for a project rather than adding configuration to existing in-use hardware and software.

**H**

**host (context: DC)** In a virtualized server environment, the term used to refer to one physical server that is running a hypervisor to create multiple virtual machines.

**Hot Standby Router Protocol (HSRP)** A Cisco-proprietary protocol that allows two (or more) routers to share the duties of being the default router on a subnet, with an active/standby model, with one router acting as the default router and the other sitting by waiting to take over that role if the first router fails.

**HSRP active** A Hot Standby Router Protocol (HSRP) state in which the router actively supports the forwarding of off-subnet packets for hosts in that subnet.

**HSRP standby** A Hot Standby Router Protocol (HSRP) state in which the router does not currently support the forwarding of off-subnet packets for hosts in that subnet, instead waiting for the currently active router to fail before taking over that role.

**HTML**      Hypertext Markup Language. A simple document-formatting language that uses tags to indicate how a given part of a document should be interpreted by a viewing application, such as a web browser.

**HTTP**       Hypertext Transfer Protocol. The protocol used by web browsers and web servers to transfer files, such as text and graphic files.

 **HTTP verb**         The action defined in an HTTP request message.

**hub and spoke**       From a topology perspective, any topology that has a device that can send messages to all other devices (the hub), with one or more spoke devices that can send messages only to the hub. Also called point-to-multipoint. **hyperthreading**           The name of Intel’s multithreading technology.

**hypervisor**              Software that runs on server hardware to create the foundations of a virtualized server environment primarily by allocating server hardware components like CPU core/threads, RAM, disk, and network to the VMs running on the server.

**I**

**IANA**       The Internet Assigned Numbers Authority. An organization that owns the rights to assign many operating numbers and facts about how the global Internet works, including public IPv4 and IPv6 addresses. _See also_ ICANN.

**ICANN**     The Internet Corporation for Assigned Names and Numbers. An organization appointed by IANA to oversee the distributed process of assigning public IPv4 and IPv6 addresses across the globe.

**imperative policy model**       A term that describes the approach in traditional networks in which the engineer chooses configuration settings for each control and data plane protocol (the imperative commands) that dictate specifically how the devices act. This model acts in contrast to the newer declarative policy model and intent-based networking (IBN).

**Infrastructure as a Service (laaS)**          A cloud service in which the service consists of a virtual machine that has defined computing resources (CPUs, RAM, disk, and network) and may or may not be provided with an installed OS.

**ingress tunnel router (ITR)**    With LISP, the node that receives an unencapsulated message and encapsulates the message.

**inside global**          For packets sent to and from a host that resides inside the trusted part of a network that uses NAT, a term referring to the IP address used in the headers of those packets when those packets traverse the global (public) Internet.

**inside local**             For packets sent to and from a host that resides inside the trusted part of a network that uses NAT, a term referring to the IP address used in the headers of those packets when those packets traverse the enterprise (private) part of the network.

**integrity** In data transfers, means that the network administrator can determine that the information has not been tampered with in transit.

**intent-based networking (IBN)**             An approach to networking in which the system gives the operator the means to express business intent, with the networking system then determining what should be done by the network, activating the appropriate configuration, and monitoring (assuring) the results.

**intercloud exchange**              A WAN service that provides connectivity between public cloud providers and their customers so that customers can install and keep the WAN connections, even when migrating from one cloud provider to another.

**Internet access technology**   Any technology that an ISP offers that allows its customers to send and receive data to/from the ISP, including serial links, Frame Relay, MPLS, Metro Ethernet, DSL, cable, and fiber Internet.

**Internet edge**         The part of the topology of the Internet that sits between an ISP and the ISP’s customer.

**Internet service provider** A company or organization that provides Internet services to customers; the company may have a heritage as a telco, WAN service provider, or cable company. **internetwork operating system (IOS)**   _See_ IOS.

**intrusion detection system (IDS)** A security function that examines more complex traffic patterns against a list of both known attack signatures and general characteristics of how attacks can be carried out, rating each perceived threat and reporting the threats.

**intrusion prevention system (IPS)** A security function that examines more complex traffic patterns against a list of both known attack signatures and general characteristics of how attacks can be carried out, rating each perceived threat, and reacting to prevent the more significant threats. _See also_ IPS.

**IOS**          Cisco operating system software that provides the majority of a router’s or switch’s features, with the hardware providing the remaining features.

**IOS feature set**       A set of related features that can be enabled on a router to enable certain functionality. For example, the Security feature set would enable the ability to have the router act as a firewall in the network.

**IOS File System (IFS)**            A file system created by a Cisco device that uses IOS.

**IOS image**        A file that contains the IOS.

**IP Precedence (IPP)**               In the original definition of the IP header’s Type of Service (ToS) byte, the first 3 bits of the ToS byte, used for marking IP packets for the purpose of applying QoS actions.

**IPS**       _See_ intrusion prevention system.

**IPsec**       The term referring to the IP Security protocols, which is an architecture for providing encryption and authentication services, usually when creating VPN services through an IP network.

**ISDN**       Integrated Services Digital Network. A communication protocol offered by telephone companies that permits telephone networks to carry data, voice, and video.

**Iterative DNS server**              A DNS server that will answer DNS requests directly but will not take on the extra work to recursively send other DNS messages to find the answer.

**J**

**JavaScript**               A programming language popular for building dynamic web pages, commonly used to run scripts on a web client.

**Jinja2**      A text-based language used to define templates, with text plus variables; used by Ansible for templates.

 **jitter**          The variation in delay experienced by successive packets in a single application flow.

**JSON (JavaScript Object Notation)**       A popular data serialization language, originally used with the JavaScript programming language, and popular for use with REST APIs.

**JSON array**             A part of a set of JSON text that begins and ends with a matched set of square brackets that contain a list of values.

**JSON object**           A part of a set of JSON text that begins and ends with a matched set of curly brackets that contain a set of key:value pairs.

**K–L**

**key:value pair**        In software, one variable name (key) and its value, separated by a colon in some languages and data serialization languages.

**keyboard, video, mouse (KVM)**            Three components of a typical desktop computer that are typically not included in a modern server because the server is installed and managed remotely.

**KVM (Red Hat)**       Kernel-Based Virtual Machine (KVM), a server virtualization/hypervisor product from the Red Hat company.

**leaf**         In an ACI network design, a switch that connects to spine switches and to endpoints, but not to other leaf switches, so that the leaf can forward frames from an endpoint to a spine, which then delivers the frame to some other leaf switch.

**library**     In software, a collection of programs packaged so that it can be posted as available in a software repository, found by others, and installed as one entity, as a means to make it easier to share code.

**LISP**         Locator/ID Separation Protocol. A protocol, defined in RFC 6830, that separates the concepts and numbers used to identify an endpoint (the endpoint identifier) versus identifying the location of the endpoint (routing locator).

**LISP mapping database**         With LISP, the table that contains mapped pairs of endpoint identifiers and routing locators.

**LISP Routing Locator (RLOC)** With LISP, a value that identifies the location of an endpoint, typically the address of the egress device.

**list variable**            In applications, a single variable whose value is a list of values, rather than a simple value.

**LLDP**        Link Layer Discovery Protocol. An IEEE standard protocol (IEEE 802.1AB) that defines messages, encapsulated directly in Ethernet frames so they do not rely on a working IPv4 or IPv6 network, for the purpose of giving devices a means of announcing basic device information to other devices on the LAN. It is a standardized protocol similar to Cisco Discovery Protocol (CDP).

**local loop**            A line from the premises of a telephone subscriber to the telephone company CO.

**local username**      A username (with matching password), configured on a router or switch. It is considered local because it exists on the router or switch, and not on a remote server.

**log message**           A message generated by any computer, but including Cisco routers and switches, for which the device OS wants to notify the owner or administrator of the device about some event. **loss**              A reference to packets in a network that are sent but do not reach the destination host.

**low latency queue** In Cisco queuing systems, a queue from which the queue scheduling algorithm always takes packets next if the queue holds any packets. This scheduling choice means that packets in this queue spend little time in the queue, achieving low delay (latency) as well as low jitter.

**Low Latency Queuing (LLQ)** The name of a queuing system that can be enabled on Cisco routers and switches by which messages sensitive to latency and jitter are placed in a queue that is always serviced first, resulting in low latency and jitter for those messages.

**LTE**          Literally, Long Term Evolution, but this term is used as a word itself to represent the type of wireless 4G technology that allows faster speeds than the original 4G specifications.

**M**

**malware**       Malicious software.

**Management Information Base (MIB)**                The data structures defined by SNMP to define a hierarchy (tree) structure with variables at the leaves of the tree, so that SNMP messages can reference the variables.

**management plane**               Functions in networking devices and controllers that control the devices themselves but that do not impact the forwarding behavior of the devices like control plane protocols do.

**man-in-the-middle attack**     An attack where an attacker manages to position a machine on the network such that it is able to intercept traffic passing between target hosts.

**marking** The process of changing one of a small set of fields in various network protocol headers, including the IP header’s DSCP field, for the purpose of later classifying a message based on that marked value.

**markup language**   A language that provides conventions to tag text to identify the type of text, which allows application of different treatments to different types of text.

**match/action logic**                The basic logic done by a networking element: to receive incoming messages, to match fields in the message, to then use logic based on those matches to take action against the message, and to then forward the message.

**MD5 hash**               A specific mathematical algorithm intended for use in various security protocols. In the context of Cisco routers and switches, the devices store the MD5 hash of certain passwords, rather than the passwords themselves, in an effort to make the device more secure.

**Metro Ethernet**      The original term used for WAN service that used Ethernet links as the access link between the customer and the service provider.

**MIB**       _See_ Management Information Base.

**MIB view**                A concept in SNMPv3 that identifies a subset of an SNMP agent’s MIB for the purpose of limiting access to some parts of the MIB to certain SNMP managers. **mitigation technique**         A method to counteract or prevent threats and malicious activity.

**modem**   Modulator-demodulator. A device that converts between digital and analog signals so that a computer may send data to another computer using analog telephone lines. At the source, a modem converts digital signals to a form suitable for transmission over analog communication facilities. At the destination, the analog signals are returned to their digital form.

**MPLS**        _See_ Multiprotocol Label Switching.

**MPLS experimental bits**           A 3-bit field in the MPLS label used for QoS marking.

**MPLS VPN**              A WAN service that uses MPLS technology, with many customers connecting to the same MPLS network, but with the VPN features keeping each customer’s traffic separate from others.

**MTU**        Maximum transmission unit. The maximum packet size, in bytes, that a particular interface can handle.

**multifactor authentication**    A technique that uses more than one type of credential to authenticate users.

**multipoint**              A topology with more than two devices in it (in contrast to a point-to-point topology, which has exactly two devices). Without any further context, the term _multipoint_ does not define whether all devices in the topology can send messages directly to each other (full mesh) or not (partial mesh).

**Multiprotocol BGP (MPBGP)** A particular set of BGP extensions that allows BGP to support multiple address families, which when used to create an MPLS VPN service gives the SP the method to advertise the IPv4 routes of many customers while keeping those route advertisements logically separated.

**Multiprotocol Label Switching (MPLS)** A WAN technology used to create an IP-based service for customers, with the service provider’s internal network performing forwarding based on an MPLS label rather than the destination IP address.

**multithreading**       In computer architecture, a process of maximizing the use of a processor core by sharing an individual core among multiple programs, taking advantage of the typical idle times for the core while it waits on various other tasks like memory reads and writes.

**N**

**name resolution**    The process by which an IP host discovers the IP address associated with a host name, often involving sending a DNS request to a DNS server, with the server supplying the IP address used by a host with the listed host name.

**name server**           A server connected to a network that resolves network names into network addresses.

**named access list** An ACL that identifies the various statements in the ACL based on a name rather than a number.

**NAT**         Network Address Translation. A mechanism for reducing the need for globally unique IP addresses. NAT allows an organization with addresses that are not globally unique to connect to the Internet, by translating those addresses into public addresses in the globally routable address space.

**NAT overload**         Another term for Port Address Translation (PAT). One of several methods of configuring NAT, in this case translating TCP and UDP flows based on port numbers in addition to using one or only a few inside global addresses.

**National Institute of Standards and Technology (NIST)** A U.S. federal agency that develops national standards, including standards for cloud computing.

**NBI**      _See_ northbound API.

**Nest**        In JSON, the concept that values can contain objects and arrays so that each object can contain other objects and arrays in a myriad of combinations.

**Network Based Application Recognition (NBAR)**               A Cisco router feature that looks at message details beyond the Layer 2, 3, and 4 headers to identify over 1000 different classifications of packets from different applications.

**Network Management System (NMS)**                 Software that manages the network, often using SNMP and other protocols.

**Network Time Protocol (NTP)**              A protocol used to synchronize time-of-day clocks so that multiple devices use the same time of day, which allows log messages to be more easily matched based on their timestamps.

**Next-generation firewall (NGFW)**        A firewall device with advanced features, including the ability to run many related security features in the same firewall device (IPS, malware detection, VPN termination), along with deep packet inspection with Application Visibility and Control (AVC) and the ability to perform URL filtering versus data collected about the reliability and risk associated with every domain name.

**Next-generation IPS (NGIPS)**                An IPS device with advanced features, including the capability to go beyond a comparison to known attack signatures to also look at contextual data, including the vulnerabilities in the current network, the capability to monitor for new zero-day threats, with frequent updates of signatures from the Cisco Talos security research group.

**Nexus 1000v**          A Cisco Nexus data center switch that runs as a software-only virtual switch inside one host (one hardware server), to provide switching features to the virtual machines running on that host.

**NMS**       Network Management Station. The device that runs network management software to manage network devices. SNMP is often the network management protocol used between the NMS and the managed device.

**northbound API**     In the area of SDN, a reference to the APIs that a controller supports that gives outside programs access to the services of the controller; for instance, to supply information about the network or to program flows into the network. Also called a northbound interface.

                   **northbound interface**            Another term for northbound API. _See_ _also_ northbound API.

**notification community**        An SNMP community (a value that acts as a password), defined on an SNMP manager, which then must be supplied by any SNMP agent that that sends the manager any unsolicited SNMP notifications (like SNMP Trap and Notify requests).

**NTP client**               Any device that attempts to use the Network Time Protocol (NTP) to synchronize its time by adjusting the local device’s time based on NTP messages received from a server.

OpenFlow  515

**NTP client/server mode** A mode of operation with the Network Time Protocol (NTP) in which the device acts as both an NTP client, synchronizing its time with some servers, and as an NTP server, supplying time information to clients.

**NTP primary server** A term defined in NTP RFCs 1305 and 5905 to refer to devices that act as NTP servers alone, with a stratum 1 external clock source.

**NTP secondary server** A term defined in NTP RFCs 1305 and 5905 to refer to devices that act as NTP clients and servers, synchronizing as a client to some NTP server, and then acting as an NTP server for other NTP clients.

**NTP server** Any device that uses Network Time Protocol (NTP) to help synchronize timeof-day clocks for other devices by telling other devices its current time.

**NTP synchronization** The process with the Network Time Protocol (NTP) by which different devices send messages, exchanging the devices’ current time-of-day clock information and other data, so that some devices adjust their clocks to the point that the time-of-day clocks list the same time (often accurate to at least the same second).

**NVRAM**   Nonvolatile RAM. A type of random-access memory (RAM) that retains its contents when a unit is powered off.

**O**

                      **ODL**      _See_ OpenDaylight.

**OID**         Object identifier. Used to uniquely describe an MIB variable in the SNMP database. This is a numeric string that identifies the variable uniquely and also describes where the variable exists in the MIB tree structure.

**on-demand self-service**         One of the five key attributes of a cloud computing service as defined by NIST, referring to the fact that the consumer of the server can request the service, with the service being created without any significant delay and without waiting on human

intervention.

**one-way delay**       The elapsed time from sending the first bit of data at the sending device until the last bit of that data is received on the destination device.

                     **ONF**        _See_ Open Networking Foundation.

                       **on-premises**          An alternate term for private cloud. _See also_ private cloud.

**Open Networking Foundation**              A consortium of SDN users and vendors who work together to foster the adoption of open SDN in the marketplace.

                       **OpenDaylight**            An open-source SDN controller, created by an open-source effort of the

OpenDaylight project under the Linux foundation, built with the intent to have a common SDN controller code base from which vendors could then take the code and add further features and support to create SDN controller products.

**OpenFlow**              The open standard for Software-Defined Networking (SDN) as defined by the Open Networking Foundation (ONF), which defines the OpenFlow protocol as well as the concept of an abstracted OpenFlow virtual switch.

516  operational management

**operational management**     A component of network management focused on extracting data about the network from the network devices, analyzing that data, and providing the data to operations staff.

**OpFlex**    The southbound protocol used by the Cisco ACI controller and the switches it controls.

**ordered data transfer** A networking function, included in TCP, in which the protocol defines how the sending host should number the data transmitted, defines how the receiving device should attempt to reorder the data if it arrives out of order, and specifies to discard the data if it cannot be delivered in order.

**origin hardware address** In both an ARP request and reply message, the field intended to be used to list the sender (origin) device’s hardware address, typically an Ethernet LAN address.

**origin IP address** In both an ARP request and reply message, the field intended to be used to list the sender (origin) device’s IP address.

**outside global** With source NAT, the one address used by the host that resides outside the enterprise, which NAT does not change, so there is no need for a contrasting term.

**overlay**   In SDA, the combination of VXLAN tunnels between fabric edge nodes as a data plane for forwarding frames, plus LISP for the control plane for the discovery and registration of endpoint identifiers.

**P**

**partial mesh**           A network topology in which more than two devices could physically communicate, but by choice, only a subset of the pairs of devices connected to the network is allowed to communicate directly.

**password guessing**                An attack where a malicious user simply makes repeated attempts to guess a user’s password.

**per-hop behavior (PHB)**        The general term used to describe the set of QoS actions a device can apply to a message from the time it enters a networking device until the device forwards the message. PHBs include classification, marking, queuing, shaping, policing, and congestion avoidance.

**permit**    An action taken with an ACL that implies that the packet is allowed to proceed through the router and be forwarded.

**pharming**               An attack that compromises name services to silently redirect users toward a malicious site.

**phishing** An attack technique that sends specially crafted emails to victims in the hope that the users will follow links to malicious websites.

**Platform as a Service (PaaS)** A cloud service intended for software developers as a development platform, with a variety of tools useful to developers already installed so that developers can focus on developing software rather than on creating a good development environment. power class  517

**PoE**         Power over Ethernet. Both a generalized term for any of the standards that supply power over an Ethernet link, as well as a specific PoE standard as defined in the IEEE 802.3af amendment to the 802.3 standard.

**point of presence (PoP)**         A term used for a service provider’s (SP) perspective to refer to a service provider’s installation that is purposefully located relatively near to customers, with several spread around major cities, so that the distance from each customer site to one of the SP’s PoPs is short.

                        **point-to-multipoint**       _See_ hub and spoke.

**point-to-point**        From a topology perspective, any topology that has two and only two devices that can send messages directly to each other.

**policing**   A QoS tool that monitors the bit rate of the messages passing some point in the processing of a networking device, so that if the bit rate exceeds the policing rate for a period of time, the policer can discard excess packets to lower the rate.

**policing rate**           The bit rate at which a policer compares the bit rate of packets passing through a policing function, for the purpose of taking a different action against packets that conform (are under) to the rate versus those that exceed (go over) the rate.

**policy model**          In both ACI and other intent-based networks (IBNs), the operational conventions (model) that combine policies of what the network will provide to grouped sets of network endpoints (endpoint groups) to create a contract for what the network will provide.

**port**         (Multiple definitions) (1) In TCP and UDP, a number that is used to uniquely identify the application process that either sent (source port) or should receive (destination port) data.

(2) In LAN switching, another term for switch interface.

**Port Address Translation (PAT)**             A NAT feature in which one inside global IP address supports over 65,000 concurrent TCP and UDP connections.

**port number**          A field in a TCP or UDP header that identifies the application that either sent (source port) or should receive (destination port) the data inside the data segment.

**port security**          A Cisco switch feature in which the switch watches Ethernet frames that come in an interface (a port), tracks the source MAC addresses of all such frames, and takes a security action if the number of different such MAC addresses is exceeded.

**port-scanner**          Jargon that refers to a security vulnerability during the time between the day in which the vulnerability was discovered, until the vendor or open-source group responsible for that software can develop a fix and make it public.

**power budget**        With PoE, data and calculations about the amount of power expected to be used by the various powered devices (PDs), the numbers of devices expected to connect to each switch, versus the amount of power available to PoE based on the capacity of the power supplies in the switches.

**power class**            In various PoE standards, a designation that can be sensed/identified via different discovery processes, with the class defining the maximum amount of power the powered device (PD) would like to receive over the Ethernet link.

518  Power over Ethernet (PoE)

**Power over Ethernet (PoE)** Both a generalized term for any of the standards that supply power over an Ethernet link and a specific PoE standard as defined in the IEEE 802.3af amendment to the 802.3 standard.

**Power over Ethernet Plus (PoE+)** A specific PoE standard as defined in the IEEE 802.3at amendment to the 802.3 standard, which uses two wire pairs to supply power with a maximum of 30 watts as supplied by the PSE.

**power sourcing equipment (PSE)** With any Power over Ethernet standard, a term that refers to the device supplying the power over the cable, which is then used by the powered device (PD) on the other end of the cable.

**powered device (PD)** With any Power over Ethernet standard, a term that refers to the device that receives or draws its power over the Ethernet cable, with the power being supplied by the power sourcing equipment (PSE) on the other end of the cable.

**Priority Code Point (PCP)** The formal term for the 3-bit field in the 802.IQ header intended for marking and classifying Ethernet frames for the purposes of applying QoS actions. Another term for Class of Service (CoS).

**priority queue** In Cisco queuing systems, another term for a low latency queue (LLQ).

**private cloud** A cloud computing service in which a company provides its own IT services to internal customers inside the same company but by following the practices defined as cloud computing.

**private IP network** Any of the IPv4 Class A, B, or C networks as defined by RFC 1918, intended for use inside a company but not used as public IP networks.

**private key** A secret value used in public/private key encryption systems. Either encrypts a value that can then be decrypted using the matching public key, or decrypts a value that was previously encrypted with the matching public key.

**programmable network** A computer network which provides programmatic interfaces that allow automation applications to change and interrogate the configuration of network devices.

**provider edge (PE)** A term used by service providers, both generally and also specifically in MPLS VPN networks, to refer to the SP device in a point of presence (PoP) that connects to the customer’s network and therefore sits at the edge of the SP’s network.

**public cloud** A cloud computing service in which the cloud provider is a different company than the cloud consumer.

**public key** A publicly available value used in public/private key encryption systems. Either encrypts a value that can then be decrypted using the matching private key, or decrypts a value that was previously encrypted with the matching private key.

**pull model** With configuration management tools, a practice by which an agent representing the device requests configuration data from the centralized configuration management tool, in effect pulling the configuration to the device.

**Puppet**   A popular configuration management application, which can be used with or without a server, using a pull model in which agents request details and pull configuration into devices, with the capability to manage network device configurations.

read-write community  519

**Puppet manifest**    A human-readable text file on the Puppet master, using a language defined by Puppet, used to define the desired configuration state of a device.

                       **Puppet master**           Another term for Puppet server. _See also_ Puppet server.

**Puppet server**        The Puppet software that collects all the configuration files and other files used by Puppet from different Chef users and then communicates with Puppet agents (devices) so that the agents can synchronize their configurations.

**Push model**            With configuration management tools, a practice by which the centralized configuration management tool software initiates the movement of configuration from that node to the device that will be configured, in effect pushing the configuration to the device.

**Python**    A programming language popular as a first language to learn and also popular for network automation tasks.

**Python dictionary** A Python variable like a JSON dictionary, containing a set of key:value pairs.

                       **Python list**         A Python variable like a JSON array, containing a list of values.

**Q–R**

**Quality of Experience (QoE)** The users’ perception of the quality of their experience in using applications in the network.

**Quality of Service (QoS)** The performance of a message, or the messages sent by an application, in regard to the bandwidth, delay, jitter, or loss characteristics experienced by the

message(s).

**queuing** The process by which networking devices hold packets in memory while waiting on some constrained resource; for example, when waiting for the outgoing interface to become available when too many packets arrive in a short period of time.

**RADIUS**   A security protocol often used for user authentication, including being used as part of the IEEE 802.lx messages between an 802.lx authenticator (typically a LAN switch) and a AAA server.

**RAM**        Random-access memory. A type of volatile memory that can be read and written by a microprocessor.

**rapid elasticity**       One of the five key attributes of a cloud computing service as defined by NIST, referring to the fact that the cloud service reacts to requests for new services quickly, and it expands (is elastic) to the point of appearing to be a limitless resource.

**read-only community**            An SNMP community (a value that acts as a password), defined on an SNMP agent, which then must be supplied by any SNMP manager that sends the agent any messages asking to learn the value of a variable (like SNMP Get and GetNext requests).

**read-write community**          An SNMP community (a value that acts as a password), defined on an SNMP agent, which then must be supplied by any SNMP manager that sends the agent any messages asking to set the value of a variable (like SNMP Set requests).

520  reconnaissance attack

**reconnaissance attack**           An attack crafted to discover as much information about a target organization as possible; the attack can involve domain discovery, ping sweeps, port scans, and so on.

**recursive DNS server**             A DNS server that, when asked for information it does not have, performs a repetitive (recursive) process to ask other DNS servers in sequence, hoping to find the DNS server that knows the information.

**reflection attack**    An attack that uses spoofed source addresses so that a destination machine will reflect return traffic to the attack’s target; the destination machine is known as the reflector.

**remote access VPN** A VPN for which one endpoint is a user device, such as a phone, tablet, or PC, typically created dynamically, and often using TLS. Also called a client VPN.

**Representational State Transfer (REST)** A type of API that allows two programs that reside on separate computers to communicate, with a set of six primary API attributes as defined early in this century by its creator, Roy Fielding. The attributes include client/server architecture, stateless operation, cachability, uniform interfaces, layered, and code-on-demand.

**resource pooling** One of the five key attributes of a cloud computing service as defined by NIST, referring to the fact that the cloud provider treats its resources as a large group (pool) of resources that its cloud management systems then allocate dynamically based on self-service requests by its customers.

                 **REST**        _See_ Representational State Transfer.

                  **REST API**          Any API that uses the rules of Representational State Transfer (REST).

                  **RESTful API**           A turn of phrase that means that the API uses REST rules.

**RFC**         Request For Comments. A document used as the primary means for communicating information about the TCP/IP protocols. Some RFCs are designated by the Internet Architecture Board (IAB) as Internet standards, and others are informational. RFCs are available online from numerous sources, including [www.rfc-editor.org.](http://www.rfc-editor.org/)

**root DNS server**     A small number of DNS servers worldwide that provide name resolution for the root zone of DNS, providing information about servers that know details about toplevel domains (TLDs) such as .com, .org, .edu, and so on.

**round robin**            A queue scheduling algorithm in which the scheduling algorithm services one queue, then the next, then the next, and so on, working through the queues in sequence.

**Round Trip Time (RTT)**          The time it takes a message to go from the original sender to the receiver, plus the time for the response to that message to be sent back.

**round-trip delay**    The elapsed time from sending the first bit of data at the sending device until the last bit of that data is received on the destination device, plus the time waiting for the destination device to form a reply, plus the elapsed time for that reply message to arrive back to the original sender.

**route redistribution**              A method by which two routing protocol processes running in the same device can exchange routing information, thereby causing a route learned by one routing protocol to then be advertised by another.

shaping rate  521

**routed access layer** A design choice in which all the switches, including the access layer switches that connect directly to endpoint devices, all use Layer 3 switching so that they route packets.

**Router on a Stick (ROAS)** Jargon to refer to the Cisco router feature of using VLAN trunking on an Ethernet interface, which then allows the router to route packets that happen to enter the router on that trunk and then exit the router on that same trunk, just on a different

VLAN.

**S**

                      **SBI**      _See_ Southbound API.

**scalable group**       In SDA, the concept of a set of related users that should have the equivalent security access. **scalable group tag (SGT)**      In SDA, a value assigned to the users in the same security group.

**Secure Shell (SSH)** A TCP/IP application layer protocol that supports terminal emulation between a client and server, using dynamic key exchange and encryption to keep the communications private.

**Secure Sockets Layer (SSL)**    A deprecated security protocol that was formerly used to secure networks and was commonly integrated into web browsers to provide encryption and authentication services between the browser and a website.

**segment** (Multiple definitions) (1) In TCP, a term used to describe a TCP header and its encapsulated data (also called an L4PDU). (2) Also in TCP, the set of bytes formed when TCP breaks a large chunk of data given to it by the application layer into smaller pieces that fit into TCP segments. (3) In Ethernet, either a single Ethernet cable or a single collision domain (no matter how many cables are used).

**service provider (SP)**             A company that provides a service to multiple customers. Used most often to refer to providers of private WAN services and Internet services. _See also_ Internet service provider.

**session key**             With encryption, a secret value that is known to both parties in a communication, used for a period of time, which the endpoints use when encrypting and decrypting data.

**SFTP**        SSH File Transfer Protocol. A file transfer protocol that assumes a secure channel, such as an encrypted SSH connection, which then provides the means to transfer files over the secure channel.

**shaping**   A QoS tool that monitors the bit rate of the messages exiting networking devices, so that if the bit rate exceeds the shaping rate for a period of time, the shaper can queue the packets, effectively slowing down the sending rate to match the shaping rate.

**shaping rate**           The bit rate at which a shaper compares the bit rate of packets passing through the shaping function, so that when the rate is exceeded, the shaper enables the queuing of packets, resulting in slowing the bit rate of the collective packets that pass through the shaper, so the rate of bits getting through the shaper does not exceed the shaping rate.

522  shared key

**shared key** A reference to a security key whose value is known (shared) by both the sender and receiver.

**shared port**            With 802.lw RSTP, a port type that is determined by the fact that the port uses half duplex, which could then imply a shared LAN as created by a LAN hub.

**Simple Network Management Protocol (SNMP)**                An Internet standard protocol for managing devices on IP networks. It is used mostly in network management systems to monitor network-attached devices for conditions that warrant administrative attention.

**simple variable**      In applications, a variable that has a single value of a simple type, such as text and integer or floating-point numbers.

**single point of failure**            In a network, a single device or link that, if it fails, causes an outage for a given population of users.

**site-to-site VPN**     The mechanism that allows all devices at two different sites to communicate securely over some unsecure network like the Internet, by having one device at each site perform encryption/decryption and forwarding for all the packets sent between the sites.

**sliding windows**     For protocols such as TCP that allow the receiving device to dictate the amount of data the sender can send before receiving an acknowledgment—a concept called a _window_—a reference to the fact that the mechanism to grant future windows is typically just a number that grows upward slowly after each acknowledgment, sliding upward.

                 **SNMP**         _See_ Simple Network Management Protocol.

**SNMP agent**           Software that resides on the managed device and processes the SNMP messages sent by the Network Management Station (NMS).

**SNMP community** A simple password mechanism in SNMP in which either the SNMP agent or manager defines a community string (password), and the other device must send that same password value in SNMP messages, or the messages are ignored. _See also_ read-only community, read-write community, and notification community.

                  **SNMP Get**          Message used by SNMP to read from variables in the MIB.

**SNMP Inform**         An unsolicited SNMP message like a Trap message, except that the protocol requires that the Inform message needs to be acknowledged by the SNMP manager.

**SNMP manager**      Typically a Network Management System (NMS), with this term specifically referring to the use of SNMP and the typical role of the manager, which retrieves status information with SNMP Get requests, sets variables with the SNMP Set requests, and receives unsolicited notifications from SNMP agents by listening for SNMP Trap and Notify messages.

**SNMP Set**               SNMP message to set the value in variables of the MIB. These messages are the key to an administrator configuring the managed device using SNMP.

**SNMP Trap**             An unsolicited SNMP message generated by the managed device, and sent to the SNMP manager, to give information to the manager about some event or because a measurement threshold has been passed.

**SNMPv2c**                A variation of the second version of SNMP. SNMP Version 2 did not originally support communities; the term _SNMPv2c_ refers to SNMP version 2 with support added for SNMP communities (which were part of SNMPvl).

spoofing attack  523

**SNMPv3** The third version of SNMP, with the notable addition of several security features as compared to SNMPv2c, specifically message integrity, authentication, and encryption.

**social engineering** Attacks that leverage human trust and social behaviors to divulge sensitive information.

**Software as a Service (SaaS)**                 A cloud service in which the service consists of access to working software, without the need to be concerned about the details of installing and maintaining the software or the servers on which it runs.

**Software-Defined Access**      Cisco’s intent-based networking (IBN) offering for enterprise networks.

**software-defined architecture**             In computer networking, any architecture that provides mechanisms for automated software control of the network components, typically using a controller. Any architecture that leads to a Software-Defined Network (SDN).

**Software-Defined Networking (SDN)**   A branch of networking that emerged in the marketplace in the 2010s characterized by the use of a centralized software controller that takes over varying amounts of the control plane processing formerly done inside networking devices, with the controller directing the networking elements as to what forwarding table entries to put into their forwarding tables.

**SOHO**      A classification of a business site with a relatively small number of devices, sometimes in an employee office in their home.

**Source NAT**            The type of Network Address Translation (NAT) used most commonly in networks (as compared to destination NAT), in which the source IP address of packets entering an inside interface is translated.

**southbound API**     In the area of SDN, a reference to the APIs used between a controller and the network elements for the purpose of learning information from the elements and for programming (controlling) the forwarding behavior of the elements. Also called a southbound interface. **southbound interface**       Another term for southbound API. _See_ _also_ southbound API.

**spear phishing**       Phishing that targets a group of users who share a common interest or connection.

**spine**       In an ACI network design for a single site, a switch that connects to leaf switches only, for the purpose of receiving frames from one leaf switch and then forwarding the frame to some other leaf switch.

**spine-leaf network**                A single-site network topology in which endpoints connect to leaf switches, leaf switches connect to all spine switches (but not to other leaf switches), and spine switches connect to all leaf switches (but not to other spine switches). The resulting topology results in predictable switching paths with three switches between any two endpoints that connect to different leaf switches.

**spoofing attack**      A type of attack in which parameters such as IP and MAC addresses are spoofed with fake values to disguise the sender.

524  spurious DHCP server

**spurious DHCP server** A DHCP server that is used by an attacker for attacks that take advantage of DHCP protocol messages.

                 **SSL**       _See_ Secure Sockets Layer.

**standard access list**               A list of IOS global configuration commands that can match only a packet’s source IP address for the purpose of deciding which packets to discard and which to allow through the router.

**star topology**         A network topology in which endpoints on a network are connected to a common central device by point-to-point links.

**stateful**   A protocol or process that requires information stored from previous transactions to perform the current transaction.

**stateless** A protocol or process that does not use information stored from previous transactions to perform the current transaction. **subinterface**          One of the virtual interfaces on a single physical interface.

**switch abstraction**                 The fundamental idea of what a switch does, in generalized form, so that standards protocols and APIs can be defined that then program a standard switch abstraction; a key part of the OpenFlow standard.

**syslog**     A server that takes system messages from network devices and stores them in a database. The syslog server also provides reporting capabilities on these system messages. Some syslog servers can even respond to select system messages with certain actions such as emailing and paging.

**syslog server**          A server application that collects syslog messages from many devices over the network and provides a user interface so that IT administrators can view the log messages to troubleshoot problems.

**T**

**T1**           A line from the telco that allows transmission of data at 1.544 Mbps, with the capability to treat the line as 24 different 64-Kbps DSO channels (plus 8 Kbps of overhead).

**T3**           A line from the telco that allows transmission of data at 44.736 Mbps, with the capability to treat the line as 28 different 1.544-Mbps DS1 (Tl) channels, plus overhead.

**TACACS+**                 A security protocol often used for user authentication as well as authorization and accounting, often used to authenticate users who log in to Cisco routers and switches.

**tail drop** Packet drops that occur when a queue fills, another message arrives that needs to be placed into the queue, and the networking device tries to add the new message to the tail of the queue but finds no room in the queue, resulting in a dropped packet.

**target hardware address**      In both an ARP request and reply message, the field intended to be used to list the destination (target) device’s hardware address, typically an Ethernet LAN address. This field is left as all binary 0s for typical ARP request messages.

top-level domain (TLD)  525

**target IP address** In both an ARP request and reply message, the field intended to be used to list the destination (target) device’s IP address.

                      **TCAM**        _See_ ternary content-addressable memory.

**TCP**          Transmission Control Protocol. A connection-oriented transport layer TCP/IP protocol that provides reliable data transmission.

**TCP window**           The mechanism in a TCP connection used by each host to manage how much data the receiver allows the sender to send to the receiver.

**TCP/IP**     Transmission Control Protocol/Internet Protocol. A common name for the suite of protocols developed by the U.S. Department of Defense in the 1970s to support the construction of worldwide internetworks. TCP and IP are the two best-known protocols in the suite. **telco**             A common abbreviation for telephone company.

**ternary content-addressable memory (TCAM)**   A type of physical memory, either in a separate integrated circuit or built into an ASIC, that can store tables and then be searched against a key, such that the search time happens quickly and does not increase as the size of the table increases. TCAMs are used extensively in higher-performance networking devices as the means to store and search forwarding tables in Ethernet switches and higher-performance routers.

**TFTP**        Trivial File Transfer Protocol. An application protocol that allows files to be transferred from one computer to another over a network, but with only a few features, making the software require little storage space.

**TFTP client**             An application that can connect to a TFTP server for the purpose of transferring copies of files to and from the server.

**TFTP server**            An application that runs and waits for TFTP clients to connect to it over UDP port 69 to support the client’s commands to transfer copies of files to and from the server. **threat**   An actual potential to use an exploit to take advantage of a vulnerability. **three-tier design**             _See_ core design.

**time interval (shaper)**           Part of the internal logic used by a traffic shaping function, which defines a short time period in which the shaper sends packets until a number of bytes are sent, and then the shaper stops sending for the rest of the time interval, with a goal of averaging a defined bit rate of sending data.

**TLD DNS server**      A DNS server with the role of identifying the IP address of the authoritative DNS server for a domain that resides within its top-level domain.

**Top of Rack (ToR) switch**       In a traditional data center design with servers in multiple racks and the racks in multiple rows, a switch placed in the top of the rack for the purpose of providing physical connectivity to the servers (hosts) in that rack.

**top-level domain (TLD)**         With DNS name services, the top-level domain is the most significant (rightmost) of the period-separated values in a DNS host name—for example, the .com within host name [www.example.com.](http://www.example.com/)

526  Transport Layer Security (TLS)

**Transport Layer Security (TLS)** A security standard that replaced the older Secure Sockets Layer (SSL) protocol, providing functions such as authentication, confidentiality, and message integrity over reliable in-order data streams like TCP. **trojan horse** Malware that is hidden and packaged inside other legitimate software.

**trust boundary** When thinking about a message as it flows from the source device to the destination device, the trust boundary is the first device the message reaches for which the QoS markings in the message’s various headers can be trusted as having an accurate value, allowing the device to apply the correct QoS actions to the message based on the marking.

**trusted port** With both the DHCP snooping and Dynamic ARP Inspection (DAI) switch features, the concept and configuration setting that tells the switch to allow all incoming messages of that respective type, rather than to consider the incoming messages (DHCP and ARP, respectively) for filtering.

**tunnel interface** A virtual interface in a Cisco router used to configure a variety of features, including Generic Routing Encapsulation (GRE), which encapsulates IP packets into other IP packets for the purpose of creating VPNs. **two-tier design**            _See_ collapsed core design.

**Type of Service (ToS)**             In the original definition of the IP header, a byte reserved for the purpose of QoS functions, including holding the IP Precedence field. The ToS byte was later repurposed to hold the DSCP field.

**U**

**UDP**        User Datagram Protocol. Connectionless transport layer protocol in the TCP/IP protocol stack. UDP is a simple protocol that exchanges datagrams without acknowledgments or guaranteed delivery.

**uncacheable**          For resources that might be repeatedly requested over time, an attribute that means that the requesting host should not use its local copy of the resource, but instead ask for a new copy every time the resource is required.

**underlay**                 In SDA, the network devices and links that create basic IP connectivity to support the creation of VXLAN tunnels for the overlay.

                     **Unified Computing System (UCS)**                   The Cisco brand name for its server hardware products.

**Universal Power over Ethernet (UPoE)**               A specific PoE standard as defined in the IEEE 802.3bt amendment to the 802.3 standard, which uses four wire pairs to supply power with a maximum of 60 watts as supplied by the PSE.

**Universal Power over Ethernet Plus (UPoE+)**      A specific PoE standard as defined in the IEEE 802.3bt amendment to the 802.3 standard, which uses four wire pairs to supply power with a maximum of 100 watts as supplied by the PSE.

**untrusted port**       With both the DHCP snooping and Dynamic ARP Inspection (DAI) switch features, the concept and configuration setting that tells the switch to analyze each incoming message of that respective type (DHCP and ARP) and apply some rules to decide whether to discard the message.

virtual MAC address (vMAC)  527

**UPoE**       Universal Power over Ethernet. A specific PoE standard as defined in IEEE 802.3bt amendment to the 802.3 standard, which uses four wire pairs to supply power with a maximum of 60 watts as supplied by the PSE.

**URI**          Uniform Resource Identifier. The formal and correct term for the formatted text used to refer to objects in an IP network. This text is commonly called a URL or a web address. For example, [http://www.certskills.com/blog](http://www.certskills.com/blog) is a URI that identifies the protocol (HTTP), host name [(www.certskills.com)](http://www.certskills.com/), and web page (blog).

**URI parameters** _See_ URI query (parameters).

**URI path (resource)**               In a URI, the part that follows the first /, up to the query field (which begins with a ?), which identifies the resource in the context of a server.

**URI query (parameters)**        In a URI, the part that follows the first ?, which provides a place to list variable names and values as parameters.

                       **URI resource**       _See_ URI path (resource).

**URL**         Uniform Resource Locator. The widely popular terms for the formatted text used to refer to objects in an IP network. For example, [http://www.certskills.com/blog](http://www.certskills.com/blog) is a URL that identifies the protocol (HTTP), host name [(www.certskills.com)](http://www.certskills.com/), and web page (blog).

**user network interface (UNI)**                A term used in a variety of WAN standards, including carrier/Metro Ethernet, that defines the standards for how a customer device communicates with a service provider’s device over an access link.

**username secret**    A reference to the password configured on the **username** _name_ **secret** _pass-value_ command, which defines a username and an encoded password, used to build a local username/password list on the router or switch.

**V**

**variable** In applications, a method to assign a name to a value so that the application can refer to the value, change it, compare it to other values, apply logic, and perform other actions typical of software applications.

**version control software**       Applications that monitor files for changes, tracking each specific change, the user, the date/time, with tools so that users can compare versions of each file through its history to see the differences.

**violation mode**      In port security, a configuration setting that defines the specific set of actions to take on a port when a port security violation occurs. The modes are shutdown, restrict, and protect.

**virtual CPU (vCPU)**                 In a virtualized server environment, a CPU (processor) core or thread allocated to a virtual machine (VM) by the hypervisor.

**virtual IP address**   For any FHRP protocol, an IP address that the FHRP shares between multiple routers so that they appear as a single default router to hosts on that subnet.

**virtual MAC address (vMAC)**                For any FHRP protocol, a MAC address that the FHRP uses to receive frames from hosts.

528  virtual machine

**virtual machine**     An instance of an operating system, running on server hardware that uses a hypervisor to allocate a subset of the server hardware (CPU, RAM, disk, and network) to that

VM.

**virtual network function (VNF)** Any function done within a network (for example, router, switch, firewall) that is implemented not as a physical device but as an OS running in a virtualized system (for instance, a VM).

**virtual network identifier (VNID)** In SDA and VXLAN, the identifier for a separate routing and switching instance. All devices in the same VNID are considered to be allowed to send data to each other unless prevented from doing so by other security mechanisms.

**virtual NIC (vNIC)** In a virtualized server environment, a network interface card (NIC) used by a virtual machine, which then connects to some virtual switch (vSwitch) running on that same host, which in turn connects to a physical NIC on the host.

**virtual private network (VPN)** A set of security protocols that, when implemented by two devices on either side of an unsecure network such as the Internet, can allow the devices to send data securely. VPNs provide privacy, device authentication, anti-replay services, and data integrity services.

**Virtual Router Redundancy Protocol (VRRP)** A TCP/IP RFC protocol that allows two (or more) routers to share the duties of being the default router on a subnet, with an active/ standby model, with one router acting as the default router and the other sitting by waiting to take over that role if the first router fails.

**virtual switch (vSwitch)** A software-only virtual switch inside one host (one hardware server), to provide switching features to the virtual machines running on that host.

**virus**       Malware that injects itself into other applications and then propagates through user intervention.

                 **VPN**      _See_ virtual private network.

**VPN client**              Software that resides on a PC, often a laptop, so that the host can implement the protocols required to be an endpoint of a VPN. **vulnerability**         A weakness that can be used to compromise security.

**VXLAN**    Virtual Extensible LAN. A flexible encapsulation protocol used for creating tunnels (overlays).

**W**

**WAN edge**              The device (typically a router) at enterprise sites that connects to private WAN links, therefore sitting at the edge of the WAN.

                 **WAN link**        Another term for leased line.

**WAN service provider**           A company that provides private WAN services to customers; the company may have a heritage as a telco or cable company.

zero-day vulnerability  529

**watering hole attack** An attack where a site frequently visited by a group of users is compromised; when the target users visit the site, they will be infected with malware, but other users will not.

**web server** Software, running on a computer, that stores web pages and sends those web pages to web clients (web browsers) that request the web pages.

**well-known port** A TCP or UDP port number reserved for use by a particular application. The use of well-known ports allows a client to send a TCP or UDP segment to a server, to the correct destination port for that application.

**whaling**   A phishing technique that targets high-profile individuals to follow links to malicious sites.

**wildcard mask**       The mask used in Cisco IOS ACL commands and OSPF and EIGRP **network** commands.