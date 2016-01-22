[![](https://badge.imagelayers.io/wernight/sikulix:latest.svg)](https://imagelayers.io/?images=wernight/sikulix:latest 'Get your own badge on imagelayers.io')

**[SikuliX](http://www.sikulix.com/)** (previously known as *Sikuli*) automates anything you see on the screen. It uses image recognition powered by [OpenCV](http://opencv.org/) to identify and control GUI components. This is handy in cases when there is no easy access to a GUI's internals or the source code of the application or web page you want to act on.

You want to automate some repetetive tasks in:

  - daily usage of applications or web pages
  - playing games
  - administration of IT systems and networks
  - test applications or web pages
  - make/run a demo or training material live


### Feature of this Dockerized image

Contains latest **SikuliX with Jypthon** (currently the nightly build 1.1.1 because it's the first that can still run even when *wmctrl* isn't fully working, on Xvfb):

 * **Simple** to run (see usage below).
 * **Small** image (or as small as possible).
 * **Secure**: Meant to be run as any user (non `root` user is good).


### Usage

#### Run the GUI

Run the **GUI** (must run as your current user):

    $ docker run --rm -it --user $UID -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v $PWD:/code wernight/sikulix

Note: Here we mounted current directory as `/code` so you may store your scripts there. This is of course optional.


#### Run headless

**HEADLESS PROBABLY WON'T WORK AS OF CURRENT STATE - YOU'LL HAVE TO FIDDLE WITH IT**

First you need to run a X11 program, for example you may run *Chromium* using *Xvfb* (but you may as well run *Android* the same way):

    $ docker run --rm --name chromium -v /tmp/.X11-unix -it markadams/chromium-xvfb xvfb-chromium

Now run an existing script on it:

    $ docker run --rm -e DISPLAY=:99 --volumes-from chromium -v $PWD:/code:ro wernight/sikulix runsikulix -c -r /code/my_script.sikuli

See [SikuliX command-line options](https://sikulix-2014.readthedocs.org/en/latest/faq/010-command-line.html) or run `runsikulix -h`.

WARNING: Due to [bug #183](https://github.com/RaiMan/SikuliX-2014/issues/183) use only the short command-line form for flags (e.g. `-h` instead of `--help`).

Keep in mind that to forward X you'll need:

  * Run as the same user.
  * Use the same `DISPLAY` environment variable (usually `:99` if using Xvfb).
  * Mount `/tmp/.X11-unix` (as read-only).

Note: Here we run as `root` to make it simpler, but you should avoid it (for security). Add `--user` flag and run as any user you want, like any random integer in [2000-32000] as long as that user has read access to your mounted files, and it's the same as the one running X11 (i.e. Xvfb).


### Feedbacks

Having more issues? [Report a bug on GitHub](https://github.com/wernight/docker-sikulix/issues).
