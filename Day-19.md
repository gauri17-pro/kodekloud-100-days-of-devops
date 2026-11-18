# Day 19: Install and Configure Web Application

## 1. Install httpd
`sudo yum install httpd -y`

## 2. Apache should serve on port 5002
`sudo vi /etc/httpd/conf/httpd/conf`
`Listen 5002`

## 3. Move the folders of news and apps from jump host to app server 2
`scp -r /home/thor/news steve@stapp02:/tmp`
`scp -r /home/thor/apps steve@stapp02:/tmp`

## 4. Move the folder from tmp to /var/www/html
`sudo mv /tmp/news /var/www/html/`
`sudo mv /tmp/apps /var/www/html/`

## 5. Access the webpages using
`curl http://localhost:5002/news/`
`curl http://localhost:5002/apps/`

