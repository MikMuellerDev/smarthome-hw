# Smarthome-hw
 Hardware interface for the Smarthome server
 
 # This repository has been transferred to the [Smarthome](https://github.com/smarthome-go/node) organization.
 
 ### What does it do?
 The Smarthome server acts like a global hub to connect services, people and hardware together.
 Because the hub is a centralized server which should be able to run on any hardware, not just the Raspberry-Pi, a master-slave setup is used to control the power outlets accessible to smarthome users. In this setup, the Smarthome-hub acts as the master which is able to orchistrate the slaves, in this case the hardware-nodes.
 
 The 433mhz sockets are controlled by a physical sender which is attached to each Raspberry-Pi, making it a hardware-node.
 The nodes can be registered in the smarthome server which is then able to manage the nodes, send power commands to them and monitor their health.
 
 *smarthome-hw* therefore provides a *REST-API* for making its hardware accessible to the Smarthome-hub.
 
 For redundancy and increased reliability, a smarthome network should contain more than just one hardware-node.
 Redundancy, fallback and latency are handled by the Smarthome-hub which automatically detects if nodes fail.
 
Because a single node is not able to serve concurrent requests due to the limitations of the attached hardware, a locking system is implemented in the nodes.
This lock is acquired when a hardware request ist made and released if the request finishes or is terminated abnormally.

In order to provide a true concurrent, non-blocking access to the power outlets, the smarthome hub contains an internal queueing system which manages the order concurrent requests are executed in a blocking manner (one after the other).

![smarthome-hw logo](./icon/readme.png)

### Token 🔑
In order to guarantee a safe communication between the `smarthome` server and the `smarthome-hw` server, a token is required.
When this application is first started, a *random* token will be generated and printed to the server's logs (**but not to file**).
#### Change token ↺
In order to change the default token, use the provided bash script:
```bash
./update_token.sh "old_token" "new_token"
```

## Api

To send a power request, make a **POST** request to a similar url with a similar request body encoded as `application/json`
```
http://localhost:8081/power?token=smarthome
```

```json
{
	"switch": "s1",
	"power": true
}
```
