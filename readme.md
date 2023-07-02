# Agon Light Programs

This repo contains various programs for the Agon Light that I've been using as part of my experiments.

Currently, two test programs have been written to demonstrate the new functionality I've submitted as a pull request to the VDP code base:

- MODELIST.BAS
- DBMODETEST.BAS


# MODELIST

This short BASIC program will work through the list of new modes and display all the colours available and text describing the mode. Press a key to progress to the next mode.

# DBMODETEST

This short BASIC program prompts for a mode number, and if provided with a double-buffered mode, will display a line of text as written to the first buffer. On pressing a key, it will display a different line of text written to the second buffer. Press "Q" to quit.

<<<<<<< HEAD
If run against a screen mode without double-buffering, both lines of text will appear at the same time.
=======
If run against a screen mode without double-buffering, both lines of text will appear at the same time.
>>>>>>> 1137a7659610b2083dd8f87ffffd66dac265ce03
