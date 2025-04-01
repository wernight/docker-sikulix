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

    $ docker run --name sikulix --rm -p 3000:3000 -v $PWD/config:/config wernight/sikulix

Once started, open http://localhost:3000 to access the UI.

Tip: After resizing the browser window (especially when elarging it), restart SikuliX so that it takes the entire screen area.

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
  - **Tessereact** used by SikuliX for OCR (you may want to set `Settings.OcrLanguage = "eng"` in your script).
      - WARNING: Currently giving an error: "BROWSE action is not supported".
  - **Firefox** web browser pre-installed.
  - **scrcpy** (and ADB) to remote control an Android device (see section).
  - **SimpleScreenRecorder** to record the screen (either for a demo or to see what it did afterwards).
  - **key-mon** to display key strokes.

### Control an Android device

 1. On the Android device: Enable USB Debugging (in Developer Options, tap "Build number" 7 times in Settings → About Phone).
 1. Start this container and either:
      - [Lower latency] Start with additional Docker argument `--device /dev/bus/usb:/dev/bus/usb` (if it fails try `--privileged --security-opt seccomp=unconfined --security-opt label=disable`).
      - [Less permissions] Install ADB on your host machine and run `adb tcpip 5555`.
 1. Inside the container (i.e. a running instance of this image), run:
    ```bash
    # If it fails try running first "adb connect <DEVICE_IP>:5555" or add "--tcp=<DEVICE_IP>:5555" argument to scrcpy.
    scrcpy --stay-awake --turn-screen-off
    ```

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


## Tips

  - Middle click the title bar to switch to the app under the current window (especially useful when maximized).
  - Right click an empty space on the desktop to start the browser or other applications.
  - Avoid scaling KasmVNC window beyond 1280x768 as screenshots will get cropped.
  - Avoid using manual naming in SikuliX (pop up won't show up after the first time).
  - Change the screenshot shortcut (default is Ctrl-Shift-2) in SikuliX preferences to something like Ctrl-Shift-S.


## Feedbacks

Having more issues? [Report a bug on GitHub](https://github.com/wernight/docker-sikulix/issues).
