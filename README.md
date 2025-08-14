<h1>CCNA Routing, Security, and Subnetting Lab</h1>

<h2>1. Objective</h2>
<p>This project is a comprehensive hands-on lab designed in GNS3 to demonstrate foundational networking concepts critical for the CCNA certification. The lab showcases the entire lifecycle of a small enterprise network: from initial design and IP subnetting to implementation of dynamic routing, security hardening with ACLs, and secure remote management with SSH.</p>

<hr>

<h2>2. Network Topology</h2>

<img width="1516" height="785" alt="tptp" src="https://github.com/user-attachments/assets/15cabac0-97c6-4dd2-9010-7f6d7a0d0d17" />

<hr>

<h2>3. Features & Skills Demonstrated</h2>
<ul>
    <li><b>Network Design & Subnetting (VLSM):</b> The entire <code>192.168.1.0/24</code> address space was subnetted using <strong>Variable Length Subnet Masking (VLSM)</strong> to efficiently allocate IP addresses for both point-to-point WAN links (<code>/30</code>) and user LANs (<code>/29</code>).</li>
    <li><b>Dynamic Routing (RIPv2):</b> <strong>RIP version 2</strong> was configured across all routers to enable automatic route discovery and ensure full network connectivity. <code>no auto-summary</code> was used as a best practice for classless routing.</li>
    <li><b>Network Security (Access Control Lists):</b> A standard numbered ACL was implemented on Router R3 to filter traffic at the source, demonstrating traffic control policies by blocking a specific host (PC2) while permitting all other traffic.</li>
    <li><b>Secure Remote Management (SSH):</b> Router R2 was configured for secure remote access via <strong>SSH</strong>, including setting a hostname, domain name, generating RSA crypto keys, creating a local user database, and enforcing SSH on the VTY lines.</li>
    <li><b>Verification and Troubleshooting:</b> Full end-to-end connectivity was verified using <code>ping</code>. The routing tables (<code>show ip route</code>) and ACL match counters (<code>show access-list</code>) were checked to confirm proper network operation.</li>
</ul>

<hr>

<h2>4. IP Addressing Scheme</h2>
<table border="1" style="width:100%; border-collapse: collapse;">
  <thead>
    <tr>
      <th style="padding: 8px; text-align: left;">Network Name</th>
      <th style="padding: 8px; text-align: left;">Used For</th>
      <th style="padding: 8px; text-align: left;">Subnet ID & Mask</th>
      <th style="padding: 8px; text-align: left;">Usable IP Range</th>
      <th style="padding: 8px; text-align: left;">Gateway/Device IPs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px;"><b>LAN A</b></td>
      <td style="padding: 8px;">R3 LAN</td>
      <td style="padding: 8px;"><code>192.168.1.0 /29</code></td>
      <td style="padding: 8px;"><code>.1</code> - <code>.6</code></td>
      <td style="padding: 8px;"><b>R3 (f1/0):</b> <code>.1</code>, <b>PCs:</b> <code>.2, .3, .4</code></td>
    </tr>
    <tr>
      <td style="padding: 8px;"><b>LAN B</b></td>
      <td style="padding: 8px;">R4 LAN</td>
      <td style="padding: 8px;"><code>192.168.1.8 /29</code></td>
      <td style="padding: 8px;"><code>.9</code> - <code>.14</code></td>
      <td style="padding: 8px;"><b>R4 (f1/0):</b> <code>.9</code>, <b>PCs:</b> <code>.10, .11, .12</code></td>
    </tr>
    <tr>
      <td style="padding: 8px;"><b>WAN Link 1</b></td>
      <td style="padding: 8px;">R1 to R2</td>
      <td style="padding: 8px;"><code>192.168.1.16 /30</code></td>
      <td style="padding: 8px;"><code>.17</code> - <code>.18</code></td>
      <td style="padding: 8px;"><b>R1 (f1/0):</b> <code>.17</code>, <b>R2 (f0/0):</b> <code>.18</code></td>
    </tr>
     <tr>
      <td style="padding: 8px;"><b>WAN Link 2</b></td>
      <td style="padding: 8px;">R2 to R3</td>
      <td style="padding: 8px;"><code>192.168.1.20 /30</code></td>
      <td style="padding: 8px;"><code>.21</code> - <code>.22</code></td>
      <td style="padding: 8px;"><b>R2 (f1/0):</b> <code>.21</code>, <b>R3 (f0/0):</b> <code>.22</code></td>
    </tr>
     <tr>
      <td style="padding: 8px;"><b>WAN Link 3</b></td>
      <td style="padding: 8px;">R2 to R4</td>
      <td style="padding: 8px;"><code>192.168.1.24 /30</code></td>
      <td style="padding: 8px;"><code>.25</code> - <code>.26</code></td>
      <td style="padding: 8px;"><b>R2 (f1/1):</b> <code>.25</code>, <b>R4 (f0/0):</b> <code>.26</code></td>
    </tr>
     <tr>
      <td style="padding: 8px;"><b>Cloud Link</b></td>
      <td style="padding: 8px;">R1 to Cloud</td>
      <td style="padding: 8px;"><code>192.168.1.28 /30</code></td>
      <td style="padding: 8px;"><code>.29</code> - <code>.30</code></td>
      <td style="padding: 8px;"><b>Cloud (PC):</b> <code>.29</code>, <b>R1 (f0/0):</b> <code>.30</code></td>
    </tr>
  </tbody>
