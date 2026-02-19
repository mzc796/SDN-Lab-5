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
1. Install maven:
   ```
   sudo apt install maven
   ```
2. Install Java 2.1:
   ```
   sudo apt install openjdk-21-jdk
   ```
   List installed Java versions
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
   
2. Download the code:
   ```
   git clone https://github.com/mzc796/SDN-Lab-4.git
   ```
3. Build L2switch project:
   ```
   cd SDN-Lab-4
   mvn clean install
   ```

