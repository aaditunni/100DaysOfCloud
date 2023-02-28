# Docker Fundamentals

## Cloud Research

Today, I started learning Docker Fundamentals by Adrian Cantrill. While doing the Free AWS Cloud Project Bootcamp by Andrew Brown, I realized that I needed to know and understand more about Docker and checked out the suggested course.

I learned some basics about Docker and it's Architecture. 
- Docker host or container host is the physical hardware. Containers un on them.
- On top of this is th host OS.
- On top of this is a container engine called Docker Engine.
- By comparison, it is lighter than a hypervisor because it has less to do.
- Any containers running on the Docker host shares the same OS.
- Basically, each container runs as a process in that OS which means that containers don't need their own OS.
- They only need to run the libraries and runtime environment and the application itself and because of this containers are much smaller an lightweight compared to the same application run on an virtual machine.
- Containers can infect other containers as they are not strongly isolated when they use resources aggressively.

I also installed Docker Desktop which installs dockerd. dockerd is the server side component that manages containers as well as command line and graphical client that can be used to interact with Docker.

## Social Proof

[Blog](https://dev.to/aaditunni/docker-fundamentals-3174)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7036471985572356097-BrIN?utm_source=share&utm_medium=member_desktop)
