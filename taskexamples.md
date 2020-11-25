appbundler
=============

# Example ant task definitions

## Example 1: Basic task, with associated document definitions

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

          <bundledocument extensions="png,jpg"
            icon="${icons.path}/${image.icns}"
            name="Images"
            role="editor"
            handlerRank="owner">
          </bundledocument>

          <bundledocument contentTypes="com.adobe.pdf"
            name="PDF files"
            role="viewer"
            handlerRank="alternate">
          </bundledocument>

          <bundledocument contentTypes="com.my.custom"
            name="Custom data"
            role="editor"
            exportableTypes="com.topografix.gpx">
          </bundledocument>

          <typedeclaration
            identifier="com.my.custom"
            description="Custom data"
            icon="${icons.path}/${data.icns}"
            conformsTo="com.apple.package"
            extensions="custom"
            mimeTypes="application/x-custom" />

          <typedeclaration
      	   imported = "true"
            identifier="com.topografix.gpx"
            referenceUrl="http://www.topografix.com/GPX/1/1/"
            description="GPS Exchange Format (GPX)"
            conformsTo="public.xml"
            extensions="gpx"
            mimeTypes="application/gpx+xml" />


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

## Example 2, use installed Java but require Java 11 (or later):

    <target name="bundle">
      <taskdef name="bundleapp"
        classpath="appbundler-1.0ea.jar"
        classname="com.oracle.appbundler.AppBundlerTask"/>
      <bundleapp
          jvmrequired="11"
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
      </bundleapp>
    </target>

## Example 3, bundle a stripped down JRE that only needs java.base and java.desktop for a non modularized app:

    <target name="bundle">
      <!-- Obtain path to the selected JRE -->
      <exec executable="/usr/libexec/java_home"
             failonerror="true"
             outputproperty="runtime">
        <arg value="-v"/>
        <arg value="11"/>
      </exec>

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
          copyright="2019 Your Company"
          applicationCategory="public.app-category.finance">

          <jlink runtime="${runtime}">
            <jmod name="java.base"/>
            <jmod name="java.desktop"/>

            <!-- Zip the generated modules file (Optional) -->
            <argument value="--compress=2"/>

            <!-- Preserve all release properties, but update the modules
              -  list. (Optional)
              -
              -  See https://bugs.openjdk.java.net/browse/JDK-8179563
             -->
            <argument value="--release-info=${runtime}/release"/>
          </jlink>
      </bundleapp>
    </target>

## Example 4, bundle a stripped down JRE that only needs java.base and java.desktop for a modularized app:

    <target name="bundle">
      <!-- Obtain path to the selected JRE -->
      <exec executable="/usr/libexec/java_home"
             failonerror="true"
             outputproperty="runtime">
        <arg value="-v"/>
        <arg value="11"/>
      </exec>

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
          mainclassname="mymodule/org.company.MainClass"
          copyright="2019 Your Company"
          applicationCategory="public.app-category.finance">

          <jlink runtime="${runtime}">
            <jmod name="java.base"/>
            <jmod name="java.desktop"/>

            <!-- Zip the generated modules file (Optional) -->
            <argument value="--compress=2"/>

            <!-- Preserve all release properties, but update the modules
              -  list. (Optional)
              -
              -  See https://bugs.openjdk.java.net/browse/JDK-8179563
             -->
            <argument value="--release-info=${runtime}/release"/>
          </jlink>
      </bundleapp>
    </target>

