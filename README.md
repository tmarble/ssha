# ssha

This tiny utility program saves information about the current
ssh-agent so it's easy to connect from multiple different
terminal sessions (even if they do not have a common parent
with the critical environment varibles set).

## LICENSE

This software is licensed under the permissive MIT license.

This license has been approved by
 * OSI: http://www.opensource.org/licenses/MIT
 * FSF: https://www.gnu.org/licenses/license-list.html#X11License (GPL compatible)

## Requirements

On Debian systems the following packages are required:

    openssh-client

## Usage

One time configuration (e.g. in your ~/.bashrc):
```
SSHA_HOME=$HOME/src/github/tmarble/ssha
export ssha=$SSHA_HOME/ssha
alias ssha=$SSHA_HOME/ssha
. $SSHA_HOME/ssha.funcs
```

First terminal:

    $ ssha_start # start the ssh-agent
	$ ssh-add    # add your private key material
	$ ssh host-which-has-my-key

Second and subsequent terminal:

    $ ssha_connect # connect to the running ssh-agent
	$ ssh host-which-has-my-key

## Scripting

You can call ssha from other scripts (see also [sshatest](https://raw.github.com/tmarble/ssha/master/sshatest))

    ssha_functions=true
    . /path/to/ssha
    if do_ssha_passwordless; then
      echo "connected to the ssh-agent with cached credentials"
    else
      echo "error: ssh key could not be added to ssh-agent (is it passwordless?)."
    fi

## NOTES

A very typical use case is re-connecting to a long running session

```
tmarble@laptop $ ssh remoteserver
tmarble@remoteserver $ ssh_ready
ssh-agent ready
tmarble@remoteserver $ ssh thirdserver uptime
 17:59:57 up 11:07,  8 users,  load average: 0.04, 0.10, 0.13
tmarble@remoteserver $
```

Another common case would be remotely logging into cloud machines.
One way to avoid excessive host checks for changing IP's is
to add the following to your ```~/.ssh.config```:
```
Host *.amazonaws.com
   StrictHostKeyChecking no
   UserKnownHostsFile /dev/null
   LogLevel QUIET
```
