# Web Server
 This project primarily focuses on configuring and managing web servers, particularly Nginx, and automating server configuration tasks using Bash scripts.  

## Tasks
0. Transfer a file to your server

### File: 0-transfer_file
A Bash script that transfers a file from the client to the server using scp.

## Requirements:
- Accepts 4 parameters: PATH_TO_FILE, IP, USERNAME, PATH_TO_SSH_KEY
- Transfers the file to the user's home directory
- Disables strict host key checking

##Usage:
./0-transfer_file some_page.html 100.26.206.13 ubuntu ~/.ssh/my_web-key

1. Install the nginx web server

### File: 1-install_nginx_web_server
Bash script that installs and configures Nginx on Ubuntu.

## Requirements:
- Installs Nginx
- Nginx listens on port 80
- Returns a page containing "Holberton School" when queried at the root

### Steps to install Nginx:
- udo apt-get update -y; This ensures that all the package lists are updated and flags prompts with a -y flag as "yes".
- Sudo apt-get install nginx -y; This ensures the installation of Nginx after the packages are updated.
- Sudo service nginx status; This is a command for checking the status of Nginx
- Curl localhost; This command is used to ensure that Nginx is currently working, and it displays the default welcome to Nginx page by default, along with some content.

### Testing based on the project assignment:
curl localhost

# Should return: Holberton School for the win!

2. Set up a domain name
### File: 2-setup_a_domain_name
Contains your domain name (root domain only, no subdomain).

## Requirements:
Configure the DNS A record to point to the web-01 IP
Domain obtained through the GitHub Student Pack+.TECH Domains

3. Redirection
### File: 3-redirection
Bash script that configures Nginx to redirect /redirect_me to another page.

## Requirements:
Must be a 301 Moved Permanently redirect
Configures a new Ubuntu machine automatically

### Testing:
curl -sI your_server_ip/redirect_me/

# Should return: HTTP/1.1 301 Moved Permanently

4. Not found page 404
### File: 4-not_found_page_404
Bash script that configures Nginx with a custom 404 page.

## Requirements:
Returns HTTP 404 error code
Page contains the string "Ceci n'est pas une page"

## Testing:
curl your_server_ip/nonexistent_page

# Should return: Ceci n'est pas une page

5. Design a beautiful 404 page
### File: 5-design_a_beautiful_404_page.html
Creative custom 404 page that includes "Ceci n'est pas une page".

