# systemd-pcap
Safely run long packet captures

Steps:
1. Run df -h to get amount of free disk space on system
2. Insert file /etc/systemd/system/packet-capture.environment
3. Insert file /etc/systemd/system/pcap-tool@.service
4. chmod 644 both files
5. Edit pcap-tool@.service and ensure BPF filter is specific as possible to traffic you want to capture 
5. Start capture using systemctl start pcap-tool@<interface name>
6. Work out disk space needed to capture traffic for X days:
    run ls on the /var/spool directory to see the size of the rotated gz files generated 
    total run time needed in minutes (e.g. 30 days * 24 * 60) * (file size in MB after 20 min run / 20)
8. Stop capture systemctl stop pcap-tool@<interface name>
9. Edit pcap-tool@.service and set:
    safety max FILESIZE for pre zipped rotated files e.g. 4 hour files should generate no more than 150 MB files
    Rotation in seconds e.g. 4 hours = 14400 seconds 
    Max number of files to create e.g. If files are rotated every 4 hours 30 day setting is 720
10. Re-check the following day the amount of files generated and the file sizes are as expected.
11. Do not enable the service otherwise if a system is rebooted the packet captures could run for mnore than 30 days and fill disk.
