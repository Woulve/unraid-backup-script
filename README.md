# Unraid Backup Script

A shell script for backing up data from a local source directory to a remote server using `rsync`. The script also provides status notifications via a webhook (e.g., Discord) and features log file management with rotation to maintain clean log files.

## Features
- Uses `rsync` to perform backups, ensuring efficient data transfer.
- Supports SSH connections to remote servers with a configurable port.
- Sends start, success, and error notifications to a webhook URL (such as a Discord webhook).
- Log file management with automatic rotation when log files exceed a specified size.
- Configurable verbosity for different levels of log details.

## Use Case
This script is ideal for scheduled backups of a local directory (e.g., from an Unraid server) to a remote server. It is designed to work in conjunction with automation tools like `cron` to periodically sync important data to a secure location. The webhook notification provides real-time feedback on the backup status, making it useful for monitoring automated backup processes.

## Usage
Run the script with the following command and the necessary arguments:

```bash
./unraid-backup-script.sh -w <WEBHOOK_URL> -r <REMOTE_SERVER> -s <BACKUP_SOURCE> -d <BACKUP_DESTINATION> -n <BACKUP_NAME> -p <SSH_PORT> -l <LOG_FILE> -v <VERBOSITY_LEVEL>
```

### Arguments
- `-w  WEBHOOK_URL`         The webhook URL for notifications.
- `-r  REMOTE_SERVER`       The remote server address (e.g., `user@remote-server.com`).
- `-s  BACKUP_SOURCE`       The source directory for the backup (local path).
- `-d  BACKUP_DESTINATION`  The destination directory on the remote server.
- `-n  BACKUP_NAME`         A name for the backup process (used in notifications).
- `-p  SSH_PORT`            SSH port to use (default: `22`).
- `-l  LOG_FILE`            Path to the log file (default: `./logs/backup.log`).
- `-v  VERBOSITY_LEVEL`     Verbosity level (0 = minimal, 1 = normal, 2 = verbose). Default is `0`.
- `-h`                      Display the help message.

### Example
```bash
./unraid-backup-script.sh -w "https://discord.com/api/webhooks/..." -r "user@remote-server.com" -s "/mnt/user/backups/" -d "/home/backups/" -n "Unraid Backup" -p 23 -v 2
```

## Log File Rotation
The script automatically rotates log files when they exceed the size of 100 MB (default). It keeps up to 5 rotated log files. The default log path is `./logs/backup.log`, but you can customize this with the `-l` option.

## Requirements
- `jq` - for processing JSON payloads.
- `curl` - for sending webhook notifications.
- `rsync` - for performing the backup operations.

Ensure these commands are installed and available in your environment.

## Caveats and Notes
1. **Destructive Operations:** The script uses `rsync` with the `--delete` flag, which deletes files in the destination that are not present in the source. Be cautious when using this script to avoid unintended data loss.
2. **SSH Key:** Ensure that passwordless SSH is set up between the source and remote server for smooth operation.
3. **Webhook URL:** The script will send data to the specified webhook URL. Ensure the URL is valid and that it accepts JSON payloads.
4. **File Permissions:** Make sure the script has execute permissions:
   ```bash
   chmod +x unraid-backup-script.sh
   ```
5. **SSH Environment:** The script assumes SSH access to the remote server with the necessary permissions to read/write the backup directory.
6. **Testing:** Test the script with a small dataset before deploying it for full backups to ensure it works as expected.

## License
This script is open-source and free to use. Modify and adapt it to suit your needs.

## Contributions
Feel free to fork this repository and make improvements. Pull requests are welcome!

## Contact
For issues or suggestions, please open an issue on this repository.
