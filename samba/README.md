# Use Samba in a Podman container

Uses dperson/samba: https://github.com/dperson/samba

```bash
# Temporarily allow binding below 1024 
sudo sysctl net.ipv4.ip_unprivileged_port_start=0

# Starting samba with current user (write will probably not work)
podman run -it --name samba -p 139:139 -p 445:445 -v ~/Desktop/:/mount docker.io/dperson/samba -u "$(id -un);Test1234;$(id -u);$(id -gn);$(id -g)" -s 'Desktop;/mount/;yes;no;no'

# User ID of current user should be kept inside container
podman run -it --userns=keep-id --name samba -p 139:139 -p 445:445 -v ~/Desktop/:/mount docker.io/dperson/samba -u "$(id -un);SomePassword;$(id -u);$(id -gn);$(id -g)" -s 'Desktop;/mount/;yes;no;no'

# Automatically setting permissions (use with caution)
podman run -it --name samba -p 139:139 -p 445:445 -v ~/Desktop/:/mount docker.io/dperson/samba -p -u "$(id -un);SomePassword;$(id -u);$(id -gn);$(id -g)" -s 'Desktop;/mount/;yes;no;no'

# Start container again
podman start samba

# List containers
podman container ls

# Get outside world PID
podman container inspect samba|jq '.[].State.Pid'

# Get logs
podman logs samba

# Enter container
podman exec -ti samba /bin/bash

# Useful inside container to find recent files
find $(ls -d /*|grep -v '/sys\|/proc\|/dev') -mmin -30

# Stop container
podman stop samba

# Delete container
podman rm samba

# Reset binding ports
sudo sysctl net.ipv4.ip_unprivileged_port_start=1024
```
