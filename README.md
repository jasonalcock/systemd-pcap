# systemd-pcap
Safely run long packet captures

Steps:
1. Run df -h to get amount of free disk space on system
2. Insert file /etc/systemd/system/packet-capture.environment
3. Insert file /etc/systemd/system/pcap-tool@.service
4. chmod 644 both files
5. Edit pcap-tool@.service and ensure BPF filter is specific as possible to traffic you want to capture 
5. Start capture using systemctl start pcap-tool@<interface name> and leave to run for 20 minutes
6. Work total disk space needed for the long running capture.
    6.1. run ls on the /var/spool directory to see the size of the rotated gz files generated 
    6.2. total disk space formula 
        total run time needed in minutes (e.g. 30 days is 43200 ) * (compressed gz file capture size in MB after 20 min run / 20)
8. Stop capture systemctl stop pcap-tool@<interface name>
9. Edit pcap-tool@.service and set:
    safety max FILESIZE for pre zipped rotated files e.g. 4 hour files should generate no more than 150 MB files
    Rotation in seconds e.g. 4 hours = 14400 seconds 
    Set max number of files to create according to the disk space you have e.g. 720 is the number of files generated for a 4 hour rotation for 30 days
10. Re-check the following day the amount of files generated and the file sizes are as expected.
11. Do not enable the service otherwise if a system is rebooted the packet captures could run for mnore than 30 days and fill disk.
12. Re-check the capture again after 4 days and ensure the disk usage is on the correct tarjectory. 
