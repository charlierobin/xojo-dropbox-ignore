# xojo-dropbox-ignore

When I’m working with Xojo I (of course) want the project (and support files) constantly uploaded by Dropbox whenever they are modified.

But I don’t want the builds folder uploaded (at least, not all the time – maybe just at the end when all is complete) and I **definitely** don’t want the my-app-name.debug.app package constantly chundering away in the background every time I hit Run in the IDE.

It’s possible to set the Builds directory to be ingnored by Dropbox from within the Dropbox app itself, but with even this simple thing I’ve sometimes encountered problems. It’s also possible to set the debug app package to be ignored, but that's really too much like hard work ...

The sample project contains two build steps, one a script that runs on Build, the other a script that runs on Run. Both run after the build step itself, and both do the same thing: set Dropbox to ignore whatever is being built via the command line xattr command.

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
