# systemd-packet-capture
Safely run long packet captures

Steps:
1. Run df -h and note the amount of free disk space on root file directory.
2. Insert file /etc/systemd/system/packet-capture.environment
3. Insert file /etc/systemd/system/packet-capture@.service
4. chmod 644 both files
5. Edit packet-capture@.service and ensure BPF filter is specific as possible to the traffic you want to capture 
6. Start the capture using systemctl start pcap-tool@\<interface name\> and run for approx 20 minutes. 
7  Stop capture systemctl stop pcap-tool@<interface name>
8. Calculate total disk space needed for your long running capture
    * Run ls -hal on the /var/spool directory to see the size of the rotated gz files generated 
    * Total disk space formula in MB
        capture duration in minutes (e.g. 30 days is 43200 ) * ((MB of gz file capture after 20 min run) / 20)
9. Edit pcap-tool@.service and set:
    safety max FILESIZE for pre zipped rotated files e.g. 4 hour files should generate no more than 150 MB files
    Rotation in seconds e.g. 4 hours = 14400 seconds 
    Set max number of files to create according to the disk space you have e.g. 720 is the number of files generated for a 4 hour rotation for 30 days
10. Re-check the following day the amount of files generated and the file sizes are as expected.
11. Do not enable the service to auto start on boot. 
12. Do re-check the quantity of capture files generated and the disk space consumed by the capture is following the predicted trajectory. 
