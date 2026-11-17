# Linux Process Troubleshooting

## Check if any other service is running on port 5004.
`ss -tulnp`

## If yes then check which service is running
`sudo fuser -v 5004/tcp`

## Stop that service
`sudo systemctl stop sendmail`

## Restart the tomcat service
`sudo systemctl restart httpd`
