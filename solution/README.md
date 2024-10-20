1.By using the below commands pulled the two images
"docker pull infracloudio/csvserver:latest"
"docker pull prom/prometheus:v2.45.2"

2.Then run the container with the image but container was not running.
"docker run -d --name csvserver infracloudio/csvserver:latest"
"docker ps"
"docker ps -a"

3.Then checked the logs of container, so then came to know that the path not avialable.
"docker logs csvserver"

4.Then created a inpuFile by using bash "gencsv.sh" with 7 entries.
"#!/bin/bash
start=$1
end=$2
> inputFile
for ((i=start; i<=end; i++)); do
    echo "$i, $(( RANDOM % 100 ))" >> inputFile
done"

"chmod +x gencsv.sh"
"./gencsv.sh 2 8"

5.Stopped and Deleted the container and again started the container with the input as inputFile in the container and login to the container and know the port number.
"docker stop csvserver"
"docker rm csvserver"
"docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata infracloudio/csvserver:latest"
"docker exec -it csvserver sh"
"netstat -tuln"

6.Now stop and delete the container again and Start the container again with the inputFile and port mapping.
"docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata -e CSVSERVER_BORDER=Orange -p 9393:9300 infracloudio/csvserver:latest"

7.Accessed the application successfuly
"http://localhost:9393"
