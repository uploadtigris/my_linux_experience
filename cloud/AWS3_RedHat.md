## AWS & RedHat / Fedora

Fedora Linux has been integrated with Amazon Web Services (AWS) in several ways. Starting in 2022, AWS's Amazon Linux will be based on Red Hat's Fedora community Linux.

For those interested in installing the AWS Command Line Interface (CLI) on Fedora, it can be done via the dnf package manager with the command ```dnf install awscli```.

Users who want to run Fedora with a graphical user interface (GUI) on AWS can install a desktop environment using dnf group install 'Xfce Desktop' or similar commands and then use X11 Forwarding via SSH or a VNC server to access the GUI remotely.
(^^^ I want to explore this ^^^)
