# Elevate Labs Cybersecurity Internship - Task 1: Local Network Port Scan

## Objective
The objective of this task was to learn to discover open ports on devices in my local network to understand network exposure.

## Tools Used
* Nmap


## Steps Performed

1.  **Tool Installation:**
    * ![image](https://github.com/user-attachments/assets/67e0d8dc-5bf3-4a89-ab25-ddcf797b76e2)

      Confirmed Nmap was already installed on Kali Linux.


3.  **Identified Local IP Range:**
    * ![image](https://github.com/user-attachments/assets/9648dc28-b50e-4172-801f-79fbe5fa7023)

      Used `ifconfig` to identify the IP address of `eth0` as `10.0.0.200` with a netmask of `255.255.255.0`.
    *![image](https://github.com/user-attachments/assets/6b253a6a-3d24-427c-92aa-67f609812125)

      Used `route -n` to confirm the network range. The relevant entry showed `10.0.0.2` with a `Genmask` of `255.255.255.0` via `eth0`.
    * From this, the local IP range was determined to be `10.0.0.2/24`.

4.  **Performed TCP SYN Scan using Nmap:**
    * Executed the following command to scan the local network for open ports, outputting results to an XML file (`scan.xml`):
        ```bash
        sudo nmap -sS -T4 -oX scan.xml 10.0.0.2/24
        ```
        *Note: The Nmap command used `10.0.0.2/24` as the target range, which includes the identified gateway `10.0.0.2` and your machine `10.0.0.200`.*

5.  **Generated HTML Report from XML Output:**
    * Downloaded the `nmap-bootstrap.xsl` file to enhance the visual presentation of the Nmap XML output:
        ```bash
        wget [https://raw.githubusercontent.com/honze-net/nmap-bootstrap-xsl/master/nmap-bootstrap.xsl](https://raw.githubusercontent.com/honze-net/nmap-bootstrap-xsl/master/nmap-bootstrap.xsl)
        ```
    * Transformed the `scan.xml` file into an HTML report (`scanresult.html`) using `xsltproc`:
        ```bash
        xsltproc nmap-bootstrap.xsl scan.xml > scanresult.html
        ```

6.  **Analyzed Scan Results and Identified Services/Risks:**
    * **Port 53 (DNS) was found open on `10.0.0.2` (the gateway).**
        * **Potential Risk:** An open DNS port could potentially be vulnerable to DNS amplification attacks or cache poisoning if not properly secured.
    * **Port 22 (SSH) was found open on `10.0.0.202`.**
        * **Potential Risk:** An open SSH port is a common target for brute-force attacks. Strong passwords, key-based authentication, and limiting access are crucial for securing SSH.

## Scan Results Summary
![image](https://github.com/user-attachments/assets/67922c9d-5e54-48df-8aff-1fd64ade049d)


* The gateway device (`10.0.0.2`) has Port 53 (DNS) open.
* Another device on the network (`10.0.0.202`) has Port 22 (SSH) open.
* Detailed scan results can be found in `scan.xml` and the more user-friendly `scanresult.html`.


## Conclusion/Learnings
This task provided practical experience in network reconnaissance and understanding network service exposure. I successfully used Nmap to perform a TCP SYN scan, identified open ports on devices within my local network, and learned to research common services associated with these ports. Furthermore, I gained insight into the potential security risks posed by open ports, such as DNS vulnerabilities and SSH brute-force attacks. The process of converting Nmap XML output to a presentable HTML format was also a valuable learning experience.

---

**Files included in this repository:**
* `README.md`: This file.
* `scan.xml`: The raw Nmap XML output.
* `scanresult.html`: The Nmap scan results in an HTML format for better readability.
