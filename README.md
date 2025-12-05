# jtac-uploader
A Small Script to assist engineers with Uploading RSI (Request support Information) and Log files from a remote device to a JTAC Case.
This is useful in cases with remote devices that you do not have direct FTP or SCP access to but the device does have internet access.
You may have SSH access via a jump host, Mist Cloud (Outbound-SSH) or direct, but do not have inbound SFTP enabled to collect the files from the device.

This script is designed as a single command to perform the folloing actions
1. Collect the RSI and store it in a local file
2. Package the /var/log/ directory into a file ready for upload
3. Provide instructions for manually uploading file to JTAC Case
4. <i>Using SFTP to upload the files to the Provided Case folder at JTAC (Work in Progress)</i>

## Execution
To run the script, on any Internet connected Juniper Device...
Execute the following command on the Operational mode prompt (>):

`op url https://raw.githubusercontent.com/thewhitehouse007/jtac-uploader/main/jtac-upload.slax`

### Example Console Output:
```
Enter JTAC Case Number (format: nnnn-nnnn-nnnnnn), or press Enter for none:1111-2222-333444

Collecting RSI data, please wait ~10-30mins ...
RSI collected: /var/tmp/RSI_ROUTER1_2025-10-14_1107.log
Archiving system logs...
Logs archived: /var/tmp/LOGS_ROUTER1_2025-10-14_1107.tgz
FTP not automated yet... Enter the following commands into JunOS Cli...
Please copy and past one line at a time, pasting all line will cause commands to execute/wait in your current shell.
---NOTE: jtac SFTP Password is 'anonymous'---
---------------------------------------------
sftp jtac@sftp.juniper.net
mkdir pub/incoming/1111-2222-333444
cd pub/incoming/1111-2222-333444
put /var/tmp/RSI_ROUTER1_2025-10-14_1107.log
put /var/tmp/LOGS_ROUTER1_2025-10-14_1107.tgz
bye
---------------------------------------------
Keeping local copies of RSI & Logs.
```

## NOTES
Please note: the jtac SFTP password is `anonymous`

## Contribution/Developer Call
Current issues: With SFTP using Shell Commands over SLAX, the password cannot be provided for the SFTP interactive command, also the `-d` script option requires the use of pass keys, 
The option to run this as python code using the netmmiko module was also considered, however JunOS has restrictions on execution of unsigned python scripts via the shell. 
If you have some ideas on how we can solve this, please reach out and I will add you as a contributer. NOTE: Options to use a third party tool to export files to and perform the scripts off box will not be highly considered as the user must maintain control of their data.
