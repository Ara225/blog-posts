# Overview
I've built a simple PXE (network boot) server with node.js. I suspect it may be the first JavaScript PXE server ever - probably because JS isn't really seen as suitable for this stuff (whether justified or not). I'm still working on making it nicer but the core functionality is working great.

PXE is a protocol for booting clients PCs over a network. It's basically a DHCP and a TFTP server smashed together. Clients get a IP from the DHCP server, the DHCP server points to the TFTP server and the client downloads the boot files from it. My implementation is built on the preexisting dhcp and tftp npm modules, both of which are completeish implementations of their protocols and work great.

# Motivation
I wanted to make something a bit different and a little bit harder than my usual fare of light web dev and AWS Lambda functions. It's not particularly glamorous but it's been an interesting project, and I've got a much better understanding of DHCP, PXE, and TFTP. I'm also hoping to make this a component of a bigger project I'm working on.

I think it's definitely worth trying weird stuff like this - it gets you out of your comfort zone and you pick up new stuff. 

# Project

https://github.com/Ara225/node-js-pxe-server