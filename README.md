# GrayLog
Docker configuration for GrayLog with OpenSearch. I'm using GrayLog 4.3, because the newest version of GrayLog is working with MongoDB v5, but it's not working on my NAS.
Compability version of GrayLog sidecar - v1.2.0-1

How to install:

1. In docker-compose.yml file in "volumes" section in MongoDB and GrayLog - specify your path. Also for GrayLog you need to amend second part of row from "date" to "data1"

2. In docker-compose.yml file in GrayLog section specify your GRAYLOG_PASSWORD_SECRET and GRAYLOG_ROOT_PASSWORD_SHA2. You can generate SHA2 by using script:
echo -n 'somepasswordpepper' | sha256sum | awk '{print $1}'

3. Run "docker-compose up -d"

4. Wait till GrayLog comes up (check via "docker ps") and login to it ("docker exec -it ContainerName bash")

5. Copy "/usr/share/graylog/data" into "/usr/share/graylog/data1"

6. Exit from container and run "docker-compose down"

7. In docker-compose.yml file in GrayLog section in "volumes" amend second part of row from "date1" to "data"

8. Open config file for Graylog ("config/graylog.conf") on your machine and replace "elasticsearch_hosts" to "http://opensearch:9200"

9. Delete files from MongoDB folder on your machine (it's not neccessary, but I did it to be sure that it won't broke anything)

10. Run "docker-compose up -d"



How to configure log collection on Windows:

1. Download and install Graylog Sidecar v1.2.0-1 (you can create API in GrayLog -> System -> Sidecars -> Click on "Create or reuse a token for the graylog-sidecar user" and create token)

2. You might need to create service via PowerShell or CMD. For some reason it wasn't created on mine.

3. Into GrayLog - create Inpit "Beats" with standard port. You just need specify only name and choose GrayLog server.

4. Into Sidecars -> Configuration amend "winlogbeat" in Log Collectors. You need to replace IP address from "192.168.1.1" to address of your GrayLog server.

5. Create new configuration. Specify name and choose Log Collector "winlogbeat". You might want to be more specific with events that you want to collect.
In this case you can use the following articles:
https://go2docs.graylog.org/5-0/getting_in_log_data/ingest_windows_eventlog.html

https://hochwald.net/ship-windows-event-logs-with-winlogbeat/

6. In Sidecars -> Administration assign this configuration on the machine.

