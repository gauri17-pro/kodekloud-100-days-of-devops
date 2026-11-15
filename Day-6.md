## Adding a Cron

1. Install cronie
```sudo yum install cronie```

2. Start crond
```systemctl start crond```

4. Setup a cron
```*/5 * * * * echo hello > /tmp/cron_text```
