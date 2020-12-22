# This is a POC of MVS

Required steps:

1 - Create subnet: sudo docker network create --subnet 10.0.1.0/24 local_network_dev
2 - Run docker-compose up

This will launch 2 services:

1 - gstreamer: Serves rtsp stream <br/>
2 - webrtc-server: Has the active Janus instance
