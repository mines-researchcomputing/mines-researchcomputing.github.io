
# Globus File Transfer

Research Computing recommends Globus as the most efficient way to transfer files between users and other sites. It has both a web interface and a command line interface (CLI). It is supported on the following Mines RC servicers:

* Wendian
* Mio
* Orebits

To use either interface, the first step is to create a personal Globus ID account. (Mines affiliates will be able to use their Mines username)

Globus also offers a feature called Globus Connect Personal for moving files to and from a laptop or desktop computer and other “endpoints.”

You may also may create personal endpoints and transfer file to other users by becoming a Globus Plus user. You may do this by joining the “Mines Globus Plus” group within the web interface.


## Using the web interface

When transferring files between systems, keep in mind that your username might not be the same on each system.

Follow these steps to transfer files.

    Go to the main Globus page ([globus.org](https://globus.org)) and use your Mines credentials to log in. 
    Go to File Transfer.
    Enter your source endpoint on one side of the panel.
    Specify the path where your source files are located and click Go.
    Enter your username and token response or password when you are asked to authenticate.
    Identify your target endpoint in the other panel.
    Specify a destination path.
    Select the files you want to copy.
    Click the arrow button to initiate the transfer.

You can check the status of your transfers any time through the web interface and will be notified when they are complete.


## Transferring files with the command line


See these Globus resources for how to make batch transfers and single item transfers, how to manage endpoints, and additional information.

* [CLI QuickStart Guide](https://docs.globus.org/cli/quickstart/)
* [CLI Examples](https://docs.globus.org/cli/examples/)
* [Globus CLI Reference](https://docs.globus.org/cli/reference/)

## Globus Connect Personal

To set up your laptop or desktop computer to use Globus Connect Personal:

* Go to [Globus Connect Personal](https://www.globus.org/globus-connect-personal) and follow the instructions to download and install it on your local system.
* Add your local system as an endpoint by following the instructions from the Globus Connect website.
* Start Globus Connect, and then sign in to [globus.org](https://globus.org).

Your local system should now appear as an endpoint that can be used for transferring files.

