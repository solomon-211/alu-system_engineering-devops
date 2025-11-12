# Load Balancer
This project implements a redundant web server infrastructure with load balancing using HAProxy.

## Project Overview
The goal of this project is to enhance our web stack by introducing redundancy to our web servers. This allows us to:

- Accept more traffic by doubling the number of web servers
- Make our infrastructure more reliable
- Continue serving requests even if one web server fails

## Infrastructure
### Servers
- web-01: 100.26.206.13 - Primary web server
- web-02: 54.211.4.59 - Secondary web server
- lb-01: 54.236.248.230 - Load balancer

## Technologies Used
- Nginx: Web server
- HAProxy: Load balancer
- Ubuntu 20.04 LTS: Operating system

Tasks
## Task 0: Double the Number of Webservers
Configure web-02 to be identical to web-01 with a custom HTTP response header.

## Requirements:
Configure Nginx so that its HTTP response contains a custom header on both web-01 and web-02

### The custom HTTP header name is X-Served-By.
The value of the custom HTTP header is the hostname of the server on which Nginx is running

## File: 0-custom_http_response_header
Usage:

### Transfer script to server
scp -i ~/.ssh/my_web-key 0-custom_http_response_header ubuntu@<SERVER-IP>:~/

### SSH to server
ssh ubuntu@<SERVER-IP> -i ~/.ssh/my_web-key

### Run the script
sudo ./0-custom_http_response_header

Testing:
## Test web-01
curl -sI 100.26.206.13 | grep X-Served-By

## Output: X-Served-By: 6921-web-01

## Test web-02
curl -sI 54.211.4.59 | grep X-Served-By

## Output: X-Served-By: 6921-web-02

## Task 1: Installation of  Load Balancer
Install and configure HAProxy on lb-01 server to distribute traffic between web-01 and web-02.

## Requirements:
- Configure HAProxy to send traffic to web-01 and web-02
- Distribute requests using a round-robin algorithm
- Ensure HAProxy can be managed via an init script
- Servers must be configured with the correct hostnames

## File: 1-install_load_balancer
Usage:

### Transfer script to lb-01
scp -i ~/.ssh/my_web-key 1-install_load_balancer ubuntu@54.236.248.230:~/

### SSH to lb-01
ssh ubuntu@54.236.248.230 -i ~/.ssh/my_web-key

### Run the script
sudo ./1-install_load_balancer

### Check HAProxy status
sudo service haproxy status

Testing:
## Test load balancer - should alternate between servers
curl -sI 54.236.248.230 | grep x-served-by
curl -sI 54.236.248.230 | grep x-served-by
curl -sI 54.236.248.230 | grep x-served-by
curl -sI 54.236.248.230 | grep x-served-by

## Expected output (alternating):
-  x-served-by: 6921-web-01
-  x-served-by: 6921-web-02
-  x-served-by: 6921-web-01
-  x-served-by: 6921-web-02

## How It Works
Load Balancing Algorithm: Round-Robin
HAProxy uses the round-robin algorithm to distribute incoming requests:
First request → web-01
Second request → web-02
Third request → web-01
Fourth request → web-02
And so on...
This ensures even distribution of traffic across both servers.

## Custom Headers
The X-Served-By header allows us to:
Track which web server handled each request
Verify that load balancing is working correctly
Debug issues in production
Understand traffic distribution patterns

## Architecture Diagram
                   Internet
                       |
                       v
                  [lb-01: HAProxy]
                   (54.236.248.230)
                       |
            +----------+----------+
            |                     |
            v                     v
    [web-01: Nginx]       [web-02: Nginx]
    (100.26.206.13)       (54.211.4.59)

