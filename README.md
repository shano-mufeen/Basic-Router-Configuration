# 🌐 Cisco Router Basic Configuration – Cisco Packet Tracer


A hands-on **Cisco Packet Tracer** project demonstrating the **top 15 basic router configuration tasks** using a Cisco 2911 router and Cisco IOS CLI.

This project covers fundamental router configuration, device security, user authentication, password protection, time configuration, brute-force protection, and configuration verification.

---

## 📌 Overview

This lab demonstrates how to perform essential Cisco IOS router configurations from a PC using a **console connection**.

The project uses a **Cisco 2911 router** connected to a PC through a console cable. The PC is used to access the router's CLI through:

```text
PC → Desktop → Terminal
```

The configuration tasks covered in this project include:

* Cisco IOS user levels
* Hostname configuration
* MOTD banner
* Enable password
* Console line security
* VTY line security
* EXEC timeout
* Login synchronization
* DNS lookup disabling
* Domain name configuration
* Local username and password
* Password encryption
* Router clock configuration
* Login brute-force protection
* Running and startup configuration verification
* Configuration saving

---

# 🎯 Project Objectives

The main objectives of this project are to:

* Understand Cisco IOS CLI configuration modes
* Configure a Cisco 2911 router
* Navigate between IOS user levels
* Configure a router hostname
* Configure a MOTD security banner
* Secure privileged EXEC mode
* Secure console access
* Configure VTY remote-access lines
* Configure session timeout settings
* Disable unnecessary DNS lookups
* Configure a domain name
* Create a local user account
* Encrypt configured passwords
* Configure the router's clock
* Protect against repeated failed login attempts
* Verify running and startup configurations
* Save the router configuration to NVRAM

---

# 🖥️ Network Topology

The lab uses a simple topology consisting of:

* One Cisco 2911 router
* One PC
* One console cable



<img width="3336" height="2082" alt="Screenshot" src="https://github.com/user-attachments/assets/5e784c55-38b6-4b38-b99a-2623224cfb9f" />



---

# 🔌 Devices Used

| Device    | Model / Type  | Purpose                          |
| --------- | ------------- | -------------------------------- |
| 🖧 Router | Cisco 2911    | Main router                      |
| 💻 PC     | End Device    | Console access and configuration |
| 🔌 Cable  | Console Cable | Connects PC to router console    |

---

# ⚙️ Configuration Process

## 1. Establish Console Connection

Connect the PC to the router using a console cable.

```text
PC RS232 ───────── Console ───────── Cisco 2911
```

In Cisco Packet Tracer:

```text
PC → Desktop → Terminal
```

Leave the default terminal settings unchanged and select **OK**.

If the router displays:

```text
Would you like to enter the initial configuration dialog? [yes/no]:
```

Enter:

```text
no
```

You can now access the router's Cisco IOS CLI.

---

# 2. Navigate Cisco IOS User Levels

Cisco IOS provides different configuration modes.

### User EXEC Mode

```text
Router>
```

This is the initial mode after accessing the router.

To enter Privileged EXEC Mode:

```bash
Router> enable
Router#
```

### Privileged EXEC Mode

```text
Router#
```

Privileged EXEC mode provides access to additional monitoring and configuration commands.

To enter Global Configuration Mode:

```bash
Router# configure terminal
Router(config)#
```

### Global Configuration Mode

```text
Router(config)#
```

This mode is used for configuring the router.

### IOS Mode Summary

```text
User EXEC
     │
     │ enable
     ▼
Privileged EXEC
     │
     │ configure terminal
     ▼
Global Configuration
```

---

# 3. Configure Hostname

The default router hostname can be changed to make the device easier to identify.

```bash
Router(config)# hostname R1
R1(config)#
```

The router prompt changes from:

```text
Router(config)#
```

to:

```text
R1(config)#
```

---

# 4. Configure MOTD Banner

The **Message of the Day (MOTD)** banner displays a message when users access the device.

Example:

```bash
R1(config)# banner motd #Authorized Access Only#
```

The message can be used to provide:

* Security warnings
* Access information
* Administrative notices
* Device identification

Example:

```text
****************************************
*      Authorized Access Only          *
*          Test Router R1              *
****************************************
```

---

# 5. Configure Enable Password

The enable password protects access to **Privileged EXEC Mode**.

```bash
R1(config)# enable password <PASSWORD>
```

Example:

```bash
R1(config)# enable password Cisco
```

The password is required when a user attempts to move from:

```text
R1>
```

to:

```text
R1#
```

### Recommended Alternative

For real-world configurations, `enable secret` is preferred:

```bash
R1(config)# enable secret <PASSWORD>
```

---

# 6. Configure Console Password

The console line provides local physical access to the router.

Enter console configuration mode:

```bash
R1(config)# line console 0
```

Configure a password:

```bash
R1(config-line)# password <PASSWORD>
R1(config-line)# login
```

