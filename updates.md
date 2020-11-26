appbundler
=============

# Updates

## Updates in this fork

Alterations made by this fork from the
[Infinite Kind fork](https://github.com/TheInfiniteKind/appbundler) include:

- Remove any preference or requirement for a JRE vs. JDK Java.


## Previous updates

Alterations made by the Infinite Kind fork
from the original application bundler include:

- Fixes [icon not showing bug](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=7159381) in `JavaAppLauncher`
- Adds `LC_CTYPE` environment variable to the `Info.plist` file in order to fix an [issue with `File.exists()` in OpenJDK 7](http://java.net/jira/browse/MACOSX_PORT-165)  **(Contributed by Steve Hannah)**
- Allows to specify the name of the executable instead of using the default `"JavaAppLauncher"` **(contributed by Karl von Randow)**
- Adds `classpathref` support to the `bundleapp` task
- Adds support for `JVMArchs` and `LSArchitecturePriority` keys
- Allows to specify a custom value for `CFBundleVersion`
- Allows specifying registered file extensions using `CFBundleDocumentTypes` and `UT[Ex|Im]portedTypeDeclarations`
- Passes to the Java application a set of system properties with the paths of
  the OSX special folders, whether the application is running in the
  sandbox (see below) and which modifier keys are held down while opening the app. With the latter, the Java application can mimic the behavior of e.g. iTunes or Photos to select another or create a new media library on startup with the option key.
- Allows overriding of passed JVM options by the bundled app itself via java.util.Preferences **(contributed by Hendrik Schreiber)**
- Allows writing arbitrary key-value pairs to `Info.plist` via `plistentry`
- Allows setting of environment variables via `Info.plist`
- Allows running the Application as `privileged` - e.g. for Setup **(Contributed by Gerry Weißbach)**
- Allows specifying a JNLP file (`jnlplaunchername`) as alternative to the `mainclassname` which can then be launched without hassle when the Application is signed. See [How to sign (dynamic) JNLP files for OSX 10.8.4 and Gatekeeper](http://stackoverflow.com/questions/16958130/how-to-sign-dynamic-jnlp-files-for-osx-10-8-4-and-gatekeeper) **(Contributed by Gerry Weißbach)**
