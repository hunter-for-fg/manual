.. _chat-commands-label:


-------------
Chat Commands
-------------

You can submit commands to Hunter through the in-sim chat dialogue. Either open the chat dialogue window using the Menu ``Multiplayer -> Chat Dialog`` or by using shortcut ``_`` (underscore) followed by the text and then press enter/return. As an admin you can submit both admin commands and pilot commands, otherwise only pilot commands.

Please be aware:

* There is currently no feedback, whether a command was successful or aborted (e.g. because the command was misspelled).
* If different pilots submit a command at about the same point in time, only one command might make it.
* In general it does not matter whether you issue a command with capital case or lower case. The exception is when submitting a callsign.
* All pilots see each others chat commands.

The commands need to start with a ``@`` (pilot commands) followed by a blank followed by a command. Depending on the command it needs to be followed by another blank and one or more parameters. E.g.:

* To get the position of the carrier a pilot would submit the following chat message in FlightGear: ``@ c_pos``.
* To steer the carrier you need to add parameters for the direction and speed: e.g. `@ C_WSET 138 15``

...................
Heading Information
...................

NB: all courses / heading are displayed in 360 degrees for True North (see also :ref:`True vs. Magnetic Heading <chapter-true-label>`).


..............................
Pilot Commands (@) for Carrier
..............................

You can always issue carrier related commands, but you might not get an answer, if there is no carrier or if the conditions are not fulfilled (and yes, this is not how it officially works):

* ``@ C_IN`` (condition: closer than 50 nm): Call [INBOUND] to the carrier, the carrier will respond: "[CALLSIGN], mother’s weather is your weather. Expected BRC is [HEADING OF CARRIER]. Report a see me at 10."
* ``@ C_SY`` (condition: closer than 15 nm): Call [SEE YOU AT 10] to the carrier, the carrier will respond: "[CALLSIGN], Tower, Roger. BRC is [HEADING OF CARRIER]".
* ``@ C_POS``: Ask for the carrier's current position. TACAN range is not unlimited (e.g. in real life below 200 nm at high altitudes) and in FlightGear it is often well below 100 nm in the F-14). Therefore, knowing the carrier position and have it available in the route dialogue is important.
* Set the carrier's course type:

    * ``@ C_CTLO``: loiter either around the mid point of the sailing area - or back to the center of the sailing area.
    * ``@ C_CTNA`` + *degrees*: navigate at a speed of 15 kn and change course to asked degrees.
    * ``@ C_CTRE`` + *speed_in_kn*: aircraft recovery with a relative wind speed over angled deck as asked for (the less natural wind there is, the more the wind will come down the BRC instead of the angled deck).
    * ``@ C_CTLA`` + *speed_in_kn*: aircraft launch with a relative wind speed over deck in BRC direction as asked for.

* ``@ C_DLON`` / ``@ C_DLOFF``: Carrier deck lights on or off
* ``@ C_FLON`` / ``@ C_FLOFF``: Carrier flood lights on or off
* ``@ C_WSET`` + *direction_in_deg* + *speed_in_kn*: Set wind direction (in degrees) and speed (in knots) - if not specified then 90° (from East) and 5 kn default. I.e. the carrier does not read the information from the environment in FG.

The carrier navigates in a sail area as defined in the scenario. It the carrier gets close to the border for the sail area, it will enter course type ``evade``, speed up and change course to the centre of the sail area. While in ``evade`` it will not accept new course types.

The MP carrier is restricted to max 30 kn of speed. Therefore, if a high relative wind over deck speed is asked for, it might not be possible if there is not enough natural wind.

.............................
Pilot Commands (@) for Tanker
.............................

If there is a tanker, then you can use the following commands:

* ``@ T_POS``: Ask for the bearing and range to the tanker as well as its height above mean sea level (in thousands of feet).


...........................................................
Pilot Commands (@) for GCI (Ground Controlled Interception)
...........................................................

OPRF assets include a `Ground control interception (GCI)⬀ <https://en.wikipedia.org/wiki/Ground-controlled_interception>`_ station, which might be run in parallel with Hunter. Some OPRF planes like the `MiG-21bis⬀ <https://github.com/l0k1/MiG-21bis>`_, the `Mirage 2000⬀ <https://github.com/5H1N0B11/Aircraft>`_ and the `F-16⬀ <https://github.com/NikolaiVChr/f16>`_ have menu items or key bindings to request information about flying enemies from a GCI-station:

* ``PICTURE`` - full tactical picture
* ``BOGEY DOPE`` - BRAA (Bearing Range Altitude Aspect) of nearest target
* ``CUTOFF`` - vector (bearing and range) to nearest target

In order not to rely on this mechanism (not all OPRF aircraft have it implemented) and to be able to also give guidance on ground-attack targets, Hunter uses chat commands. Also, Hunter scenarios can include an :ref:`AWACS <awacs-label>`, which renders GCI somewhat obsolete.

Therefore, Hunter provides its own set of target assignment requests. You can think of it as a combination of intelligence/reconnaissance picked up by various means of sensors, photographs or field observations by `Forward Air Control (FAC)⬀ <https://en.wikipedia.org/wiki/Forward_air_control>`_. The following requests are available:

* ``@ G_P``: BRAA (Bearing Range Altitude Aspect) plus intercept vector plus estimated time to the nearest available plane (fixed-wing aircraft, not drone). Aspect ``hot`` means target is flying towards pilot (within 30 degs left/right), ``flank`` means it is still somewhat pointing its nose at you (30 - 60 degs left/right), ``beam`` means it is flying ca. at 90 deg angle, ``drag`` means it is flying away in somewhat same direction as you are flying.
* ``@ G_H``: BRAA to the nearest available helicopter.
* ``@ G_D``: BRAA to the nearest available drone.
* ``@ G_OS``: Vector (bearing and range) to the nearest available offshore ship.
* ``@ G_CS``: Vector to the nearest available coastal ship.
* ``@ G_S``: Vector to the nearest available SAM (fixed or movable).
* ``@ G_V``: Vector to the nearest available land vehicle, which is not a SAM.
* ``@ G_B``: Lon/lat and altitude of the nearest available building or other static structure, which is not a SAM.
* ``@ G_C``: Clear my assignments (e.g. if you are out of suitable ammo or returning to base or just want a different type of target or let someone else use it etc.). After this request you will have to issue a new specific request.

``Nearest available`` here means that the target has not yet been assigned to another attacker. Hunter makes no assumptions about the attack capabilities of your aircraft - so you need to pick what is sensible given the weapons you have left.

If the same request type is sent again, updated information for the same target will be displayed (if the target is still alive) - no matter whether the target is still the nearest one or not anymore.

It can be that no target seems to be available - including situations where the target due to aspect and/or speed difference cannot be intercepted. That might change if you increase the speed of your own aircraft and the calculation therefore turns to your advantage.

No information can be requested for targets being FG instances controlled by Hunter or real pilots acting as defenders on the OPFOR side.

.........................................
Chat does not seem to be working for me?!
.........................................

Root causes are often:

* You are not using the correct commands. Hunter will just ignore instead of reminding of correct commands. This is to reduce cluttering the chat messages.
* Try just submitting a short unrelated message (e.g. "hi") to see whether your chats make it to the multiplayer server at all. Afterwards try again. It has been shown to work.
* A programming error: nothing you can do. Provide as much information as possible about the case to the maintainer(s) of Hunter.
