===================
Installation Server
===================

You do not need to do this as an end user flying in FlightGears multiplayer environment - e.g. when someone else has announced that Hunter is running. In that case just read :ref:`End User Requirements <chapter-end-user-requirements-label>` and :ref:`End User Flying <chapter-end-user-flying-label>`.

NB:

* Installing the `server` means the processes running the multiplayer stuff. There is also an interactive web-site, which is not covered here.
* The server side has exclusively been tested on modern Linux/Ubuntu versions on x86-64/amd64 with `scripted compilation⬀ <(https://wiki.flightgear.org/Scripted_Compilation_on_Linux_Debian/Ubuntu>`_ of FlightGear.
* The whole programmed setup works under the critical assumption that only one instance of Hunter is running with a specific scenario being active at a time on a specific MP-server, such that the FlightGear Multiplayer Server does not get confused etc.


-------------------------------------------------------------------------
Installing and Running The Easy Way - Container for Standalone Controller
-------------------------------------------------------------------------

This installs a standalone Hunter server. It runs only simulated targets (no shooting FG instances, no fighter automats). In return you get a simple installation, simple configuration and relatively low resource usage. You need ca. 1 CPU and 2 GB RAM for running 30+ MP targets. However, be aware that network usage can get significant.

............................
A] Install the prerequisites
............................

- You need a container runtime environment. If you do not understand what this is, then probably you should just go and download the `Docker Desktop⬀ <https://www.docker.com/products/docker-desktop/>`_.
- Clone the scenario files for Hunter into a directory of your choice: https://gitlab.com/vanosten/hunter-scenarios.

..............................................
B] Running on a Computer with Docker Installed
..............................................

* First you need to pull the image: Run ``docker pull vanosten/hunter_container:latest``.
* Check your local images with ``docker images``.
* To start the container: Run ``docker run -p 5001-5110:5001-5110/udp -v /home/vanosten/develop_hunter/hunter-scenarios:/hunter-scenarios vanosten/hunter_container:latest -d /hunter-scenarios -s swiss -o 0_0_0_0_0``.
* To stop the container: Run ``docker ps`` in a different CLI and then run ``docker stop -t 30 NAME``.

There will be a lot of logging messages - you have to live with them for now. After the container has been stopped, the log messages will contain a few rather primitive statistics.

The network port range to configure is so large because the MP targets get assigned numbers up to 99 and there is a bit of slack. You computer needs to have an open firewall in the whole port range plus port 5000 - all of them UDP.

..
    As it is already the default, it is no needed to specify "docker run --restart=no ..."

The MP server is automatically set to ``mpserver.opredflag.com`` for military scenarios and ``mpserver01.flightgear.org`` for the rest. If you want to change the default MP servers start the docker command with something like ``docker run -e HUNTER_MP_SERVER=mpserver.mydomain.net -p ...``.

..
    The scenario type is automatically set to military. If you want to change the scenario type to e.g. ``racetrack``, then you also have to submit an environment variable: ``HUNTER_SCENARIO_TYPE`` (e.g. HUNTER_SCENARIO_TYPE=racetrack).


---------------------------
Installation the Harder Way
---------------------------

If you are a bit IT savvy and know some Python, then you can also do a local installation. The advantage of this is that you

* do not need a container runtime and need less space
* can run against the latest code (the Hunter Docker image is not updated frequently)

Read the `Dockerfile⬀ <https://gitlab.com/vanosten/hunter/-/blob/master/Dockerfile>`_ to get hints about what to do.


.. _chapter-cli-label:

=========================================
Command Line Arguments for Running Hunter
=========================================

A set of command line arguments are available - in the example above ``-s swiss`` means that the Swiss scenario is requested:

