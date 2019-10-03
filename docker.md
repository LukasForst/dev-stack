# Docker
Useful tricks for docker engine.

## Installation
Install from here https://docs.docker.com/install/ or from current distro repo.

Ubuntu packages `docker-ce docker-ce-cli`

### Postinstall
Add current user to the `docker` group to allow use docker without `sudo` command.
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker
```

### Login to Docker hub
```bash
docker login -u <username> -p <password> <hub url>
```

### Basic images
```bash
docker pull driveone/onedrive
docker pull blang/latex:ubuntu
docker pull node:10-alpine
docker pull	jlesage/jdownloader-2
```


## Useful docker images related commands

### Javascript
```bash
# installs and builds javascript located ad /client folder in the node image 
# npm run build-prod is custom for CAPA project

docker run -it -v $(pwd)/client:/client -w /client node:10-alpine /bin/ash -c "npm install; npm run build-prod;" 
```
### JDownloader
Popular downloader running inside Docker. To access the program, go to http://localhost:5800 (if running on localhost).

```bash
isRunning=`docker inspect -f {{.State.Running}} jdownloader-2`
downloadDir="~/Downloads"

if [[ "$isRunning" == "false" ]]
then
	firstRun=false
	docker inspect jdownloader-2 > /dev/null || { firstRun=true; }
	if [[ "$firstRun" == "true" ]]
	then
		docker run -d \
    		--name=jdownloader-2 \
    		-p 5800:5800 \
    		-v /docker/appdata/jdownloader-2:/config:rw \
		-v "${downloadDir}:/output:rw"
    		jlesage/jdownloader-2
    else
    	docker start jdownloader-2
	fi
fi
xdg-open http://localhost:5800/ &
```

### OneDrive
Script for starting up and using the OneDrive cloud from the Docker.
It synchronizes files from the OneDrive with local folder.
```bash
# Update onedriveDir with correct existing OneDrive directory path
isRunning=`docker inspect -f {{.State.Running}} onedrive`
if [[ "$isRunning" == "true" ]]
then
	echo "Container is already running, exiting script."
	exit 0
fi
echo "Running script"

onedriveDir="~/OneDrive"

firstRun='-d'
docker inspect onedrive_conf > /dev/null || { docker volume create onedrive_conf; firstRun='-it'; }
docker inspect onedrive > /dev/null && docker rm -f onedrive
docker run $firstRun --cpus="0.15" --restart unless-stopped --name onedrive -v onedrive_conf:/onedrive/conf -v "${onedriveDir}:/onedrive/data" driveone/onedrive
```
