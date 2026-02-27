# SDN-Lab-5
Learn L2switch project. Later, we will use L2switch project as the basis to build customized SDN applications.

## Virtual Machine Summary
Memory: >= 8GB

Storage: 50GB

CPU: 2 cores, AMD64 Architecture

Installation Disc: [ubuntu-22.04.4-desktop-amd64.iso](https://old-releases.ubuntu.com/releases/22.04/)

## References
[An Instant Virtual Network on your Laptop (or other PC)](https://mininet.org/)

[PICOS 4.4.3 Configuration G](https://pica8-fs.atlassian.net/wiki/spaces/PicOS443sp/overview?homepageId=10453009)

[OpenDaylight Flow Examples](https://docs.opendaylight.org/projects/openflowplugin/en/latest/users/flow-examples.html)

[L2switch User Guide](https://test-odl-docs.readthedocs.io/en/latest/user-guide/l2switch-user-guide.html)
## Preparation
0. If you encounter an error indicating insufficient disk space, please follow the instructions in Step 0 below. Otherwise, proceed directly to Step 1.
   
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
   
   (1) Install Java 21:
   ```
   sudo apt install openjdk-21-jdk
   ```
   (2) List installed Java versions
   ```
   sudo update-alternatives --config java
   ```
   You should see something like:
   ```
   There are 2 choices for the alternative Java (providing /usr/bin/java).
   Selection    Path                                         Priority   Status
   * 0            /usr/lib/jvm/java-21-openjdk-arm64/bin/java   2111      auto mode
     1            /usr/lib/jvm/java-17-openjdk-arm64/bin/java   1711      manual mode
     2            /usr/lib/jvm/java-21-openjdk-arm64/bin/java   2111      manual mode
   
   Press <enter> to keep the current choice[*], or type selection number: 
   ```
   (3) Configure Java Home for Java 21
   ```
   sudo vim /etc/profile
   ```
   Point Java Home to Java 21, instead of 17
   ```
   JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
   #JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
   PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
   export JAVA_HOME
   export JRE_HOME
   export PATH
   ```
   (4) Make JAVE_HOME valid and Test JAVA Version
   ```
   source /etc/profile
   java -version
   ```
2. Install compatible Maven:
   
   (1) Install maven 3.9.6
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
   git clone https://github.com/mzc796/SDN-Lab-5.git
   ```
4. Build L2switch project:
   ```
   cd SDN-Lab-5/
   mvn clean install -DskipTests -Dcheckstyle.skip
   ```
5. Run OpenDaylight-L2switch
   ```
   cd distribution/karaf/target/assembly/bin/
   sudo ./karaf
   ```
6. Run Mininet. Open a new terminal:
   ```
   sudo mn --topo tree,depth=3,fanout=2 --switch ovsk,protocols=OpenFlow13 --controller remote,ip=127.0.0.1,port=6653
   ```
   ```
   s1
   ├── s2
   │   ├── s3
   │   │   ├── h1
   │   │   └── h2
   │   └── s4
   │       ├── h3
   │       └── h4
   └── s5
       ├── s6
       │   ├── h5
       │   └── h6
       └── s7
           ├── h7
           └── h8
   ```
7. Check default configuration.

   (1) Observe default flow entries. For example:
   ```
   cd SDN-Lab-5/mn/
   sudo ./dump_flows.sh s3
   ```
   (2) Observe topology.
   ```
   cd SDN-Lab-5/odl-scripts/
   sudo ./req_topo.sh
   ```
   Note: Remember what you observed.
   
8. `ping` different hosts and observer flow entries. 

   (1) In the mininet terminal:
   ```
   h1 ping h2
   ```
   In the system terminal:
   ```
   sudo ./dump_flows s3
   ```
   (2) In the mininet terminal:
   ```
   h1 ping h3
   ```
   In the system terminal:
   ```
   sudo ./dump_flows s3
   ```
   Question: Observe cases (1) and (2), and reason about the difference.
   
9. Request the topology after ping. In a system terminal:
   ```
   cd SDN-Lab-5/odl-scripts/
   sudo ./req_topo.sh
   ```
   Read the `topo.json` file. 
   
   Question: What is the change compared to the topology observed at Step 7 (2)?
   
10. Change the source code to make the link ID of the links between the host and the switch start with "hello-".
    For example, "link-id": "hello-openflow:3:2/host:ea:09:0d:5c:77:31" instead of "openflow:3:2/host:ea:09:0d:5c:77:31".
