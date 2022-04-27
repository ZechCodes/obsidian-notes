To have something run in the background on [[macOS]] you have to edit the plist file and use launchctl.

**Reference**
- https://stackoverflow.com/questions/9522324/running-python-in-background-on-os-x

---
**Orgininal Source**

If you want to have the script running as a daemon process which starts automatically, you can use [launchctl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/launchctl.1.html) and a plist file.

For example, Bob has a simple python script which writes the word 'foo' to a file every second in his home directory:

```python
#!/usr/bin/env python
import os
import time

while True:
  os.system('echo " foo" >> /Users/bob/foostore.txt')
  time.sleep(1)
```

To have it run as a daemon process, create a plist file, `~/Library/LaunchAgents/com.bobbob.osx.test.plist`, with the contents:

```python
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC -//Apple Computer//DTD PLIST 1.0//EN http://www.apple.com/DTDs/PropertyList-1.0.dtd >
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.bobbob.osx.test</string>
    <key>Program</key>
    <string>/Users/bob/pyfoo.py</string>
    <key>KeepAlive</key>
    <true/>
  </dict>
</plist>
```

Then use `launchctl` to load the plist from a terminal:

```python
launchctl load ~/Library/LaunchAgents/com.bobbob.osx.test.plist
```

This will load that script and immediately run the program in the `<string>` element beneath `<key>Program</key>`. You can also specify arguments for the program using a `<ProgramArguments>` node with an array of `<string>` elements. For more information see the [launchd.plist man page](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html#//apple_ref/doc/man/5/launchd.plist)

If you want to remove the script, you can use the unload command of `launchctl`:

```python
launchctl unload ~/Library/LaunchAgents/com.bobbob.osx.test.plist
```

The Label used in the script can be anything, but it should be unique on your system, so Apple generally uses a reversed domain name.

As for autorunning a script, I don't think there's any way to do that.