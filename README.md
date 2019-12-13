### bag
Docker in 600 lines of bash using [https://proot-me.github.io/](proot).
Fully compatible with [https://termux.com/](termux), runs on android up to
version 10 with no problems.

This is a stateless script, it creates no files or anything to keep track of the instances you create. Doesn't run a daemon. It's plain bash, will `ls` into a directory to see what instances you have. Simple as that.

### Supported images
Currently only alpine. Will add arch, fedora, ubuntu, debian and ... later.

### Road Map
- Directly pull and run docker images.
- Support `chroot`.
- Test more on PC / x86.
- Make proot accept custom port mapping.

### Prerequisites
- [https://proot-me.github.io/](proot)
- bash 4.0+
- tar (d'oh! does this even count?)
- grep
- sha256sum (or disable image verification)
- curl


### Examples

| Example                                                    | Command
| ---------------------------------------------------------- | ------------------
| Run the command `whoami` in the *default* container        | `bag x whoami`
| Run the command `sleep 3` in the *default* container       | `bag x sleep 3`
| Run the command `whoami` in the container called foo, if the container doesn't exist, create it with default image first   | `bag r foo whoami`
| Create an container called *foo* with image **alpine**, don't run anything   | `bag c foo alpine`
| Create an container called *foo* with default image (alpine), don't run anything   | `bag c foo`
| Remove the container **foo**                               | `bag rm foo`
| Remove the default container                               | `bag rmd`
| Create tar archive from the container foo                  | `bag e foo`
| Copy a file/dir into the continaer foo                     | `bag cp /etc/host foo:/opt`
| Copy a file/dir from the continaer foo                     | `bag cp foo:/opt/config.cfg /tmp`
| Copy a file/dir from the continaer foo to container bar    | `bag cp foo:/etc bar:/usr/`


### Implemented commands

| (Docker) Command  | Args            | Alt Form         | Info                                 |
| -------- | ------------    | ---------------- | ------------------------------------ |
|  version |                 | -v               | Display version.
|  help    |                 | [-]h[elp]        | Display help and usage.
|  images  |                 | i[mages]         | List all previously downloaded images.
|  rmi     | rmi             |                  | Remove a previously downloaded image.
|  c       | c image         | c[reate]         | Create an container from the image id <image>.
|  run     | run <id> [cmd]  | r[un]            | Run a command in a container, can be existing container. If it doesn't exist a new one will be created from default image. If command is not given, it will open a login shell. But if the container has a **entry point** file and no command is given, the entry point file is ran instead of a login shell.
|  ps      |                 |                  | List all previously created containers.
|  rm      | rm id           |                  | Remove the container with the id <id>.
|  rmd     |                 | rm bagdefault    | Remove the default container.
|  cp      | cp id:/src /dst | cp src id:/dst   | Copy a file from host to an container or from an container to a host or from one container to another container. Recursive by default.
|  e       | e id            | e[xport]         | Export the container with the id <id> as a tar archive.
|  x       | x cmd           | execute-default  | execute the command <cmd> in the default container.

### To be implemented commands:
- exec
- import
- kill
- pull
- push
- rename

