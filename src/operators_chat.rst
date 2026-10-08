==========================
MP Chat Admin Commands ($)
==========================


Cf. :ref:`Chat Commands <chat-commands-label>` pilots can issue MP chat commands to interact with Hunter. that does also work for operators.

If one or more callsigns have been registered as admins on the command line interface with parameter ``-a`` during the startup of the Hunter server, then the chat messages of these callsigns are screened for admin commands to Hunter.

There are currently 2 generic admin commands:

* ``$ TD`` + *callsign*: Take down the Hunter controlled callsign (mostly a target shooting at you, but it could also be a tanker).
* ``$ AN`` + *callsign* + *number_of_bullets*: Hit the target with a number of GSh-30 bullets (armament notification). E.g. ``$ td OPFOR34``.
