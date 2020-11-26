appbundler
=============

This is a Java ant task that creates a macOS app Java bundler.
This repo is forked from the
[Infinite Kind Application Bundler](https://github.com/TheInfiniteKind/appbundler),
which in turn is a fork (see "Previous Updates" below) from the original
[Java Application Bundler](https://svn.java.net/svn/appbundler~svn).

Details of the ant task are given in the
[ant task documentation](http://htmlpreview.github.io/?https://github.com/rschmunk/appbundler/blob/main/appbundler/doc/appbundler.html).

These macOS system properties are passed to the JVM:

- `LibraryDirectory`
- `DocumentsDirectory`
- `CachesDirectory`
- `ApplicationSupportDirectory`
- `ApplicationDirectory`
- `AutosavedInformationDirectory`
- `DesktopDirectory`
- `DownloadsDirectory`
- `MoviesDirectory`
- `MusicDirectory`
- `PicturesDirectory`
- `SharedPublicDirectory`
- `SystemLibraryDirectory`
- `SystemApplicationSupportDirectory`
- `SystemCachesDirectory`
- `SystemApplicationDirectory`
- `SystemUserDirectory`
- `UserHome`  (the user's home directory, even if running within a sandbox)
- `SandboxEnabled` (the String `true` or `false`)
- `LaunchModifierFlagCapsLock` (the String `true` or `false`)
- `LaunchModifierFlagShift` (the String `true` or `false`)
- `LaunchModifierFlagControl` (the String `true` or `false`)
- `LaunchModifierFlagOption` (the String `true` or `false`)
- `LaunchModifierFlagCommand` (the String `true` or `false`)
- `LaunchModifierFlagNumericPad` (the String `true` or `false`)
- `LaunchModifierFlagHelp` (the String `true` or `false`)
- `LaunchModifierFlagFunction` (the String `true` or `false`)
- `LaunchModifierFlags` (an Integer)


## Example ant task

Following is a typical ant task definition.
[Additional examples](https://github.com/rschmunk/appbundler/blob/main/taskexamples.md)
are also available.

    <target name="bundle">
      <taskdef name="bundleapp" 
        classpath="appbundler-1.0ea.jar"
        classname="com.oracle.appbundler.AppBundlerTask"/>

      <bundleapp 
          classpathref="runclasspathref"
          outputdirectory="${dist}"
          name="${bundle.name}"
          displayname="${bundle.displayname}"
          executableName="MyApp"
          identifier="com.company.product"
          shortversion="${version.public}"
          version="${version.internal}"
          icon="${icons.path}/${bundle.icns}"
          mainclassname="Main"
          copyright="2012 Your Company"
          applicationCategory="public.app-category.finance">
          
          <runtime dir="${runtime}/Contents/Home"/>

          <arch name="x86_64"/>
          <arch name="i386"/>

          <!-- Define custom key-value pairs in Info.plist -->
          <plistentry key="ABCCustomKey" value="foobar"/>
          <plistentry key="ABCCustomBoolean" value="true" type="boolean"/>

          <!-- Workaround as com.apple.mrj.application.apple.menu.about.name property may no longer work -->
          <option value="-Xdock:name=${bundle.name}"/>

          <option value="-Dapple.laf.useScreenMenuBar=true"/>
          <option value="-Dcom.apple.macos.use-file-dialog-packages=true"/>
          <option value="-Dcom.apple.macos.useScreenMenuBar=true"/>
          <option value="-Dcom.apple.mrj.application.apple.menu.about.name=${bundle.name}"/>
          <option value="-Dcom.apple.smallTabs=true"/>
          <option value="-Dfile.encoding=UTF-8"/>

          <option value="-Xmx1024M" name="Xmx"/>
      </bundleapp>
    </target>


## Updates

### Updates in this fork

Alterations made by this fork from the Infinite Kind fork include:

- Remove any preference or requirement for JRE vs. JDK Java.

### Previous updates

Alterations made by the Infinite Kind fork from the original application bundler include:

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
