**[SikuliX](http://www.sikulix.com/)** (previously known as *Sikuli*) automates anything you see on the screen. It uses image recognition powered by [OpenCV](http://opencv.org/) to identify and control GUI components. This is handy in cases when there is no easy access to a GUI's internals or the source code of the application or web page you want to act on.

You want to automate some repetitive tasks in:

  - daily usage of applications or web pages
  - playing games
  - administration of IT systems and networks
  - test applications or web pages
  - make/run a demo or training material live


## Usage

Contains latest official **SikuliX** inside a container based on **KasmVNC**.

Simplest usage is:

    $ docker run --rm -it -p 3000:3000 wernight/sikulix

Once started, open http://localhost:3000 to access the UI.

See [SikuliX official documentation](https://sikulix.github.io/docs) for the scripting language.

It's based on [LinuxServer.io docker-baseimage-kasmvnc](https://github.com/linuxserver/docker-baseimage-kasmvnc);
see their documentation for supported features like:

  - Change port.
  - Add authentication.
  - Support GPU.
  - [PRoot Apps](https://github.com/linuxserver/proot-apps) (e.g. `proot-apps install chromium`)

The simplest is to then have your application also run from within that X11 virtual session.

Comes preinstalled with:

  - SikuliX (duh!)
  - **Firefox** web browser pre-installed.
  - **scrcpy** (and ADB) to remote control an Android device:
     1. Enable USB Debugging (in Developer Options, tap "Build number" 7 times in Settings → About Phone).
     2. Either:
          - [Lower latency] Start with additional Docker argument `--device /dev/bus/usb:/dev/bus/usb` (if it fails try `--privileged --security-opt seccomp=unconfined --security-opt label=disable`).
          - [Less permissions] Run `adb tcpip 555a && adb connect <DEVICE_IP>:55555`.
     3. From within the container, run: `scrcpy --stay-awake  --turn-screen-off` (possibly preceeded by `adb connect <DEVICE_IP>:5555`).

### Run advanced/headless

**HEADLESS PROBABLY WON'T WORK AS OF CURRENT STATE - YOU'LL HAVE TO FIDDLE WITH IT**

First you need to run a X11 program, for example you may run *Chromium* using *Xvfb* (but you may as well run *Android* the same way):

    $ docker run --rm --name chromium -v /tmp/.X11-unix -it markadams/chromium-xvfb xvfb-chromium

Now run an existing script on it:

    $ docker run --rm -e DISPLAY=:99 --volumes-from chromium -v $PWD:/code:ro wernight/sikulix -- sikulix -c -r /code/my_script.sikuli

See [SikuliX command-line options](https://sikulix-2014.readthedocs.org/en/latest/faq/010-command-line.html) or run `sikulix -h`.

WARNING: Due to [bug #183](https://github.com/RaiMan/SikuliX-2014/issues/183) use only the short command-line form for flags (e.g. `-h` instead of `--help`).

Keep in mind that to forward X you'll need:

  * Run as the same user.
  * Use the same `DISPLAY` environment variable (usually `:99` if using Xvfb).
  * Possibly forward and mount `XAUTHORITY` or `/tmp/.X11-unix` (as read-only).

Note: Here we run as `root` to make it simpler, but you should avoid it (for security). Add `--user` flag and run as any user you want, like any random integer in [2000-32000] as long as that user has read access to your mounted files, and it's the same as the one running X11 (i.e. Xvfb).


## Feedbacks

Having more issues? [Report a bug on GitHub](https://github.com/wernight/docker-sikulix/issues).