</table>

<hr>

<h2>5. Device Configurations</h2>

<h3>Router R1</h3>
<pre><code>hostname R1
!
interface FastEthernet0/0
ip address 192.168.1.30 255.255.255.252
no shutdown
!
interface FastEthernet1/0
ip address 192.168.1.17 255.255.255.252
no shutdown
!
router rip
version 2
network 192.168.1.0
no auto-summary
!
end
</code></pre>

<h3>Router R2 (SSH Enabled)</h3>
<pre><code>hostname R2
!
ip domain-name mylab.local
username admin secret MySecurePa$$w0rd
!
crypto key generate rsa
!
interface FastEthernet0/0
ip address 192.168.1.18 255.255.255.252
no shutdown
!
interface FastEthernet1/0
ip address 192.168.1.21 255.255.255.252
no shutdown
!
interface FastEthernet1/1
ip address 192.168.1.25 255.255.255.252
no shutdown
!
router rip
version 2
network 192.168.1.0
no auto-summary
!
line vty 0 4
login local
transport input ssh
!
end
</code></pre>

<h3>Router R3 (Standard-ACL Enabled)</h3>
<pre><code>hostname R3
!
interface FastEthernet0/0
ip address 192.168.1.22 255.255.255.252
no shutdown
!
interface FastEthernet1/0
ip address 192.168.1.1 255.255.255.248
ip access-group 1 in
no shutdown
!
access-list 1 deny host 192.168.1.3
access-list 1 permit any
!
router rip
version 2
network 192.168.1.0
no auto-summary
!
end
</code></pre>

<h3>Router R4</h3>
<pre><code>hostname R4
!
interface FastEthernet0/0
ip address 192.168.1.26 255.255.255.252
no shutdown
!
interface FastEthernet1/0
ip address 192.168.1.9 255.255.255.248
no shutdown
!
router rip
version 2
network 192.168.1.0
no auto-summary
!
end
</code></pre>

<h3>VPCS Host Configurations</h3>
<pre><code># On R3's LAN (Gateway: 192.168.1.1)
PC1> ip 192.168.1.2/29 192.168.1.1
PC2> ip 192.168.1.3/29 192.168.1.1
PC3> ip 192.168.1.4/29 192.168.1.1

# On R4's LAN (Gateway: 192.168.1.9)
PC4> ip 192.168.1.10/29 192.168.1.9
PC5> ip 192.168.1.11/29 192.168.1.9
PC6> ip 192.168.1.12/29 192.168.1.9
</code></pre>

<hr>
<p><i>This lab was built and tested in GNS3 version 2.2.54</i></p>
