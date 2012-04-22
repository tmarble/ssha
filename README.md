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



