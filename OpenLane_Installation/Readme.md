
# <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/51b57fea-c69a-46d8-9dfa-3dbbf04756df" /> OpenLane_Installation Steps

Before Installation steps, Lets start with what is Docker and Openlane.


# About **Docker and OpenLane**: 

OpenLane is an open-source automated RTL-to-GDSII flow for VLSI design, and it is distributed through a Docker-based environment to ensure portability and reproducibility. By using Docker, users avoid dependency issues, as the entire toolchain (Yosys, OpenROAD, Magic, Netgen, etc.) is packaged within a container. This makes the setup process much simpler and guarantees consistency across different systems (Ubuntu, Windows WSL, macOS). Running OpenLane inside Docker ensures that designs can be built, tested, and verified in a controlled environment, making it easier for researchers, students, and engineers to contribute to and replicate results from the open-source silicon ecosystem.


# Steps to install Docker and OpenLane:
 

# Step-1: Setup the Docker:

```bash

** Set Up Docker Repository **

sudo apt-get update

sudo apt-get install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

```


# step-1.1 :Adding Docker's repository

```bash

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

```

# step-1.2: Installing Docker

```

sudo apt-get install docker-ce docker-ce-cli containerd.io

```

# step-1.3: Verification

```

sudo docker run hello-world

```




**Add our User to the Docker Group**

```

# Create the docker group (if it doesn't exist)

sudo groupadd docker

# Add our user to the docker group

sudo usermod -aG docker $USER

# Log out and back in for group changes to take effect

```

# Step-2: Installing the OpenLane

# step-2.1: Checking tool version for installing openlane

```
git --version
docker --version
python3 --version
python3 -m pip --version
make --version
python3 -m venv -h

```

# step-2.2: Update the packages

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip python3-tk curl make git


```


# step-2.3: Building the Openlane tool
```

git clone https://github.com/The-OpenROAD-Project/OpenLane 
cd OpenLane 
make 
make test

```