Example:

```bash
R1(config)# line console 0
R1(config-line)# password Cisco
R1(config-line)# login
```

The `login` command tells the router to require the configured line password.

---

# 7. Configure EXEC Timeout

An EXEC timeout automatically disconnects inactive sessions after a specified period.

Example:

```bash
R1(config-line)# exec-timeout 4 0
```

This means:

```text
4 minutes
0 seconds
```

After four minutes of inactivity, the session is terminated.

---

# 8. Configure Login Synchronous

The `logging synchronous` command prevents system messages from interrupting CLI input.

```bash
R1(config-line)# logging synchronous
```

Complete console configuration:

```bash
R1(config)# line console 0
R1(config-line)# password Cisco
R1(config-line)# login
R1(config-line)# exec-timeout 4 0
R1(config-line)# logging synchronous
```

---

# 9. Configure VTY Lines

VTY lines are used for remote access to the router.

On this router, the VTY range is:

```text
0–15
```

This represents **16 VTY lines**.

Configure them using:

```bash
R1(config)# line vty 0 15
R1(config-line)# password <PASSWORD>
R1(config-line)# login
R1(config-line)# exec-timeout 2 20
R1(config-line)# logging synchronous
```

Example:

```bash
R1(config)# line vty 0 15
R1(config-line)# password Cisco
R1(config-line)# login
R1(config-line)# exec-timeout 2 20
R1(config-line)# logging synchronous
```

> **Security recommendation:** SSH should be preferred over Telnet for remote administration because Telnet does not encrypt credentials.

---

# 10. Disable IP Domain Lookup

Cisco IOS may attempt DNS resolution when an invalid command is entered.

For example, a mistyped command may cause the router to attempt to resolve it as a hostname.

Disable this behavior with:

```bash
R1(config)# no ip domain lookup
```

### Purpose

```text
no ip domain lookup
        ↓
Prevents unnecessary DNS lookups
        ↓
Avoids delays caused by mistyped commands
```

---

# 11. Configure Domain Name

A domain name can be configured for the router.

Example:

```bash
R1(config)# ip domain-name cisco.com
```

This configuration is also commonly required when preparing a Cisco device for SSH key generation.

---

# 12. Configure Local Username and Password

A local user account can be created for device authentication.

```bash
R1(config)# username admin password Cisco
```

Example:

```text
Username: admin
Password: Cisco
```

A stronger alternative is:

```bash
R1(config)# username admin secret <PASSWORD>
```

The local user database can then be used for authentication, particularly with SSH.

---

# 13. Encrypt Passwords

Passwords configured with commands such as `password` can otherwise appear in readable form in the configuration.

Enable password encryption with:

```bash
R1(config)# service password-encryption
```

Before encryption:

```text
password Cisco
```

After encryption:

```text
password 7 <encrypted-value>
```

Verify the configuration:

```bash
R1# show running-config
```

> **Important:** `service password-encryption` provides basic obfuscation. It is not considered strong password protection. Use `secret`-based credentials where possible.

---

# 14. Configure Current Clock Time

The router's clock can be configured from **Privileged EXEC Mode**.

First exit Global Configuration Mode:

```bash
R1(config)# exit
R1#
```

Then use:

```bash
R1# clock set HH:MM:SS MONTH DAY YEAR
```

Example:

```bash
R1# clock set 17:30:45 March 1 2023
```

Verify the clock:

```bash
R1# show clock
```

Example:

```text
17:30:45.123 UTC Sat Mar 1 2023
```

---

# 15. Prevent Brute-Force Login Attempts

Repeated login attempts can be used in brute-force attacks to guess passwords.

Cisco IOS provides a login blocking feature.

Example:

```bash
R1(config)# login block-for 180 attempts 3 within 50
```

This configuration means:

```text
3 failed login attempts
        ↓
within 50 seconds
        ↓
Block login attempts
        ↓
for 180 seconds
```

### Configuration Breakdown

| Parameter    | Meaning                    |
| ------------ | -------------------------- |
| `180`        | Blocking period in seconds |
| `attempts 3` | Number of failed attempts  |
| `within 50`  | Time window in seconds     |

This provides a basic defense against repeated login attempts.

---

# 🔍 Verification

## Verify Running Configuration

The running configuration is stored in **RAM** and represents the current active configuration.

Use:

```bash
R1# show running-config
```

Short form:

```bash
R1# show run
```

While in a configuration mode, use:

