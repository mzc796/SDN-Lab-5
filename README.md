# SDN-Lab-4
We use L2switch project as the basis to build customized SDN applications.

## Virtual Machine Summary
Memory: >= 8GB

Storage: 50GB

CPU: 2 cores, AMD64 Architecture

Installation Disc: [ubuntu-22.04.4-desktop-amd64.iso](https://old-releases.ubuntu.com/releases/22.04/)

## References
[An Instant Virtual Network on your Laptop (or other PC)](https://mininet.org/)

[PICOS 4.4.3 Configuration G](https://pica8-fs.atlassian.net/wiki/spaces/PicOS443sp/overview?homepageId=10453009)

[OpenDaylight Flow Examples](https://docs.opendaylight.org/projects/openflowplugin/en/latest/users/flow-examples.html)
## Preparation
0. If the system doesn't have enough disk space:
   
   (1) Shut down the VM. In the VMWare Fusion settings->Hardware Disk-> Enlarge the Disk allocation.
   
   (2) Power on the VM. Install growpart (if not installed):
   ```
   sudo apt update
   sudo apt install cloud-guest-utils
   ```
   (3) Expand partition 2
   ```
   sudo growpart /dev/nvme0n1 2
   ```
   Expected output:
   ```
   CHANGED: partition=2 ...
   ```
   (4) Resize ext4 filesystem
   ```
   sudo resize2fs /dev/nvme0n1p2
   ```
   (5) Verify
   ```
   df -h
   ```
   You should see something like this:
   ```
   /dev/nvme0n1p2   98G   18G   80G   19%   /
   ```
1. Install compatible Java:
   
   (1) Install Java 2.1:
   ```
   sudo apt install openjdk-21-jdk
   ```
   (2) List installed Java versions
   ```
   sudo update-alternatives --config java
   ```
   You should see something like:
   ```
   There are 2 choices for the alternative java (providing /usr/bin/java).
   Selection    Path                                         Priority   Status
   * 0            /usr/lib/jvm/java-21-openjdk-arm64/bin/java   2111      auto mode
     1            /usr/lib/jvm/java-17-openjdk-arm64/bin/java   1711      manual mode
     2            /usr/lib/jvm/java-21-openjdk-arm64/bin/java   2111      manual mode
   
   Press <enter> to keep the current choice[*], or type selection number: 
   ```
   (3) Configure Java Home for Java 2.1
   ```
   sudo vim /etc/profile
   ```
   Add the following to the end of /etc/profile
   ```
   JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
   PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
   export JAVA_HOME
   export JRE_HOME
   export PATH
   ```
   (3) Make JAVE_HOME valid and Test JAVA Version
   ```
   source /etc/profile
   java -version
   ```
2. Install compatible maven:
   ```
   cd /tmp
   MAVEN_VER=3.9.6

   curl -fL -o apache-maven-${MAVEN_VER}-bin.tar.gz \
   https://archive.apache.org/dist/maven/maven-3/${MAVEN_VER}/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz

   ls -lh apache-maven-${MAVEN_VER}-bin.tar.gz
   tar -xzf apache-maven-${MAVEN_VER}-bin.tar.gz
   sudo rm -rf /opt/maven
   sudo mv apache-maven-${MAVEN_VER} /opt/maven
   ```
   (2) Add Maven Path:
   ```
   echo 'export MAVEN_HOME=/opt/maven' >> ~/.bashrc
   echo 'export PATH=$MAVEN_HOME/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   hash -r
   mvn -version
   ```
   You should see Maven 3.9.6.
   
3. Download the code:
   ```
   git clone https://github.com/mzc796/SDN-Lab-4.git
   ```
4. Build L2switch project:
   ```
   cd SDN-Lab-4
   mvn clean install
   ```

