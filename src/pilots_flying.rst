.. _chapter-end-user-flying-label:

======
Flying
======

--------
Hunter?!
--------

The goal and capabilities of Hunter are described in :ref:`About <about-label>`.


-------
OPFOR?!
-------
`OPFOR⬀ <https://en.wikipedia.org/wiki/Opposing_force>`_ is an opposing force. Almost all simulated targets are ``OPFOR``. In most situations you are an ``attacker`` and try to destroy OPFOR targets. When there is an event using Hunter, then sometimes a few participants act as ``defenders`` and try to protects ``OPFOR`` from the attackers.

--------------------
Callsigns of Targets
--------------------

By default Hunter shows callsigns like 'OPFORxx' and thereby hides the type of the target. Otherwise the callsigns of the targets are built up like follows (e.g. ``xvehi57``):

* 1 letter identifier (per default `x`)
* Up to 4 letters type
* Two digits running index number

The following types exist:

* building (e.g. depot)
* vehicle (e.g. truck, buk-m2)
* ship (e.g. frigate)
* helicopter
* plane
* unknown

A target's callsign can be reused later for a new and different target. I.e. just because a target was once destroyed you cannot count on that the callsign will be gone.

Depending on the scenario, some of the ships, SAMs, aircraft etc. shoot at you - watch out and know your key binding for chaff/flares!


----------------
Damaging Targets
----------------

NB: :ref:`MP Damage <mp-damage-label>` has to be enabled in the pilot's aircraft!

To make sure that your shooting at targets actually leads to impact, you need to pilot the airplane at the same time as handling the weapons with some accuracy. "Impact" means that the pilot shoots weapons with sufficient destructive power at an asset, so it visibly takes damage and after some time goes away. Ships stop shooting and moving after some impact - and sink, when they are destroyed. Helicopters fall out of the sky.

Each target has a number of hitpoints: the stronger built a target is (e.g. a frigate vs. a truck) the higher the number of hitpoints. Each weapon has a strength in hitpoints: bullets have far fewer hitpoints than a missile. When a target is hit, the weapon's hitpoints reduces the hitpoints of the target based on some heuristics (e.g. distance from the target, a bit of randomness). The heuristics in Hunter are the same as in OPRF assets (if it exists), otherwise they are similar. While the hitpoints for weapons are the same as in OPRF assets (``damage.nas``), the hitpoints for targets might differ in some cases.

A target's health has five states:

* ``fit for fight``: has not been hit yet
* ``hit``: some reduction of hitpoints. Depending on the target aircraft's modelling, some functions might not work anymore
* ``broken``: when the target's hitpoints have been reduced to below a certain threshold (currently 33%), then the target is just handled as a brick. It does not move/fly anymore and radar is off.
* ``destroyed``: hitpoints have reached 0. Explodes and starts burning (if the model supports it). A ship begins to sink.
* ``dead``: after an amount of time depending on the target, the target is removed from MP and not visible anymore. A new target of any type at any time could pop up again using the same callsign.

---------------------------------------
Assets Available to the Attacking Pilot
---------------------------------------

................
Aircraft Carrier
................
For landing and taking off from a carrier, you need to follow the instructions closely - amongst others you have to set the additional command line property correctly - or you will not see the carrier. Use the following resources:

* The general description of `how to use carriers⬀ <https://wiki.flightgear.org/Howto:Carrier>`_ in FlightGear . Includes amongst others the TACAN codes to use in finding the carrier.
* You need to follow at least the installation pilot sections of the `Carrier over MP⬀ <https://wiki.flightgear.org/Carrier_over_MP>`_ wiki article.
* The F-14 is currently the only carrier capable aircraft in OPRF with weapons. There is an extensive description of `how to land on a carrier⬀ <https://wiki.flightgear.org/Howto:Carrier_Landing>`_.

The person launching the scenario will announce which carrier (e.g. the Vinson) is active and which callsign it has (needed to configure the property in FG). Most often it will be the Vinson with callsign ``MP_029X``.

Therefore, in your "Additional Settings" in the FG Launcher you should have the following:

``--ai-scenario=vinson_demo``
``--prop:/sim/mp-carriers/vinson-callsign=MP_029X``

Save yourself frustrations by:

* Closely following the instructions for MP carriers. You will not gain time by just skimming the wiki - you need to read and understand.
* Train carrier recoveries with very little fuel and using AI scenarios. It is difficult and the F-14 is a complex and difficult beast. Crashing after a successful hunt and then also having to re-spawn on land instead of launching on the carrier is not fun.

