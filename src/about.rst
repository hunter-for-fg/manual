
.. _about-label:

============
About Hunter
============

``Hunter`` is a combat simulation backend server for the free flight simulator `FlightGear⬀ <https://flightgear.org/>`_. It places different types of static and moving targets into `FlightGear's Multiplayer⬀ <https://wiki.flightgear.org/Howto:Multiplayer>`_, such that virtual pilots can fly attacks against air and ground targets. The server supports the work in the FlightGear flight sim military community `Operation Red Flag (OPRF - on Discord)⬀ <https://discord.gg/ptVapkE>`_.

``Hunter`` uses FlightGear's Multiplayer (MP) capability, because multiplayer allows several people to fly together (`Nasal⬀ <https://wiki.flightgear.org/Nasal_scripting_language>`_ is better suited for single player). And because historically OPRF-assets have been written for multiplayer.

``Hunter`` can be used for training or as the basis of community events, where several pilots fly together or against each other. While ground attack is simulated pretty well, air to air is more rudimentary - amongst others because dogfights in OPRF are mostly flown between participants and therefore need not be simulated (which is also much harder to do realistically).

Some capabilities of ``Hunter``:

* Inject dozens of simulated targets into multiplayer. Hunter has been used with 100+ simulated targets.
* Show static targets (e.g. trucks, bunkers).
* Show moving targets (e.g. ships/trucks/helicopters following random routes in a predefined network).
* MP-enabled: tanker for air-to-air refueling, AWACS for situational awareness, carrier for naval aviation.
* Depending on a target's strength it takes more explosives to kill it.
* Depending on how broken a target is, it might not move anymore and not shoot anymore. When it is broken, it gives visual feedback (explosion).
* There are some targets, which can detect you with or without radar and shoot back with guns or missiles.
* A relatively easy to read and write scenario definition, where static and dynamic targets can be defined including the related networks.
* Scripts to automatically create helicopter, road and ship networks based on `OpenStreetMap⬀ <https://www.openstreetmap.org/>`_ and FlightGear scenery data.

``Hunter`` can currently be run in the following modes:

* Based on a Docker image available to all interested: static and moving targets incl. shooting targets (a parameter can determine whether they should shoot back or not).
* Ditto but including a simple `Web UI⬀ <https://mango-meadow-0bc8d0703.5.azurestaticapps.net/>`_ showing damage statistics, a map of airports, a map of targets as well as a list of targets.



The name ``Hunter`` refers to the beautiful British `Hawker Hunter⬀ <https://en.wikipedia.org/wiki/Hawker_Hunter>`_, which for many years provided the ground attack capability for the `Swiss Air Force <https://en.wikipedia.org/wiki/Swiss_Air_Force>`_.

-------
Credits
-------

The interaction with FlightGear multiplayer protocol is using parts of `ATC-pie⬀ <https://wiki.flightgear.org/ATC-pie>`_ by Michael Filhol during execution.

Some data preparation like scenery objects (buildings, roads, etc.) as well as routing data for movable targets like ships are based on `OpenStreetMap (OSM)⬀ <https://www.openstreetmap.org/>`_ data being processed with `osm2city <https://wiki.flightgear.org/Osm2city.py>`_.

See the ``requirements...txt`` files in the root of the `repository⬀ <https://gitlab.com/vanosten/hunter>`_ to see which other software Hunter is depending on directly.

------
People
------

**Core Developers**: Rick Gruber-Riemer (rick AT vanosten DOT net)

**Contributors**: Coaching by OPRF members like ``Leto``, ``pinto``, ``JMav``, ``Richard``, ``Rudolf`` and others. Damage code and weapons logic is heavily based on `OpRedFlag - Meta data for Operation Red Flag Aircraft and assets⬀ <https://github.com/NikolaiVChr/OpRedFlag>`_ by OPRF members.


------------
Contributing
------------

The author could need some help with creating 3D models, research of how things work in real life, extend the web UI or programming in Python.

The main Git repositories are:

* The server: https://gitlab.com/vanosten/hunter
* The scenarios: https://gitlab.com/vanosten/hunter-scenarios


-------
License
-------
The different parts of Hunter are licensed under `GNU GPLv2⬀ <https://www.gnu.org/licenses/old-licenses/gpl-2.0.html>`_.
