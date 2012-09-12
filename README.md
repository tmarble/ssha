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

First terminal:

    $ ssha start # start the ssh-agent
	$ ssh-add    # add your private key material
	$ ssh host-which-has-my-key
	
Second and subsequent terminal:

    $ ssha connect # connect to the running ssh-agent
	$ ssh host-which-has-my-key

## Scripting

You can call ssha from other scripts (see also [sshatest](https://raw.github.com/tmarble/ssha/master/sshatest))

    ssha_functions=true
    . /path/to/ssha
    if do_ssha_connect; then
      echo "connected to the ssh-agent"
    else
      echo "WARNING: NOT connected to the ssh-agent, starting one now..."
      do_ssha_start
      if [ $? -eq 2 ]; then
        # ssh-agent is running, but does not have any identities
        # assume a passwordless ssh key
        echo "adding key to ssh-agent..."
        if ! ssh-add; then
          echo "ssh key could not be added to ssh-agent (is it passwordless?)."
          exit 1
        fi
      fi
    fi

## Notes

One common use case is to do **ssha connect** when re-logging in
to a server to connect to an already running agent.

