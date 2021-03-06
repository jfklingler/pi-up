Pi UP
=====

Pi UP is a Raspberry Pi startup notification broadcaster and listener designed
to solve a simple problem of discovering the IP address of a fresh booted
Raspberry Pi running without a monitor. This is accomplished by broadcasting a
UDP packet on port 31415 (get it?) from the Raspberry Pi, while the client
listens for the broadcast on another machine. Both the UDP broadcast server and
the listener client are contained within a single, simple file. Efforts were
also made to ensure a minimim of library dependencies to preserve space on the
Pi's SD card and also to simplify installation.

## Installation and Setup

Setup is easy but does need to be performed on both the Raspberry Pi and
another client machine that will listen for broadcasts. The first step is to
get the code on both machines. You can do this in a couple of ways:

1. Clone the repo
  * `git clone http://github.com/jfklingler/pi-up.git`
1. Grab the archive and extract it
  * `wget -O pi-up.zip http://github.com/jfklingler/pi-up/archive/master.zip`

You may need to make `pi-up` executable
```
chmod +x pi-up
```

### Raspberry Pi
Pi UP does need to be set up initially with a monitor connected of course. On
the Raspberry Pi, add the following to /etc/rc.local:

```
/path/pi-up broadcast
```

If you're DHCP lease assignment and interface startup is a little too slow,
you might not have a IP assigned by the time rc.local runs. I've found this
modification to rc.local, while not particularly genius, get's the job done:

```
_IP=$(hostname -I) || true
while [ ! "$_IP" ]; do
  sleep 1
  _IP=$(hostname -I) || true
done
```

### Client Machine
Run this on another machine on the same network:

```
pi-up listen
```
This will wait for a broadcast from the Raspberry Pi and print out the details
of all network interfaces on the Pi. Now just power on the Pi and wait.

## Errata
You can get help on the tool's relatively few options at the command line via
`pi-up --help`.

If you find any problems and would like to see any new features or
improvements, please create an issue. Or, if you're so inclined, fork it, fix
it, and send me a pull request.
