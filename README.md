# systemd-pcap
for long running pcaps

Steps:


1. Run df -h to get amount of free disk space
2. Place pcap-tool@.service file in /etc/systemd/system/
3. chmod 644 /etc/systemd/system/pcap-tool@.service
4. Start and stop using systemctl start|stop pcap-tool@<interface name>
5. The default settings will roitate the the pcap file every 25MB
6. Once the captures have started recording check the time taken to record 2 25MB files
7. Work out disk space needed to capture traffic for X days do:
    (total run time needed (minutes e.g. days * 24 * 60) / time taken to record a 25 MB file (minutes)) * 25 MB
8. Compression setting in file is gzip
