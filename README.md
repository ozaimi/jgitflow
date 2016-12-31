Init
====

The following initialises a File Repository.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
git init --bare /tmp/GITRepo/jgitflow
mkdir -p /tmp/jgitflow/dev
mkdir -p /tmp/jgitflow/build
cd /tmp/jgitflow/dev
git clone /tmp/GITRepo/jgitflow
cd jgitflow
cp -rf ~/Desktop/jgitflow/* .
git add .
git commit -a -m "Initial Release"
git push
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Release Builds
==============

The following describes how to perform the release operation.

Prepare
-------

This is a normally a step initiated by a Release Manager and can be automated or
manual.

1.  Start a release 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    mvn jgitflow:release-start
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    When prompted accept the defaults.

    It should return 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    Nicholass-MBP:jgitflow nirving$ mvn jgitflow:release-start
    [INFO] Scanning for projects...
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Build Order:
    [INFO] 
    [INFO] Parent
    [INFO] Next Sample
    [INFO] Sample
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Parent 0.0.4-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [WARNING] The following dependencies could not be resolved at this point of the build but seem to be part of the reactor:
    [WARNING] o au.com.versent:nextsample:jar:0.0.4-SNAPSHOT (compile)
    [WARNING] Try running the build up to the lifecycle phase "package"
    [INFO] 
    [INFO] --- jgitflow-maven-plugin:1.0-m5.1:release-start (default-cli) @ parent ---
    [INFO] (development) Checking for SNAPSHOT version in projects...
    [INFO] (development) Checking dependencies and plugins for snapshots ...
    What is the release version for "Parent"? (au.com.versent:parent) [0.0.4]: 
    [INFO] (release-0.0.4) adding snapshot to pom versions...
    [INFO] (release-0.0.4) updating poms for all projects...
    [INFO] turn on debug logging with -X to see exact changes
    [INFO] (release-0.0.4) updating pom for Parent...
    [INFO] (release-0.0.4) updating pom for Next Sample...
    [INFO] (release-0.0.4) updating pom for Sample...
    What is the development version for "Parent"? (au.com.versent:parent) [0.0.5-SNAPSHOT]: 
    [INFO] (development) updating poms with next development version...
    [INFO] (development) updating poms for all projects...
    [INFO] turn on debug logging with -X to see exact changes
    [INFO] (development) updating pom for Parent...
    [INFO] (development) updating pom for Next Sample...
    [INFO] (development) updating pom for Sample...
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary:
    [INFO] 
    [INFO] Parent ............................................. SUCCESS [ 32.836 s]
    [INFO] Next Sample ........................................ SKIPPED
    [INFO] Sample ............................................. SKIPPED
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 33.059 s
    [INFO] Finished at: 2016-02-26T05:46:23+11:00
    [INFO] Final Memory: 13M/231M
    [INFO] ------------------------------------------------------------------------

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Confirm that you are now on the `release` branch 
    
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git branch
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

    It should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    development 
    master
    release-0.0.4
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Add a `version` file  
    
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    echo "Version 0.0.4" > version     
    git add version    
    git commit -a -m "Version 0.0.4"    
    git push`
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Release
=======

1.  Change to the Build directory and ensure it is clean before checking out the
    code and changing to the Release branch. 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    cd /tmp/jgitflow/build/ 
    rm -rf *
    git clone /tmp/GITRepo/jgitflow 
    cd jgitflow 
    git branch -a
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return
    
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
    * master
      remotes/origin/HEAD -> origin/master
      remotes/origin/development
      remotes/origin/master
      remotes/origin/release-0.0.4
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Checkout out the `release-0.0.4` branch 
    
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git checkout -b release-0.0.4
    origin/release-0.0.4
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    returns 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Branch release-0.0.4 set up to track remote branch release-0.0.4 from origin.
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.  Confirm you are on the correct branch. 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git branch
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    returns 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Branch release-0.0.4 set up to track remote branch release-0.0.4 from origin.
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3.  Confirm that `version` contains the data we added previously. 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    cat version
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return 

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Version 0.0.4
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

4.  Once done then perform the release. `mvn jgitflow:release-finish` should
    return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    [INFO] Scanning for projects...
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Build Order:
    [INFO] 
    [INFO] Parent
    [INFO] Next Sample
    [INFO] Sample
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Parent 0.0.4-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [WARNING] The following dependencies could not be resolved at this point of the build but seem to be part of the reactor:
    [WARNING] o au.com.versent:nextsample:jar:0.0.4-SNAPSHOT (compile)
    [WARNING] Try running the build up to the lifecycle phase "package"
    [INFO] 
    [INFO] --- jgitflow-maven-plugin:1.0-m5.1:release-finish (default-cli) @ parent ---
    [INFO] running jgitflow release finish...
    [INFO] (release-0.0.4) Updating poms for RELEASE
    [INFO] (release-0.0.4) removing snapshot from pom versions...
    [INFO] (release-0.0.4) updating poms for all projects...
    [INFO] turn on debug logging with -X to see exact changes
    [INFO] (release-0.0.4) updating pom for Parent...
    [INFO] (release-0.0.4) updating pom for Next Sample...
    [INFO] (release-0.0.4) updating pom for Sample...
    [INFO] (release-0.0.4) Checking for RELEASE version in projects...
    [INFO] (release-0.0.4) Checking dependencies and plugins for snapshots ...
    [INFO] Executing: /bin/sh -c cd /private/tmp/jgitflow && /usr/local/Cellar/maven/3.3.3/libexec/bin/mvn -s /var/folders/t1/sz_cq1tj337_dfk4ryjtm1_80000gn/T/release-settings5415689704011879769.xml clean deploy --no-plugin-updates -DperformRelease=true
        [WARNING] Command line option -npu is deprecated and will be removed in future Maven versions.
        [INFO] Scanning for projects...
        [WARNING] 
        [WARNING] Some problems were encountered while building the effective model for au.com.versent:sample:jar:0.0.4
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-source-plugin is missing. @ au.com.versent:sample:[unknown-version], /private/tmp/jgitflow/sample/pom.xml
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-javadoc-plugin is missing. @ au.com.versent:sample:[unknown-version], /private/tmp/jgitflow/sample/pom.xml
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-deploy-plugin is missing. @ au.com.versent:sample:[unknown-version], /private/tmp/jgitflow/sample/pom.xml
        [WARNING] 
        [WARNING] Some problems were encountered while building the effective model for au.com.versent:nextsample:jar:0.0.4
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-source-plugin is missing. @ au.com.versent:nextsample:[unknown-version], /private/tmp/jgitflow/nextsample/pom.xml
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-javadoc-plugin is missing. @ au.com.versent:nextsample:[unknown-version], /private/tmp/jgitflow/nextsample/pom.xml
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-deploy-plugin is missing. @ au.com.versent:nextsample:[unknown-version], /private/tmp/jgitflow/nextsample/pom.xml
        [WARNING] 
        [WARNING] Some problems were encountered while building the effective model for au.com.versent:parent:pom:0.0.4
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-source-plugin is missing.
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-javadoc-plugin is missing.
        [WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-deploy-plugin is missing.
        [WARNING] 
        [WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
        [WARNING] 
        [WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
        [WARNING] 
        [INFO] ------------------------------------------------------------------------
        [INFO] Reactor Build Order:
        [INFO] 
        [INFO] Parent
        [INFO] Next Sample
        [INFO] Sample
        [INFO]                                                                         
        [INFO] ------------------------------------------------------------------------
        [INFO] Building Parent 0.0.4
        [INFO] ------------------------------------------------------------------------
        [INFO] 
        [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ parent ---
        [INFO] 
        [INFO] >>> maven-source-plugin:3.0.0:jar (attach-sources) > generate-sources @ parent >>>
        [INFO] 
        [INFO] <<< maven-source-plugin:3.0.0:jar (attach-sources) < generate-sources @ parent <<<
        [INFO] 
        [INFO] --- maven-source-plugin:3.0.0:jar (attach-sources) @ parent ---
        [INFO] 
        [INFO] --- maven-javadoc-plugin:2.10.3:jar (attach-javadocs) @ parent ---
        [INFO] Not executing Javadoc as the project is not a Java classpath-capable package
        [INFO] 
        [INFO] --- maven-install-plugin:2.4:install (default-install) @ parent ---
        [INFO] Installing /private/tmp/jgitflow/pom.xml to /Users/nirving/.m2/repository/au/com/versent/parent/0.0.4/parent-0.0.4.pom
        [INFO] 
        [INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ parent ---
        Uploading: file:////tmp/mvn/release/au/com/versent/parent/0.0.4/parent-0.0.4.pom
        Uploaded: file:////tmp/mvn/release/au/com/versent/parent/0.0.4/parent-0.0.4.pom (2 KB at 123.4 KB/sec)
        Downloading: file:////tmp/mvn/release/au/com/versent/parent/maven-metadata.xml
        Uploading: file:////tmp/mvn/release/au/com/versent/parent/maven-metadata.xml
        Uploaded: file:////tmp/mvn/release/au/com/versent/parent/maven-metadata.xml (299 B at 146.0 KB/sec)
        [INFO]                                                                         
        [INFO] ------------------------------------------------------------------------
        [INFO] Building Next Sample 0.0.4
        [INFO] ------------------------------------------------------------------------
        [INFO] 
        [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ nextsample ---
        [INFO] 
        [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ nextsample ---
        [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
        [INFO] skip non existing resourceDirectory /private/tmp/jgitflow/nextsample/src/main/resources
        [INFO] 
        [INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ nextsample ---
        [INFO] Changes detected - recompiling the module!
        [WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
        [INFO] Compiling 1 source file to /private/tmp/jgitflow/nextsample/target/classes
        [INFO] 
        [INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ nextsample ---
        [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
        [INFO] skip non existing resourceDirectory /private/tmp/jgitflow/nextsample/src/test/resources
        [INFO] 
        [INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ nextsample ---
        [INFO] No sources to compile
        [INFO] 
        [INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ nextsample ---
        [INFO] No tests to run.
        [INFO] 
        [INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ nextsample ---
        [INFO] Building jar: /private/tmp/jgitflow/nextsample/target/nextsample-0.0.4.jar
        [INFO] 
        [INFO] >>> maven-source-plugin:3.0.0:jar (attach-sources) > generate-sources @ nextsample >>>
        [INFO] 
        [INFO] <<< maven-source-plugin:3.0.0:jar (attach-sources) < generate-sources @ nextsample <<<
        [INFO] 
        [INFO] --- maven-source-plugin:3.0.0:jar (attach-sources) @ nextsample ---
        [INFO] Building jar: /private/tmp/jgitflow/nextsample/target/nextsample-0.0.4-sources.jar
        [INFO] 
        [INFO] --- maven-javadoc-plugin:2.10.3:jar (attach-javadocs) @ nextsample ---
        [WARNING] Source files encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
        [INFO] 
        Loading source file /private/tmp/jgitflow/nextsample/src/main/java/AnotherHelloWorld.java...
        Constructing Javadoc information...
        Standard Doclet version 1.8.0_51
        Building tree for all the packages and classes...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/AnotherHelloWorld.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/package-frame.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/package-summary.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/package-tree.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/constant-values.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/class-use/AnotherHelloWorld.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/package-use.html...
        Building index for all the packages and classes...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/overview-tree.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/index-all.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/deprecated-list.html...
        Building index for all classes...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/allclasses-frame.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/allclasses-noframe.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/index.html...
        Generating /private/tmp/jgitflow/nextsample/target/apidocs/help-doc.html...
        [INFO] Building jar: /private/tmp/jgitflow/nextsample/target/nextsample-0.0.4-javadoc.jar
        [INFO] 
        [INFO] --- maven-install-plugin:2.4:install (default-install) @ nextsample ---
        [INFO] Installing /private/tmp/jgitflow/nextsample/target/nextsample-0.0.4.jar to /Users/nirving/.m2/repository/au/com/versent/nextsample/0.0.4/nextsample-0.0.4.jar
        [INFO] Installing /private/tmp/jgitflow/nextsample/pom.xml to /Users/nirving/.m2/repository/au/com/versent/nextsample/0.0.4/nextsample-0.0.4.pom
        [INFO] Installing /private/tmp/jgitflow/nextsample/target/nextsample-0.0.4-sources.jar to /Users/nirving/.m2/repository/au/com/versent/nextsample/0.0.4/nextsample-0.0.4-sources.jar
        [INFO] Installing /private/tmp/jgitflow/nextsample/target/nextsample-0.0.4-javadoc.jar to /Users/nirving/.m2/repository/au/com/versent/nextsample/0.0.4/nextsample-0.0.4-javadoc.jar
        [INFO] 
        [INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ nextsample ---
        Uploading: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4.jar
        Uploaded: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4.jar (2 KB at 622.1 KB/sec)
        Uploading: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4.pom
        Uploaded: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4.pom (513 B at 501.0 KB/sec)
        Downloading: file:////tmp/mvn/release/au/com/versent/nextsample/maven-metadata.xml
        Uploading: file:////tmp/mvn/release/au/com/versent/nextsample/maven-metadata.xml
        Uploaded: file:////tmp/mvn/release/au/com/versent/nextsample/maven-metadata.xml (303 B at 147.9 KB/sec)
        Uploading: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4-sources.jar
        Uploaded: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4-sources.jar (2 KB at 576.2 KB/sec)
        Uploading: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4-javadoc.jar
        Uploaded: file:////tmp/mvn/release/au/com/versent/nextsample/0.0.4/nextsample-0.0.4-javadoc.jar (23 KB at 4405.9 KB/sec)
        [INFO]                                                                         
        [INFO] ------------------------------------------------------------------------
        [INFO] Building Sample 0.0.4
        [INFO] ------------------------------------------------------------------------
        [INFO] 
        [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ sample ---
        [INFO] 
        [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ sample ---
        [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
        [INFO] skip non existing resourceDirectory /private/tmp/jgitflow/sample/src/main/resources
        [INFO] 
        [INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ sample ---
        [INFO] Changes detected - recompiling the module!
        [WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
        [INFO] Compiling 1 source file to /private/tmp/jgitflow/sample/target/classes
        [INFO] 
        [INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ sample ---
        [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
        [INFO] skip non existing resourceDirectory /private/tmp/jgitflow/sample/src/test/resources
        [INFO] 
        [INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ sample ---
        [INFO] No sources to compile
        [INFO] 
        [INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ sample ---
        [INFO] No tests to run.
        [INFO] 
        [INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ sample ---
        [INFO] Building jar: /private/tmp/jgitflow/sample/target/sample-0.0.4.jar
        [INFO] 
        [INFO] >>> maven-source-plugin:3.0.0:jar (attach-sources) > generate-sources @ sample >>>
        [INFO] 
        [INFO] <<< maven-source-plugin:3.0.0:jar (attach-sources) < generate-sources @ sample <<<
        [INFO] 
        [INFO] --- maven-source-plugin:3.0.0:jar (attach-sources) @ sample ---
        [INFO] Building jar: /private/tmp/jgitflow/sample/target/sample-0.0.4-sources.jar
        [INFO] 
        [INFO] --- maven-javadoc-plugin:2.10.3:jar (attach-javadocs) @ sample ---
        [WARNING] Source files encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
        [INFO] 
        Loading source file /private/tmp/jgitflow/sample/src/main/java/HelloWorld.java...
        Constructing Javadoc information...
        Standard Doclet version 1.8.0_51
        Building tree for all the packages and classes...
        Generating /private/tmp/jgitflow/sample/target/apidocs/HelloWorld.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/package-frame.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/package-summary.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/package-tree.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/constant-values.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/class-use/HelloWorld.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/package-use.html...
        Building index for all the packages and classes...
        Generating /private/tmp/jgitflow/sample/target/apidocs/overview-tree.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/index-all.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/deprecated-list.html...
        Building index for all classes...
        Generating /private/tmp/jgitflow/sample/target/apidocs/allclasses-frame.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/allclasses-noframe.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/index.html...
        Generating /private/tmp/jgitflow/sample/target/apidocs/help-doc.html...
        [INFO] Building jar: /private/tmp/jgitflow/sample/target/sample-0.0.4-javadoc.jar
        [INFO] 
        [INFO] --- maven-install-plugin:2.4:install (default-install) @ sample ---
        [INFO] Installing /private/tmp/jgitflow/sample/target/sample-0.0.4.jar to /Users/nirving/.m2/repository/au/com/versent/sample/0.0.4/sample-0.0.4.jar
        [INFO] Installing /private/tmp/jgitflow/sample/pom.xml to /Users/nirving/.m2/repository/au/com/versent/sample/0.0.4/sample-0.0.4.pom
        [INFO] Installing /private/tmp/jgitflow/sample/target/sample-0.0.4-sources.jar to /Users/nirving/.m2/repository/au/com/versent/sample/0.0.4/sample-0.0.4-sources.jar
        [INFO] Installing /private/tmp/jgitflow/sample/target/sample-0.0.4-javadoc.jar to /Users/nirving/.m2/repository/au/com/versent/sample/0.0.4/sample-0.0.4-javadoc.jar
        [INFO] 
        [INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ sample ---
        Uploading: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4.jar
        Uploaded: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4.jar (2 KB at 611.7 KB/sec)
        Uploading: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4.pom
        Uploaded: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4.pom (640 B at 312.5 KB/sec)
        Downloading: file:////tmp/mvn/release/au/com/versent/sample/maven-metadata.xml
        Uploading: file:////tmp/mvn/release/au/com/versent/sample/maven-metadata.xml
        Uploaded: file:////tmp/mvn/release/au/com/versent/sample/maven-metadata.xml (299 B at 146.0 KB/sec)
        Uploading: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4-sources.jar
        Uploaded: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4-sources.jar (2 KB at 849.1 KB/sec)
        Uploading: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4-javadoc.jar
        Uploaded: file:////tmp/mvn/release/au/com/versent/sample/0.0.4/sample-0.0.4-javadoc.jar (22 KB at 4380.1 KB/sec)
        [INFO] ------------------------------------------------------------------------
        [INFO] Reactor Summary:
        [INFO] 
        [INFO] Parent ............................................. SUCCESS [  1.771 s]
        [INFO] Next Sample ........................................ SUCCESS [  3.041 s]
        [INFO] Sample ............................................. SUCCESS [  1.070 s]
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time: 6.002 s
        [INFO] Finished at: 2016-02-26T05:48:38+11:00
        [INFO] Final Memory: 22M/309M
        [INFO] ------------------------------------------------------------------------
    [INFO] (development) copying pom versions...
    [INFO] (development) updating poms for all projects...
    [INFO] turn on debug logging with -X to see exact changes
    [INFO] (development) updating pom for Parent...
    [INFO] (development) updating pom for Next Sample...
    [INFO] (development) updating pom for Sample...
    [INFO] copying pom versions...
    [INFO] (development) updating poms for all projects...
    [INFO] turn on debug logging with -X to see exact changes
    [INFO] (development) updating pom for Parent...
    [INFO] (development) updating pom for Next Sample...
    [INFO] (development) updating pom for Sample...
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary:
    [INFO] 
    [INFO] Parent ............................................. SUCCESS [  9.699 s]
    [INFO] Next Sample ........................................ SKIPPED
    [INFO] Sample ............................................. SKIPPED
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 9.924 s
    [INFO] Finished at: 2016-02-26T05:48:39+11:00
    [INFO] Final Memory: 15M/304M
    [INFO] ------------------------------------------------------------------------
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

5.  Confirm that you are back on the Development branch.

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git branch
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    development 
    master 
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

6.  Confirm that there is a release in the Maven Release Reposioty `ls -lRa
    /tmp/mvn/release` Should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    total 0
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 .
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 ..
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 au

    /tmp/mvn/release/au:
    total 0
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 .
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 ..
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 com

    /tmp/mvn/release/au/com:
    total 0
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 .
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 ..
    drwxr-xr-x  5 nirving  wheel  170 26 Feb 06:04 versent

    /tmp/mvn/release/au/com/versent:
    total 0
    drwxr-xr-x  5 nirving  wheel  170 26 Feb 06:04 .
    drwxr-xr-x  3 nirving  wheel  102 26 Feb 06:04 ..
    drwxr-xr-x  6 nirving  wheel  204 26 Feb 06:04 nextsample
    drwxr-xr-x  6 nirving  wheel  204 26 Feb 06:04 parent
    drwxr-xr-x  6 nirving  wheel  204 26 Feb 06:04 sample

    /tmp/mvn/release/au/com/versent/nextsample:
    total 24
    drwxr-xr-x   6 nirving  wheel  204 26 Feb 06:04 .
    drwxr-xr-x   5 nirving  wheel  170 26 Feb 06:04 ..
    drwxr-xr-x  14 nirving  wheel  476 26 Feb 06:04 0.0.4
    -rw-r--r--   1 nirving  wheel  304 26 Feb 06:04 maven-metadata.xml
    -rw-r--r--   1 nirving  wheel   32 26 Feb 06:04 maven-metadata.xml.md5
    -rw-r--r--   1 nirving  wheel   40 26 Feb 06:04 maven-metadata.xml.sha1

    /tmp/mvn/release/au/com/versent/nextsample/0.0.4:
    total 136
    drwxr-xr-x  14 nirving  wheel    476 26 Feb 06:04 .
    drwxr-xr-x   6 nirving  wheel    204 26 Feb 06:04 ..
    -rw-r--r--   1 nirving  wheel  22547 26 Feb 06:04 nextsample-0.0.4-javadoc.jar
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 nextsample-0.0.4-javadoc.jar.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 nextsample-0.0.4-javadoc.jar.sha1
    -rw-r--r--   1 nirving  wheel   1770 26 Feb 06:04 nextsample-0.0.4-sources.jar
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 nextsample-0.0.4-sources.jar.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 nextsample-0.0.4-sources.jar.sha1
    -rw-r--r--   1 nirving  wheel   1911 26 Feb 06:04 nextsample-0.0.4.jar
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 nextsample-0.0.4.jar.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 nextsample-0.0.4.jar.sha1
    -rw-r--r--   1 nirving  wheel    500 26 Feb 06:04 nextsample-0.0.4.pom
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 nextsample-0.0.4.pom.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 nextsample-0.0.4.pom.sha1

    /tmp/mvn/release/au/com/versent/parent:
    total 24
    drwxr-xr-x  6 nirving  wheel  204 26 Feb 06:04 .
    drwxr-xr-x  5 nirving  wheel  170 26 Feb 06:04 ..
    drwxr-xr-x  5 nirving  wheel  170 26 Feb 06:04 0.0.4
    -rw-r--r--  1 nirving  wheel  300 26 Feb 06:04 maven-metadata.xml
    -rw-r--r--  1 nirving  wheel   32 26 Feb 06:04 maven-metadata.xml.md5
    -rw-r--r--  1 nirving  wheel   40 26 Feb 06:04 maven-metadata.xml.sha1

    /tmp/mvn/release/au/com/versent/parent/0.0.4:
    total 24
    drwxr-xr-x  5 nirving  wheel   170 26 Feb 06:04 .
    drwxr-xr-x  6 nirving  wheel   204 26 Feb 06:04 ..
    -rw-r--r--  1 nirving  wheel  1871 26 Feb 06:04 parent-0.0.4.pom
    -rw-r--r--  1 nirving  wheel    32 26 Feb 06:04 parent-0.0.4.pom.md5
    -rw-r--r--  1 nirving  wheel    40 26 Feb 06:04 parent-0.0.4.pom.sha1

    /tmp/mvn/release/au/com/versent/sample:
    total 24
    drwxr-xr-x   6 nirving  wheel  204 26 Feb 06:04 .
    drwxr-xr-x   5 nirving  wheel  170 26 Feb 06:04 ..
    drwxr-xr-x  14 nirving  wheel  476 26 Feb 06:04 0.0.4
    -rw-r--r--   1 nirving  wheel  300 26 Feb 06:04 maven-metadata.xml
    -rw-r--r--   1 nirving  wheel   32 26 Feb 06:04 maven-metadata.xml.md5
    -rw-r--r--   1 nirving  wheel   40 26 Feb 06:04 maven-metadata.xml.sha1

    /tmp/mvn/release/au/com/versent/sample/0.0.4:
    total 136
    drwxr-xr-x  14 nirving  wheel    476 26 Feb 06:04 .
    drwxr-xr-x   6 nirving  wheel    204 26 Feb 06:04 ..
    -rw-r--r--   1 nirving  wheel  22417 26 Feb 06:04 sample-0.0.4-javadoc.jar
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 sample-0.0.4-javadoc.jar.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 sample-0.0.4-javadoc.jar.sha1
    -rw-r--r--   1 nirving  wheel   1737 26 Feb 06:04 sample-0.0.4-sources.jar
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 sample-0.0.4-sources.jar.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 sample-0.0.4-sources.jar.sha1
    -rw-r--r--   1 nirving  wheel   1877 26 Feb 06:04 sample-0.0.4.jar
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 sample-0.0.4.jar.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 sample-0.0.4.jar.sha1
    -rw-r--r--   1 nirving  wheel    631 26 Feb 06:04 sample-0.0.4.pom
    -rw-r--r--   1 nirving  wheel     32 26 Feb 06:04 sample-0.0.4.pom.md5
    -rw-r--r--   1 nirving  wheel     40 26 Feb 06:04 sample-0.0.4.pom.sha1
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

7.  Confirm that the version has been updated to the next Snapshot version

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    find . -name pom.xml | xargs grep 'version' 
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    ./nextsample/pom.xml:        <version>0.0.5-SNAPSHOT</version>    
    ./pom.xml:   <version>0.0.5-SNAPSHOT</version>    
    ./pom.xml:               <version>2.5.3</version>    
    ./pom.xml:               <version>1.0-m5.1</version>    
    ./sample/pom.xml:        <version>0.0.5-SNAPSHOT</version>    
    ./sample/pom.xml:            <version>0.0.5-SNAPSHOT</version>
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

8.  Cleaning up the Development git, by changing to the `dev/jgitflow` folder
    and changing to the master branch

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    cd /tmp/jgitflow/dev/jgitflow    
    git checkout master 
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    hould return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Your branch is up-to-date with 'origin/master'.
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

9.  Perform a git prune

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git fetch -p
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    From /tmp/GITRepo/jgitflow
     x [deleted]         (none)     -> origin/release-0.0.4
    remote: Counting objects: 14, done.
    remote: Compressing objects: 100% (13/13), done.
    remote: Total 14 (delta 6), reused 9 (delta 1)
    Unpacking objects: 100% (14/14), done.
       5eeec4e..a081cfb  master     -> origin/master
       3b9ce3e..9b2357a  development -> origin/development
     * [new tag]         0.0.4      -> 0.0.4
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

10. delete the `release-0.0.4` branch

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git branch -D release-0.0.4
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return similar to

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Deleted branch release-0.0.4 (was 7c77d9a).
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

11. Validate the branches are in sync

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git branch -a
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     development
    *master
     remotes/origin/development
     remotes/origin/master
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Release Candidate Builds
========================

These are built in the same way as Release Builds, but with an additional step
to a RC Number

1.  Start the release in the dev area and clean up the `development` branch.

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    cd /tmp/jgitflow/dev/jgitflow    
    git checkout development
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Switched to branch 'development'    
    Your branch is behind 'origin/development' by 8 commits, and can be fast-forwarded.  (use "git pull" to update your local branch)`
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.  Update the development branch

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git pull
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    should return

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Updating 3b9ce3e..9b2357a
    Fast-forward
     version | 1 +
     1 file changed, 1 insertion(+)
     create mode 100644 version
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3.  Start the release

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    mvn jgitflow:release-start
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

4.  Update the Version

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    echo "Version 0.0.5-RC1" > version
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

5.  Commit it

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git commit -a -m "Version 0.0.5-RC1"
    git push
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

**Note:** The following steps 6 and 7 are repeated as many times are required,
with an incremental `buildNumber`

1.  Change to the build area, clean it, clone and checkout the release branch

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    cd /tmp/jgitflow/build/    
    rm -rf *     
    git clone /tmp/GITRepo/jgitflow    
    cd jgitflow    
    git checkout -b release-0.0.5 origin/release-0.0.5
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.  Increment the Build Number and deploy to the Maven Repository

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    mvn jgitflow:build-number -DbuildNumber=1    
    mvn deploy
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3.  Finally a release is committed based on the required RC version GIT commitid

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    mvn jgitflow:release-finish -DstartCommit={SHA}
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
