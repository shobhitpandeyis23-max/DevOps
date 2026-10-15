# Exercise 5: Docker Security with AppArmor

## Objective
Apply a custom AppArmor security profile to a Docker container and verify its enforcement.

## Execution Steps
1. Built a simple Flask application image (`flask-apparmor`).
2. Loaded the custom profile into the kernel: `sudo apparmor_parser -r -W my-apparmor-profile`
3. Started the container with the security flag: `docker run -d --name flask-protected --security-opt apparmor=my-apparmor-profile -p 5000:5000 flask-apparmor`
4. Verified attachment using `docker inspect --format '{{json .HostConfig.SecurityOpt}}' flask-protected`

## Verification Result
The AppArmor profile successfully restricted container permissions. The container was denied read access to required Python shared libraries, resulting in a crash. This enforcement was verified via kernel audit logs (`dmesg`), showing `apparmor="DENIED"` events for `libpython3.9.so.1.0`.