......
Tanker
......
If there is a tanker for `air-to-air-refueling (AAR)⬀ <https://en.wikipedia.org/wiki/Aerial_refueling>`_, then you can use its callsign to look-up the `TACAN⬀ <https://en.wikipedia.org/wiki/Tactical_air_navigation_system>`_ code according to table in `Howto:Aerial refueling⬀ <https://wiki.flightgear.org/Howto:Aerial_refueling>`_. By default the tanker uses callsign ``TEXACO1`` and thus its TACAN code is ``050X``, but the callsign can be changed in a scenario.

The tanker serves the attackers and is vulnerable to attacks from OPFOR (so whoever creates the scenario needs to take care of this).

NB: it is not a requirement that Hunter provides the tanker. A human pilot supplying a tanker to attacker (or defender) pilots is fine.

.. _awacs-label:

.....
AWACS
.....
If there is an `airborne early warning and control⬀ <https://en.wikipedia.org/wiki/Airborne_early_warning_and_control>`_ plane (aka. AWACS), then its name is by default ``SKYEYE``.

The AWACS serves the attackers and is vulnerable to attacks from OPFOR. The AWACS transmits attacker and OPFOR positions and IFF info using :ref:`Datalink <datalink-label>` - only for flying units like planes, helicopters, drones.


.. _iff-label:

...
IFF
...
Tankers, AWACS and other planes supporting the attackers (you) use the IFF (`identification of friend or foe⬀ <https://en.wikipedia.org/wiki/Identification_friend_or_foe>`_) system, so you can interrogate them. Unless the scenario is run with a different set of parameters, then:

* The OPFOR (the targets plus maybe pilots helping as defenders) will use code ``1``
* The attackers (you) will use code ``6``.

If you get the IFF code wrong, then either the simulated targets or other attackers will shoot at you (or both).


.. _datalink-label:

........
Datalink
........
If the scenario is run with an AWACS (see above), then the AWACS distributes tactical information over a datalink. See `Link 16 Tactical Data Information Link⬀ (TADIL) <https://en.wikipedia.org/wiki/Link_16>`_ as an example - but the OPRF implementation is not trying to mimic any particular system.

Unless the scenario is run with a different set of parameters, then:

* The OPFOR (the simulated targets plus maybe pilots helping as defenders) will use channel ``11``
* The attackers (you) will use code ``66``.

-----------------
Target Behaviours
-----------------

................
Radar of targets
................
The following target types have active radar, which can be sensed by OPRF enabled aircraft:

* Warships
* GCI
* SAMs and certain other anti-aircraft weapon systems (like the Shilka)
* Fighter aircraft, tanker, AWACS

If a target's health is broken or it is destroyed, then the radar is turned off also on these target types. Some targets might turn the radar off based on heuristics taking the distance to attacking aircraft into account. Some SAMs might randomly turn their radar on and off.

If the target's radar acquires a lock, it sets the STT signal in OPRF.

.............................
Targets Shooting at Attackers
.............................

When running Hunter there is a set of :ref:`Command Line Arguments for Running Hunter <chapter-cli-label>`. The ``-o`` option determines, what actually is shooting in the given session.

Whoever runs Hunter will be able to provide information about how hostile the currently run scenario is - or have a look at the session info in the web-site whether something might be shooting at you at all:

* Certain attack helicopters like the `Ka-50⬀ <https://en.wikipedia.org/wiki/Kamov_Ka-50>`_ or the `Mil Mi-24⬀ <https://en.wikipedia.org/wiki/Mil_Mi-24>`_ defend themselves using `infrared homing⬀ <https://en.wikipedia.org/wiki/Infrared_homing>`_ missiles. The simulation is a compromise between the `9K38 Igla⬀ <https://en.wikipedia.org/wiki/9K38_Igla>`_ (basis) and the `FIM-92 Stinger⬀ <https://en.wikipedia.org/wiki/FIM-92_Stinger>`_. Max speed is ca. mach 1.8 and max range is ca. 4600 metres (longer and faster if launched from moving target).
* The same applies for the `MQ-9 Reaper⬀ <https://en.wikipedia.org/wiki/General_Atomics_MQ-9_Reaper>`_ drone.
* Depending on the setting ships will either shoot anti-aircraft bullets (a loose interpretation of the `Kashtan-M⬀ <https://en.wikipedia.org/wiki/Kashtan_CIWS>`_ - only the guns part) or surface-to-air missiles flying up to mach 5+ and >> 60 km.
* Often there will be simulated `Shilkas⬀ <https://en.wikipedia.org/wiki/ZSU-23-4_Shilka>`_ (see `also <https://archive.org/details/DTIC_ADA392785/page/n3/mode/2up>`_) -- they are less accurately modelled, but still deadly if you fail to respect them (you can get easy kills with `anti-radiation missiles⬀ <https://en.wikipedia.org/wiki/Anti-radiation_missile>`_ -- but what if you use conventional weapons?).
* If there are SAMs (e.g. S-300) run as ``FG instances``, then they will launch realistically modelled missiles at you over tens of nautical miles. Otherwise, if SAMs are set to be shooting, then BUK-M2 will shoot surface-to-air missiles with up to mach 3 and up to ca. 30 km distance, S-300 will do the same up to mach 5+ and >> 60 km and the SA-3 will also shoot. Other SAMs in the scenario will either be faking to be active with radars etc. (but will actually not shoot at you) or will be replaced by Shilkas depending on the setting.
* With the Shilka-IR there is a Shilka-like fantasy vehicle, which launches IR-missiles in `MANPAD⬀ <https://en.wikipedia.org/wiki/Man-portable_air-defense_system>`_ style (using the Igla as per above; the Shilka model is used such that the nozzle fire gives a bit of a hint, where the man-pads come from).
* If there are "automats" (fighter planes run as ``FG instances``): they shoot bullets or launch missiles in a realistic way until they run out of ammunition. They will actively engage you.

If something is shooting missiles at you, then from that specific target there will always only be one active missile in the air. If a gun is firing at you, then often it will be fired in short bursts with some shorter (ship) or longer (Shilka) breaks inbetween.

``FG instances`` are a bit special and will have to be announced specifically by the person running Hunter.

The simulated Shilka (ZSU-23-4) has amongst others the following characteristics:

* Has an active radar, which is toggled on if an attacker is within 12 km (amongst others to simulate that the radar is quite weak)
* Active tracking of attackers within 10 km. It takes minimum 8 seconds to acquire and lock a target - but the Shilka can track a previously locked and now hidden target for up to 4 seconds.
* On top of target acquisition the turret also needs to adjust (max 15 degrees per second vertically and 30 degrees per second horizontally).
* Max shooting distance is 2500 metres - it shoots 2 consecutive bursts and then is silent for a few seconds in order not to reveal its position (you can see the muzzle fire, not the bullets).
* The visibility of the Shilka can be constrained by terrain - and it cannot detect attackers below 0.5 degrees above terrain (ca. 8 metres per 1000 metres distance). In hilly surroundings you can come close to the Shilka with the cannon or rockets by hiding behind terrain and then profit from the Shilka's rather slow acquisition time.


.........................
Targets with Self Defense
.........................

Some targets (e.g. helicopters) can automatically release countermeasures (flares/chaff). So you might need to sneak in and get close enough or wait until they have lost all their countermeasures.


.....................................
Defeating Targets Attacking the Pilot
.....................................

The implementation of the radars in Hunter is currently rather primitive. Therefore, some dodging/defeating techniques are currently not possible, because the radar in Hunter would not notice :-(

* The radar and missiles will respect the line-of-sight, but will not differentiate aspect angles (which is worst against infrared homing).
* Line-of-sight takes the curvature of the earth into account (useful over the ocean).
* But line-of-sight is only checking the angle above the highest point on the line of sight - not the type of surface or vegetation (woods) or buildings etc.
* The missiles will run out of energy based an actual use of energy and not only time/distance - i.e. an intelligently maneuvering pilot can defeat a missile.
* The missiles from Hunter targets are all either `infrared homing⬀ <https://en.wikipedia.org/wiki/Infrared_homing>`_ (Fox 2) or `active radar homing⬀ <https://en.wikipedia.org/wiki/Active_radar_homing>`_ (Fox 3). I.e. you have to hide from the missile coming towards you, not the radar detecting you. Destroying the asset shooting at you prevents new missiles coming after you, but does not defeat the missiles already in the air.
* The missiles will bite on your aircraft's counter-measure with a given probability - the more counter-measures the better.
* Each target is shooting at attackers on their own. E.g. two frigates will not coordinate which one shoots at the attacker - both will shoot if they see the attacker.
* Each target is only tracking and shooting at one attacker only.