* ``-i``: the identifier of this instance will be used as prefix in callsigns for targets. Max 5 letters. E.g. ``-i OPFOR`` leads to callsigns like ``OPFOR25``. Default is "x".
* ``-l``: sets the logging level. Valid levels are DEBUG, INFO (default), WARNING.
* ``-L``: set this flag to log to file on top of console (might have performance impact).
* ``-c``: the callsign used for the controller in MP chat (default = ``h_``).
* ``-s``: The scenario to run (default = "north_norway"). In the `scenario file⬀ <https://gitlab.com/vanosten/hunter/-/blob/master/hunter/scenarios.py>`_ look for methods called "_build_scenario_" and use the rest of the method name as the parameter (e.g. "northern_mariana"). And yes: currently scenarios are hard-coded.
* ``-d``: the path to directory where the hunter scenarios live (e.g. ``/home/vanosten/develop_hunter/hunter-scenarios``). This is the only required command line argument.
* ``-g``: If set then an emulated Ground Controlled Interception (GCI) interface is available.
* ``-o``: Specify how hostile the environment is. Any number > 0 means that part is shooting. A string separated by "_" -> ``#_#_#_#_#``. E.g. ``0_0_1_1_0`` means only ships (using CIWS) and Shilkas are shooting. However, is there are FG-instances (SAMs, automats), then they will shoot no matter what. If the environment is hostile, then ships, drones and helis do not respawn to keep a minimum number of targets of that category available.

    * 1st position: helicopter (IR missiles)
    * 2nd position: drones (IR missiles)
    * 3rd position: ships: 1 = all ships use only CIWS; 2 = ca. 50% use only CIWS, the other ca. 50% use missiles [either/or, not both]; 3 = all ships use only missiles
    * 4th position: Shilkas (artillery)
    * 5th position: SAMs [only Shilka.IR, S-300, SA-3 and BUK-M2 actually shoot missiles - others (e.g. the S-75) are not modelled and will just emit radar etc. to fool]:

        * 0 = nothing shoots;
        * 1 = replace all SAMs with Shilkas;
        * 2 = replace all non-shooting SAMs with Shilkas;
        * 3 = replace all non-shooting SAMs with SA-3;
        * 4 = all shooting SAMs shoot missiles, rest is fooling.

* ``-y``: ``#`` separated list (no space) of reserved callsigns for human defenders - it should be something like ``OPFOR33#OPFOR42`` (only needed if a human defender wants to have a callsign in the same range as the targets' identifiers (see parameter ``-i``)). The entries in this list are only used to reserve callsigns, so they are not taken by simulated targets - you still have to set the correct IFF to not get shot at.
* ``-x``: Use cloud integration meaning that data is stored in cloud and can be accessed through the web application.
* ``-a``: ``#`` separated list (no space) of callsigns for admins in this Hunter session. Allows pilots with one of those callsigns to send commands to Hunter through MP chat.
* ``-t``: Throttle the creation of static targets to have less spike of RAM/CPU. Default is 2 seconds between static targets. Larger values result in less spike of RAM/CPU, but also longer time until all static targets are loaded.
* ``-n``: Do not show this session in the list over sessions in the web application. This can e.g. be used if the participants should not be able to see session information. An admin starting the session will still see the session id in the log output and can thereby access the session in the web-application (e.g. ``/ongoing_session/current_session?session_id=1234`` for the session details, ``/ongoing_session/positions_list?session_id=1234`` for the list of attackers, ``/ongoing_session/positions_map?session_id=1234`` for the map, ``/ongoing_session/damages?session_id=1234`` for damage results, ``/ongoing_session/attacker_events?session_id=1234`` for attacker events etc. -- ``1234`` gets replaced with a pretty long session ID).


==============================
Optional Environment Variables
==============================

* You can set an alternative MP Server (instead of OPRF default = ``mpserver.opredflag.com``) by setting ``HUNTER_MP_SERVER``. However, the standard flightgear.org servers will not be accepted.
* If you are running against cloud (``-x``):

    * Set ``REST_API_BASE_URL`` and ``REST_API_FUNCTION_KEY`` (the values for these must be obtained from the author)
    * Optionally set ``HUNTER_OPERATOR`` to e.g. your Discord name or MP callsign, so it can be seen through the Hunter web-ui, who is running a session.

To run the container with environment variables in a local ``.env`` file use e.g. ``docker run --env-file .env -p 5001-5110:5001-5110/udp -v /home/vanosten/develop_hunter/hunter-scenarios:/hunter-scenarios hunter_controller -d /hunter-scenarios -s test -o 0_0_0_0_0 -x``.

===================================
Some Advise on Network Connectivity
===================================

Hunter is sensitive to the network connection -- especially the number and length of drops as well as latency. Hunter catches network errors and does retries, but often that is not enough and will no matter what lead to a bad experience (e.g. if an attacker plane shoots and hits a target at a moment, where Hunter is not receiving packages).

* If you run Hunter at home: using a wired connection to your router is better than using WIFI.
* Running Hunter on a comparatively small virtual computer (e.g. 2 vCores, 4 GB RAM) in a datacenter will give the best network connectivity.
* Running the MP-server on the same computer as Hunter or on a machine that is very close will give 2 advantages: (a) network connectivity issues are reduced significantly, (b) the lag between Hunter and the MP-server is reduced to ca. almost nothing, which improves the responsiveness of the simulation by a factor 2.


==============================
If You Need More Customization
==============================

Talk to the maintainer of Hunter. If you have found Hunter, then you will know who that is.
