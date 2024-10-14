---
title: "OpenText Output Transformation Server (OTS): memory configuration"
header:
  image: /images/2024-10-14-ots-memory-configuration/01-Output-Transformation-Designer.png
  og_image: /images/2024-10-14-ots-memory-configuration/01-Output-Transformation-Designer.png
tags:
  - OpenText
  - Output Transformation Server
  - OTS
last_modified_at: 2024-10-14T08:34:18-04:00
---

As multiple versions of **OpenText Output Transformation Server (OTS)** can be installed at any given time, 
you can use the `<OTS_home>\settings\startup.properties` file to control which version is used 
upon starting the application.


## Java Virtual Machine's -Xms and -Xmx parameters I 

The `-Xms` and `-Xmx` parameters are used to control the initial and maximum heap size allocated 
to the Java Virtual Machine (JVM). This is the amount of memory in bytes that will be used for 
creating and storing objects. Values for these parameters can be specified as bytes or in the 
following larger units:

 - `k` for `kilobytes` (e.g. `–Xms104k`)
 - `m` for `megabytes` (e.g. `–Xms512m`)
 - `g` for `gigabytes` (e.g. `–Xms4g`)

The value specified in -Xms will be allocated to the heap when the program starts. For example, `-Xms512m` will 
allocate `512 megabytes` to the heap. The heap is not a fixed size: if more memory is needed, it can grow, up to 
a maximum of the size specified in `-Xmx`. So, for example, `-Xmx4g` will set the maximum heap size to `4 gigabyte`. 
When both of these example parameter values are used together, i.e. –Xms512m –Xmx4g, the size of the heap will 
initially be `512 megabytes` but will be allowed to grow to `4 gigabyte` during the program’s lifecycle.

The amounts provided to each parameter will depend on the resources of the system running the application and 
the application’s memory requirements. It is therefore difficult to provide a generic suggestion for setting 
these parameters. Keep the following points in mind:

 - Setting the initial heap size too low could slow down the application’s operations, as the JVM will spend 
   extra time increasing the heap size. But setting it too high may waste system resources if the application’s 
   memory use is far below the allocation.
 - Setting the maximum heap size too low may cause the application to run out of memory during normal operations, 
   and throw this exception: java.lang.OutOfMemoryError: Java heap space. However, setting it too high may cause 
   the application to interfere with other applications on the system.

If `-Xms` and `-Xmx` are not explicitly set, the JVM will set the initial heap size based on the amount of available 
physical memory on the system and the maximum heap size based on the total amount of physical memory on the system. 
The exact details of these default settings will depend on the JVM implementation in use and whether it is in client 
or server mode.

## startup.properties

If you are using `startDesigner.bat \ .sh` to launch **Output Transformation Designer**, open the `startup.properties` 
file for editing and locate the designer.version property; this property specifies the version to use when launching 
the application and you can enter an asterisk (*) as the value if you wish to always use the newest installed version.

If you are using `startEngine.bat \ .sh` to launch a **standalone instance of the engine**, open the startup.properties 
file for editing and locate the engine.version property; this property specifies the version to use when launching the 
engine and you can enter an asterisk (*) as the value if you wish to always use the newest installed version.

`startup.properties` includes  a section to configure heap size used by **Designer**:

```properties
#####################################
###
### Controls how the product runs in Designer mode
###
### Use startup/startDesigner.bat or .sh to run
designer.jvmargs=-Xmx800M -Xms300M
designer.version=24.2.00_11801
designer.name=designer
```

`startup.properties` also includes  a section to configure heap size used by **Engine**:

```properties
#####################################
###
### Controls how the product runs in Stand-alone engine mode
###
### Use startup/startEngine.bat or .sh to run
###
### Note: Web interfaces and many components will not be active in this mode.
###       Good for simple FileEvents runing transform processes
###
### To allow multiple instances to run, specify the unique name for
### variables engine_2.name, engine_3.name, etc.
###
### You can specify different jvmargs and version vars too.  If not specifed,
### the same settings for the first engine are used.
###
engine.jvmargs=-Xmx800M -Xms300M
engine.version=24.2.00_11801
engine.name=engine
```

So, to modify the maximum and minimum heap size you just need to modifify the property 
`designer.jvmargs` for **Designer**, or `engine.jvmargs` for **Engine**. i.e. to set 
a maximum of 4 Gb for Designer and 6 Gb for Engine:

```properties
designer.jvmargs=-Xmx4G -Xms300M
designer.version=24.2.00_11801
designer.name=designer
#…
engine.jvmargs=-Xmx6G -Xms300M
engine.version=24.2.00_11801
engine.name=engine
```