```bash
R1(config)# do show running-config


---

# 💾 Verify Startup Configuration

The startup configuration is the saved configuration stored in **NVRAM**.

Use:

```bash
R1# show startup-config
```

Short form:

```bash
R1# show start
```

If the configuration has not been saved, you may see:

```text
startup-config is not present
```

### Running vs Startup Configuration

| Configuration  | Stored In | Purpose                           |
| -------------- | --------- | --------------------------------- |
| Running Config | RAM       | Current active configuration      |
| Startup Config | NVRAM     | Configuration loaded after reboot |

---

# 💾 Save the Configuration

The running configuration should be saved to NVRAM so it survives a router restart.

Use:

```bash
R1# write
```

Or the more explicit command:

```bash
R1# copy running-config startup-config
```

After saving:

```text
Running Configuration
        │
        │ copy running-config startup-config
        ▼
Startup Configuration
        │
        ▼
       NVRAM
```

Verify the saved configuration:

```bash
R1# show startup-config
```

---

# 📊 Top 15 Configuration Summary

|  # | Configuration                     | Purpose                                       |
| -: | --------------------------------- | --------------------------------------------- |
|  1 | Navigate User Levels              | Understand IOS configuration modes            |
|  2 | Hostname                          | Identify the router                           |
|  3 | MOTD Banner                       | Display an access/security message            |
|  4 | Enable Password                   | Protect Privileged EXEC mode                  |
|  5 | Console Password                  | Secure console access                         |
|  6 | EXEC Timeout                      | Disconnect inactive sessions                  |
|  7 | Logging Synchronous               | Prevent CLI interruption from system messages |
|  8 | VTY Password                      | Secure remote access lines                    |
|  9 | Disable IP Domain Lookup          | Prevent unnecessary DNS lookups               |
| 10 | Domain Name                       | Configure the router's domain                 |
| 11 | Local Username                    | Provide local authentication                  |
| 12 | Password Encryption               | Obfuscate line passwords                      |
| 13 | Clock Configuration               | Set the router's date and time                |
| 14 | Login Blocking                    | Reduce brute-force login attempts             |
| 15 | Configuration Verification & Save | Verify and preserve configuration             |

---

# 🧪 Verification Commands

The following commands can be used to verify the configuration:

```bash
show running-config
show startup-config
show clock
```

Additional useful commands:

```bash
show ip interface brief
show users
show login
```

---

# 🧰 Technologies & Tools

* 🟢 **Cisco Packet Tracer**
* 🖧 **Cisco 2911 Router**
* 💻 **Cisco IOS CLI**
* 🌐 **IPv4 Networking**
* 🔌 **Console Communication**
* 🔐 **Network Security**
* 🛡️ **Password Protection**
* 📡 **Remote Device Management**

---

# 🧠 Skills Learned

Through this project, I gained practical experience in:

* Cisco IOS CLI navigation
* User EXEC and Privileged EXEC modes
* Global Configuration Mode
* Router hostname configuration
* MOTD banner configuration
* Console line security
* VTY line configuration
* Session timeout configuration
* Local user authentication
* Password protection
* DNS lookup control
* Domain configuration
* Router clock configuration
* Brute-force protection
* Running and startup configuration management
* NVRAM configuration storage
* Basic Cisco network troubleshooting

---

# 📥 How to Use This Project

## 1. Clone the Repository

```bash
git clone <REPOSITORY-URL>
```

## 2. Open Cisco Packet Tracer

Launch **Cisco Packet Tracer** on your computer.

## 3. Open the Project

Open:

```text
Cisco-Router-Basic-Configuration.pkt
```

## 4. Explore the Topology

Review the Cisco 2911 router and PC connection.

## 5. Access the Router

From the PC:

```text
Desktop → Terminal
```

## 6. Review the Configuration

Use Cisco IOS commands such as:

```bash
show running-config
show startup-config
show clock
show ip interface brief
```

---

# 📄 Project Files

### 📦 Cisco Packet Tracer File

The `.pkt` file contains the complete Cisco Packet Tracer topology and router configuration.

### 📷 Screenshots

The `screenshots/` directory contains screenshots demonstrating:

* Network topology
* Console access
* Router configuration
* Running configuration
* Startup configuration
* Clock configuration

---

# 🏁 Conclusion

This project provided hands-on experience with **basic Cisco router configuration and security using Cisco Packet Tracer**.

By completing this lab, I strengthened my understanding of:

* Cisco IOS CLI
* Router configuration modes
* Device identification
* Console and VTY security
* Password protection
* Local authentication
* DNS lookup control
* Router time configuration
* Login attack protection
* Configuration verification
* RAM and NVRAM configuration storage
* Configuration backup and persistence

This project represents a **Networking Fundamentals** lab focused on the **top 15 basic Cisco router configuration tasks**.

---

## ⭐ Project Status

🟢 **Completed**

| Category     | Details                               |
| ------------ | ------------------------------------- |
| **Platform** | Cisco Packet Tracer                   |
| **Device**   | Cisco 2911 Router                     |
| **Level**    | Networking Fundamentals               |
| **Focus**    | Basic Router Configuration & Security |
| **Status**   | Completed                             |

---

## 👨‍💻 Author

**Your Name**

If you found this project useful, consider giving the repository a ⭐.
