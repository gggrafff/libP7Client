# libP7Client
P7 is open source and cross-platform library for high-speed sending telemetry &amp; trace data from your application with minimal usage of CPU and memory.  
Library contains exhaustive documentation inside package.  

## Features:  
    C++/C/C#/Python support  
    Cross platform (Linux x86/x64, Windows x86/x64)  
    Speed is priority, library and server are designed and optimized to suit high load, for example average performance for Intel i7-870 (more than 10 years old CPU) is  
    Traces:  
        0,5% CPU, one core: 450 000 traces per second (binary file or network)  
        100% CPU, one core: ~5.7 million traces per second to network  
        100% CPU, one core: ~10 million traces per second to binary file  
    Telemetry:  
        0,5% CPU, one core: 600 000 telemetry samples per second (binary file or network)  
        100% CPU, one core: ~6.8 million telemetry samples per second to network  
        100% CPU, one core: ~11 million telemetry samples per second to binary file  
    Small memory footprint (optional, min is 16KB) - used for embedded devices  
    Thread safe  
    Unicode support (UTF-8, UTF-32 for Linux, UTF-16 for Windows)  
    ANSI char support  
    No external dependencies  
    High-resolution time stamps (resolution depends on HW high-resolution performance counter, usually it is 100ns)  
    Different sinks (transport & storages) are supported:  
        Network (Baical server)  
        Binary File  
        Text File (Linux: UTF-8, Windows: UTF-16)  
        Console  
        Syslog (RFC 5424)  
        Auto (Baical server if reachable, else - binary file)  
        Null  
    Files rotation setting (by size or time)  
    Files max count setting (deleting old files automatically)  
    Remote management from Baical server (set verbosity per module, enable/disable telemetry counters)  
    Shared memory is used - create your trace and telemetry channels once and access it from any process module or class without passing handles  
    Crash handler or in case of user defined crash handler - special function to flush all P7 buffers for all P7 objects inside process in case of crush  
    Trace & telemetry files have compact binary format (due to speed requirements - binary files much more compact than raw text), export to text is available  
    Command line interface for configuration may be used in addition to application parameters  
    Big/Little endian support  
    Intel/AMD, ARM, MCST, PowerPC, Baikal T1, etc.  
    GCC, VC++, Clang, MinGW  


## Components  

Internally P7 has very simple design, and consist of few sub-modules:  

    Channel - named data channel, used for wrapping user data into internal P7 format. For now there are next channels types are available:  
        Telemetry  
        Trace  
    Sink - module which provides a way of delivering serialized data from channels to final destination. Next types are available for now:  
        Network - deliver data directly to Baical server  
        Binary File - writes all user data into single binary file  
        Text File - writes all user data into single text file (Linux: UTF-8, Windows: UTF-16)  
        Console - writes all user data into console  
        Syslog - writes all user data into UDP socket using Syslog format  
        Auto - delivers to Baical server if it is reachable otherwise to file  
        Null - drops all incoming data, save CPU for the hosting process  
    Client - is a core module, it aggregate sink & channels together and manage them. Every client object can handle up to 32 independent channels  


Let’s take an example (diagram below) - developed application has to write 2 independent log (trace) streams and 1 telemetry stream, and delivers them directly to Baical. Initialization sequence will be:  

    First of all you need to create P7 Client, and specify parameters for sink ”/P7.Sink=Baical /P7.Addr=127.0.0.1”  
    Using the client create:  
        first trace channel with name ”Core”  
        second trace channel named ”Module A”  
        telemetry channel named ”CPU, MEM”  

