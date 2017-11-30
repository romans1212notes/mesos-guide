# Install with Docker
* https://github.com/dcos/dcos-docker
```
  $ git clone https://github.com/dcos/dcos-docker
  $ cd dcos-docker

  $ ./configure --auto
  $ make
  
  Creating directories under /etc/mesosphere
  Creating role file for slave_public
  Configuring DC/OS
  Setting and starting DC/OS
  Created symlink from /etc/systemd/system/multi-user.target.wants/dcos-setup.service to /etc/systemd/system/dcos-setup.service.
  DC/OS node setup in progress. Run 'make postflight' to block until they are ready.
  Master IP: 172.17.0.2
  Agent IP:  172.17.0.3
  Public Agent IP:  172.17.0.4
  Web UI: http://172.17.0.2
```
* Can use nginx to export for external access
