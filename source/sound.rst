=====
Sound
=====

Adventure contains an API to play any built-in or resource pack-provided sound. Note that
not all platforms implement playing sound.

Constructing a Sound
^^^^^^^^^^^^^^^^^^^^

Sounds are composed of:
  * A Key (also known as ``Identifier`` or ``ResourceLocation``) that decides which sound to play. Any custom sounds from resource packs can be used. If a client does not know about sounds, it will ignore the sound (though a warning will be printed to the client log).
  * A Sound source, used to tell the client what type of sound its hearing. The clients sound settings are also attributed to a source.
  * A number, determining the radius where the sound can be heard
  * A number from 0 to 2 determining the pitch the sound will be played at

**Examples:**

.. code:: java

  // Create a built-in sound using standard volume and pitch
  Sound musicDisc = Sound.sound(Key.key("music_disc.13"), Sound.Source.MUSIC, 1f, 1f);

  // Create a sound from our resource pack with a higher pitch
  Sound myCustomSound = Sound.sound(Key.key("adventure", "rawr"), Sound.Source.AMBIENT, 1f, 1.1f);

Playing a Sound
^^^^^^^^^^^^^^^

.. warning::

  The client can play multiple sounds at once, but as of version 1.16 is limited to 8 sounds playing at once.

  Due to `MC-138832 <https://bugs.mojang.com/browse/MC-138832>`_, the volume and pitch of sounds played with an emitter may be ignored.

  As documented in `MC-146721 <https://bugs.mojang.com/browse/MC-146721>`_, any stereo sounds will not play at a specific position or following an entity, therefore, the location or emitter parameters will be ignored.

Once you've created a sound, they can be played to an audience using multiple methods:

.. code:: java

  // Play a sound at the location of the audience
  audience.playSound(sound);

  // Play a sound at a specific location
  audience.playSound(sound, 100, 0, 150);

  // Play a sound that follows the audience member
  audience.playSound(sound, Sound.Emitter.self());

  // Play a sound that follows another emitter (usually an entity)
  audience.playSound(sound, someEntity);

Stopping Sounds
^^^^^^^^^^^^^^^

A sound stop will stop the chosen sounds -- ranging from every sound the client is playing, to specific named sounds.

.. code:: java

   public void stopMySound(final @NonNull Audience target) {
    // Stop a sound for the target
    target.stopSound(SoundStop.named(Key.key("music_disc.13"));
    // Stop all weather sounds for the target
    target.stopSound(SoundStop.source(Sound.Source.WEATHER));
    // Stop all sounds for the target
    target.stopSound(SoundStop.all());
  }

Sound stops can be constructed using the methods in the example block above.
Alternatively, they can be constructed directly from a sound.

.. code:: java

  // Get a sound stop that will stop a specific sound
  mySound.asStop();

  // Sounds can also be stopped directly using the stopSound method
  audience.stopSound(mySound);

Creating a custom sound
^^^^^^^^^^^^^^^^^^^^^^^^

Use the ``sounds.json`` to define sounds in a resource pack. Further reading about this limits can be done at the `Minecraft Wiki <https://minecraft.gamepedia.com/Sounds.json>`_

