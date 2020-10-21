# Setting directories and app packages to be ignored by Dropbox from within the Xojo IDE

When I’m working with Xojo I (of course) want the project (and support files) constantly uploaded by Dropbox whenever they are modified.

But I don’t want the builds folder uploaded (at least, not all the time – maybe just at the end of the project when I’m done, or at the most at major milestones) and I **definitely** don’t want the `my-app-name.debug.app` package constantly chundering away in the background every time I hit `Run` in the IDE.

It’s possible to set the Builds directory to be ingnored by Dropbox from within the Dropbox app itself, but with even this simple thing I’ve sometimes encountered problems. It’s also possible to set the debug app package to be ignored, but that's really too much like hard work ...

The sample project contains two build steps, one a script that runs on `Build`, the other a script that executes on `Run`. Both run after the build step itself, and both do the same thing: set Dropbox to ignore whatever is being built via the command line `xattr` command.

(Find out more about `xattr` [here](https://en.wikipedia.org/wiki/Extended_file_attributes) and [here](https://ss64.com/osx/xattr.html).)

The debug/run script sets just the appp package to be ignored:

    var path as String = CurrentBuildLocationNative + "/" + CurrentBuildAppName + ".app"

    var result as String

    result = DoShellCommand( "xattr -w com.dropbox.ignored 1 """ + path + """" )

    if result <> "" then print( result )

The build script sets the entire build products drectory to be ignored:

    var bits() as String = CurrentBuildLocationNative.Split( "/" )

    var path as String

    for i as Integer = 0 to bits.LastRowIndex - 1
  
      path = path + bits( i )
  
      if i < bits.LastRowIndex - 1 then path = path + "/"
  
    next

    var result as String

    result = DoShellCommand( "xattr -w com.dropbox.ignored 1 """ + path + """" )

    if result <> "" then print( result )

I only tend to work on relatively simple stuff, so the two scenarios above are adequate for me ... your own mileage may vary.

But for me, the end result is a work folder with just the project (and other stuff uploading), and all the transient crud from the endless run-debug-sometimes-build-to-see-what-happens cycle not polluting my Dropbox account.

![Screenshot](https://github.com/charlierobin/xojo-dropbox-ignore/blob/main/images/Screen%20Shot%202020-10-21%20at%2008.23.26.png)

This was all last checked in Xojo 2019 r3.1, but I see no reason why it shouldn’t be good for a lot of other versions spanning the years.

The original Dropbox help page about ignoring folders is [here](https://help.dropbox.com/files-folders/restore-delete/ignored-files).

(The Dropbox help page outlines how this can also be done on Windows and Linux, but I’m a Mac person, so sorry! ... You only get the OSX version on a plate. The other platform you’ll have to adapt for yourself.)
