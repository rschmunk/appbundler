appbundler
=============

A Java ant task that creates a macOS app Java bundler.

This repo is forked from the
[Infinite Kind Application Bundler](https://github.com/TheInfiniteKind/appbundler),
which in turn is a fork (see "Previous Updates" below) from the original
Java Application Bundler (formerly distributed from Java.net).

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

Updates made in this fork of the appbundler are
[described separately](https://github.com/rschmunk/appbundler/blob/main/updates.md).
