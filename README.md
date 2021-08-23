1. Install Splunk Enterprise by using an installation package, on Windows:
  * First check the system requirements :http://docs.splunk.com/Documentation/Splunk/8.2.1/Installation/SystemRequirements
  * Download the installation package: http://www.splunk.com/download
  * Perform the installation by using the instructions: https://docs.splunk.com/Documentation/Splunk/8.2.1/Installation/InstallonWindows
  
2. Install Splunk Enterprise on Linux:
  You can install Splunk on Linux using RPM or DEB packages or a tar file, depending on the version of Linux your host runs.
  Before installing, please make sure an firewall rule for port 8000 is added to the Linux firewall and ssh is enabled.
  * To install Splunk on an Debian machine, from the CLI you need to download first the .deb package by running the command ( :
         $  wget -O splunk-8.2.1-ddff1c41e5cf-linux-2.6-amd64.deb 'https://d7wz6hmoaavd0.cloudfront.net/products/splunk/releases/8.2.1/linux/splunk-8.2.1-ddff1c41e5cf-linux-2.6-amd64.deb'
  * If the package is not located in /opt, you need to move it to that location because /opt/splunk is the default desired location for Splunk Enterprise.
  * Next run the dpkg installer:
        $   dpkg -i splunk_package_name.deb
  * Navigate to the bin folder and start Splunk by running the command:
         $  ./splunk start --accept-license
  After Splunk starts, you should be able to access it on the provided URL from a local browser.
  
3. Install Splunk Enterprise inside a docker container:
  The official repository containing docker files for building Splunk can be found here: https://github.com/Splunk/docker-Splunk
   * In order to install Splunk inside Docker, please install docker first:
        $ apt install docker.io
   * next, enable docker:
        $ systemctl enable --now docker 
        $ systemctl disable --now docker // command to disable docker
   * Check to see if you have latest version orf docker installed:
        $ docker --version
   * Start an image to test docker functionality:
        $ docker run hello-world
   * Install Splunk docker container: 
        $ docker pull splunk/splunk:latest
   * Start the docker image, by default on port 8000 and with 'admin' user ( you can change the container name by replacing the entry form the '' after the --name argument): 
        $ docker run -d -p 8000:8000 -e SPLUNK_START_ARGS='--accept-license' -e SPLUNK_PASSWORD='<login_password>' --name <container_name> splunk/splunk:latest
        The output of the docker run command is a hash of numbers and letters that represents the container ID of your new Splunk Enterprise instance. Run the following command with the container ID to display the status of the container.
        $ docker ps -a -f id=<container_id>
   * In order to create a new Splunk docker container, but on another port ( 9000 for example ) you only need to change the first port number after the '-p' argument and the name of the container:"
        $ docker run -d -p 9000:8000 -e SPLUNK_START_ARGS='--accept-license' -e SPLUNK_PASSWORD='<login_password>' --name <new_container_name> splunk/splunk:latest
   * Other usefull docker commands:
        $ docker ps - list of running containers
        $ docker ps -a show all containers/even the ones offline
        $ docker stop <container_name>  -stops container
        $ docker start <container_name> - starts container
        $ docker rm <container_name>  - removes container ( an container must first be stopped and only after that it can be removed )
        
4. Import data into Splunk:
      Settings > Add Data and select what type of data you want to import to Splunk: either Cloud data, Networking data, OS Data or upload data from you local computer which you want Splunk to analyze, or get the data from a Forwarder which you have set up.
      
5. Parsing logs:
      In order to get the most of the data imported into Splunk, you need to correctly set the source type associated with it which allows Splunk to correctly parse the logs and interpret the data. Splunk has built in some parses but, for the logs which are not parsed by Splunk, you can search and install various AddOns from Splunk marketplace which will correctly parse the data.

6. Search in Splunk  :

Full list of search commands which can be used to filter the data, can be found here: https://docs.splunk.com/Documentation/SplunkLight/7.3.6/References/Listofsearchcommands

Search modes: fast mode (field discovery is disabled for better performance); Verbose Mode ( returns all of the field and event data possible, even if the search takes more time to complete) and Smart Mode (toggles behavior based on search type ) 

Restricting or filtering the data is the easiest and most effective way to optimize the search output.
the Splunk SPL ( SPL - search processing language) supports the boolean operators AND, OR and NOT:

example:
to search for all failed login attempts for user root: root AND failed
to search for failed logins for all users except admin: failed not admin
to search failed logins for user admin OR root: failed admin OR root

When performing a search, the AND operator is always implied between multiple terms ( 'web error' has the same meaning as 'web AND error')

The Splunk search assistant has three modes: Full, Compact and None. By default the compact mode is selected but you can switch to the other modes from Account Settings.

