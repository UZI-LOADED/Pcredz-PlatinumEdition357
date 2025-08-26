# Pcredz-PlatinumEdition357💯
💻Remastered by the Pros---> Platinum Edition357🔫  fully loaded PCredz scans network traffic for credentials. When it finds one, it logs it to a file, sends it to syslog, and updates the dashboard in real time.
🤖How 2 deploy everything-->>> (Layman’s Terms)
1. Install dependencies (do this once)--->  pip3 install flask requests gunicorn

2. Start the dashboard---->  chmod +x start_dashboard.sh
./start_dashboard.sh   <---
This runs the dashboard at http://localhost:5000 (or your server’s IP).

3. Run PCredz--->   python3 Pcredz -f yourfile.pcap
# or for live capture
sudo python3 Pcredz -i eth0   <---
Credentials will show up in the dashboard, syslog, and log files.

4. (Optional) Disable dashboard integration--->  export PCREDZ_ENABLE_DASHBOARD=0   <---
Run this before running PCredz if you don’t want dashboard integration.
    


=====>>> 🔫When it's deploy--->💻
💀PCredz scans network traffic for credentials.
When it finds one, it logs it to a file, sends it to syslog, and updates the dashboard in real time.
You can view credentials live in your browser and get alerts via your SIEM.
 
 +++> Don't thank me pay me $$$💰🤑🫰💵💶💸💳💴🪙
