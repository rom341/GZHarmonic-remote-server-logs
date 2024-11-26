# Goal:
The goal is to run the GZ Harmonic server (`gz sim -s`) on AWS, and on clients connected to the server via a local OpenVPN network (Tap mode), open the interface using `gz sim -g`.

# Problem:
When connecting, clients see a blank window.  
![Screenshot from 2024-11-26 16-53-09](https://github.com/user-attachments/assets/36bd1185-3200-4219-98b0-6cb0c532a0a0)

# Setup:
Server: AWS EC2 Ubuntu 24.04 + OpenVPN in TAP mode + GZ Harmonic v8.7.0 from binary install.

Clients: Ubuntu 24.04 + GZ Harmonic v8.7.0 from binary install.

# Steps to reproduce:
On the server, I run the following commands:
```bash
export GZ_IP=[server_ip_in_lan]
export GZ_PARTITION=1
gz sim -s -v4
```

On the client, I run the following commands:
```bash
export GZ_IP=[client_ip_in_lan]
export GZ_PARTITION=1
gz sim -g -v4
```

# Additional Information:
1) If these steps are repeated when the server and client are on the same Wi-Fi network, everything works correctly; the client can see and interact with the world.
2) The client receives UDP multicast from the server, and the server receives it from the client (verified with Wireshark).
3) If the client uses `export GZ_RELAY=[server_ip_in_lan]`, nothing changes, and the screen remains blank.
4) If the server and client are run on the same computer with `GZ_IP=127.0.0.1` for both, everything works fine. However, if a delay is added using the command:
```bash
sudo tc qdisc add dev lo root netem delay 17ms
```
and the delay is set to 17 milliseconds or more, the client will show the same blank screen as seen in the image above.

5) If an object is added, it will be created on the server and visible if `gz sim -g` is run on the server, but client window will not change.
6) If a world containing a camera is launched, the client will still be able to view the camera feed correctly, but the world itself will not be displayed.  

for example camera_sensor.sdf
![Screenshot from 2024-11-26 17-00-59](https://github.com/user-attachments/assets/e54b1998-4d8b-4211-b200-096439250815)
