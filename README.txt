README for nmonchart 39 by Nigel Griffiths
==========================================

The Korn shell script file nmonchart transforms .nmon performance capture files in to .html files for a webserver site.

If your .nmon is called hostname_date_time.nmon  and the website pages are at /webpages/docs then use it like this:

	./nmonchart hostname_date_time.nmon /webpages/docs/hostname_date_time.html

Then nmonchart Korn shell script needs the Korn shell installed.
- it has NOT been tested on Csh or bash.
AIX and Linux have Korn Shell called kshi - if your like does not have it you should install it.

The script works out if it is running on AIX or Linux and makes two changes based on that.
- see the top 10 lines for this.

nmonchart is to complete - it is fairly simple Korn shell, grep, sed and awk.
For data that is in fixed format like physical CPU, run queue and memory stats in the nmon output 
it is simple to pick out the column and graph it.
For data that is not fixed format as it depends on the resources like disk, network, CPUs it is more complex
as it has to workout the number of header line (the resource names) and data lines.
The graphs like TOPSUM and TOPCMD are quite tricky. 

For testing I have supplied a sample nmon file called: sampleC.nmon and a sample output file called: sampleC.html
which was generated using the following command: ./nmonchart sampleC.nmon sampleC.html

Examine the sample.html to find out how the data and graphs are generated.
- It is using Javascript
- The Googlechart library (directly from the web) is used to generate the graphs
- - find more information here https://developers.google.com/chart/
- - it has lots of functions and pretty graphs
- then for each graphs
- - There is the data in an array format and the ZZZ data transformed in to a java dat format
- - Then the chart drawing options and graph instructions
- At the bottom it creates the Javascript buttons and then the config data that is displayed at the bottom of the webpage.

----
Output file size

Also note nmonchart output files are typically much smaller than the original nmon file.
Something like 20% of the size.
This is unlike the nmon Analyser file output which can typically be twice the size.

----
New Features from nmonchart version 40
- Support for VIOS SEA, VIOS SEA Packet and VIOS SEA Phyiscal Network
- This requires capturing with the nmon -O option on the VIOS
- These and FibreChannel (FC) stats buttons go on a new line - if found in the .nmon file.
- Change the threshold from 5.0 to 0.05 on CPU_USE graphs.
----
New Features from nmonchart version 39
- Support for AIX 7.2 TL4+ CPUMHZ state if you add the -y dfreq=on flag on POWER9+
----
New Features from nmonchart version 38 
- Added Disk Transfer Stacked graph 
- Changes Zoom in/out wording
----
New Features from nmonchart version 37 
- Correction to the Paging rate graph title to
   "All Paging (pgin & pgout) & Paging from Paging Space only (pgsin & pgsout) per second"
   - thanks to Alan Wilcox for reporting this confusion
- Change Logical CPU USE (SMT) to ignore values below 5%
   - the highlight real use of the SMT threads + new title
   - ignores SMT just ticking over
   - change graph title to explian this
    Average Use of Logical CPU Core Threads - POWER=SMT or x86=Hyperthreads (ignoring values below 5%)
- Typo transers to transfers
----
New Features from nmonchart version 35 - thanks to Klaus Franke for the speed up code snippet
- Good speed up in performance - specially with large numbers of snapshots (i.e. many thousands)
- Added four AIX Fibre Channel Graphs by request. Note you need to run nmon with the -^ to include FC stats.
  FC READ & WRITE KB/s and FC TRANSFERS IN and OUT/s
- Removed the need for the 2nd parameter - it uses the 1st parameter & will replace the .nmon with .html
  If not ending in .nmon it just added the .html anyway.
----
New Features from nmonchart version 34 - thanks to Enrico Joedecke for the fix and suggestions
- Fix to TopDisks by adding the paste command -d, option
- Added to Linux nmon files from PowerVM (POWER hypervisor) PhysicalCPU and Shared CPU Pool Free graphs
- For Linux the RunQueue graph incldes Blocked processes
- JFS data "nan" short for "not a number" (usually not a real filesystem) replaced with -1.234 so graphs work
----
New Features from nmonchart version 33
- Added the Top 15 Disks graph - button on the top line
- Top disks are determined by the sum of all the Busy percentages for each disk
- Short lived peaks to high percentages may not appear as a busy disk
- This top 15 disks can help when the nmon file has many tens to houndred or thousands of disks.
----
New Features from nmonchart version 32
- Added the AIX MemUse graph for Process%, File system cache%, System%, Free%
- Added handling of Linux GPU, if there is just one GPU
----
New Features from nmonchart version 31
- Fix for Top Processes sort arguments for Linux which handles -t, differently.
- Fix Linux machines with no disks reported in AAA,disks,0 line - we skip graphing the disks
- Added sampleD.nmon and sampleD.html as a much larger & more interesting set of nmon stats 
- Note: sampleC had some stats faked up my me!
----
New Features from nmonchart version 30
- work around differences of echo and print for AIX and Linux - see PRINTN in the script.
----
New Features from nmonchart version 29
Zoom In and Out added  - thanks to Ivan Goncharov for the hint
- Left-click and drag the mouse to highlight an area to Zoom then when you let go it will Zoom In
- Right-click to Reset and see the whole graph again
----
New Features from nmonchart version 28
1) Added PROCCOUNT (count of processes) as this was release via an AIXpert Blog about nmon Extermal Data Collectors
2) Added MORE1 and MORE3 examples for people adding their own stats.
3) Buttons for adapter typo fixed
----
New Features from nmonchart version 27
The buttons labels are upper and lowercase = look nicer 
Added the nmon Configuration button to pop-up a window with the AAA and BBB data 
Unfortunately, this highlights browser behaving differently 
- Firefox no scroll bars so use PageDown 
- Chrome has scroll bars 
- Internet Explorer I get a blank windows but it works for other people 
The Top Process Summary and Top Commands Buttons moved up at the top line to save space 
If you do not want the Config button (it does add to the .html size) set wantCONFIG=0 near the top of the nmonchart script - =1 means we want it.
----
New Features from nmonchart version 27
1) Better Configurtion button support
2) Top  Commands bubble diagram and graph move to the top line
3) Graph title improvements and clarification
4) Buttons in mixed case for readability
5) Link to nmonchart download and information website
----
New Features from nmonchart version 26
- not released
----
New Features from nmonchart version 25
Added the following AIX Disk Service time stats which are collected if you add the nmon -d option
In the nmon file these lines start with DISKSERV DISKREADSERV DISKWRITESERV DISKWAIT
- Disk Service time in milli-seconds
- Disk Read Service time in milli-seconds
- Disk Write Service time in milli-seconds
- Disk Wait  time in milli-seconds

New Features from nmonchart version 24
1) Disk Groups
Now supported User Defined Disks Groups data.  nmon -g filename   or nmon -g auto
- This is particularly good for nmon for Linux ae -g auto will remove all the disk + partition duplication
- In the data frile look for likes starting like DGBUSY etc.
- Also very good to group your disks like database, backup, dump, or rdbms_log, rdbms_data
- New Graphs
    DGBUSY - Disk Group busy percentage for each disk - Stacked lines
    DGBUSYu - Disk Group busy percentage for each disk - Unstacked lines
    DGREAD - Disk Group read throughput in KBytes per second for each disk - Stacked lines
    DGREADu - Disk Group read throughput in KBytes per second for each disk - Unstacked lines
    DGWRITE - Disk Group write throughput in KBytes per second for each disk - Stacked lines
    DGWRITEu - Disk Group write throughput in KBytes per second for each disk - Unstacked lines
    DGSIZE - Disk Group block sizes
    DGXTER - Disk Group Transfers per second

2) S822LC can have NVidia GPU adapters
- A special nmon version 16c to capture, GPU stats: GPU Util, Memory Util, Temperature, Watts and GPU MHz
- In the data look for lines starting with GPU_
- Note each adapters has two GPUs so a total of four
- New Graphs
    GPU_UTIL - NVidea GPU Stats, GPU CPU Utilisation percent
    GPU_MEM - NVidea GPU Stats, GPU Memory Utilisation percent
    GPU_TEMP - NVidea GPU Stats, Temperature in degrees Centigrade
    GPU_WATTS - NVidea GPU Stats, input electrical power in Watts
    GPU_MHZ - NVidea GPU Stats, GPU Mhz

3) Linux Utilisation stats now ten of them
- Linux now has ten utuilisaiton stats: user, nice, system, ide, wait for IO, steal, irq, soft irq, guest and guert nice
- nmon for LInux 16c onwards captures these
- Note a CPU core thread is reported as 100% otherwise machines with 160 only ever reports tiny percentages.
- New Graph
    CPUUTIL_ALL
    Not included the data for each CPU core thread - my machines have up to 160!!!

----
Summary of the graphs

    PHYSICAL_CPU - PhysicalCPU, VirtualCPU and entitlement (AIX only LPAR stats))
    POOLIDLE - If switched on at the LPAR level PoolIdle and Pool CPU count (AIX only
    CPU_UTILisation - User%, System%, Wait% and Idle%
    CPU_USE - Logical CPU Core Use (Power SMT or x86 Hyperthreads) Average(User%+System%)
    RUNQ - Run Queue in number of processes
    PSWITCH - Process Switches as the kernel rns different programs
    SYSCALL - Systems calls of processes requesting Kernel operations - Total and read, write calls
    READWRITE - Read and Write System calls only
    FORKEXEC - Systems call fork (duplicate a process) and exec (overwrite current process with a new program)
    FILEIO - System call - number of bytes on the read + write system call - includes disks, networt sockets and pipes
    REALMEM - Total RAM (MB) and Free RAM (MB) (AIX only)
    VIRTMEM - Virtual memory (paging space) Total (MB) and Free (MB) (AIX only)
    MEM_LINUX - Total RAM, Free RAM (MB), and other Linux memory stats (Linux Only)
    SWAP_LINUX - Swap size (MB) and Swap Free (MB (Linux only)
    FSCACHE - Filesystem Cache (numperm) size in percent with minperm% and maxperm%
    PAGING - Paging space: pages in (pgin) and out (pgout) plus Filesystem paging: in (pgsin) and out (psout)
    SWAPIN - Process swap back in to memory per second
    TOPSUM - If your nmon file includes TOP process (nmon -t or nmon -T) - Bubble diagram of top process by total CPU cycles, total I/O KB and max Memory size
	- horizontal axis = CPU cycles in total
	- vertical axis the I/O generated this could be network, disk, pipes, sockets
	- size of the bubble is the memory size
    TOPCMD - If your nmon file includes TOP process (nmon -t or nmon -T) - top 15 commands nd their CPU use over time. 

    NET - Network throughput read and write for each network in KByes per second
    NETPACKET - Numbers of read and write packets per second for each network
    NETSIZE - The average number of bytes in each packet for each network read and write
    ADAPT_KPS - Throughput in KBytes per second read and write for each disk adapter
    ADAPT_TPS - Transactions per second read and write for each disk adapter
    DISKBUSY - Disk busy percentage for each disk - Stacked lines
    DISKBUSYu - Disk busy percentage for each disk - Unstacked lines
    DISKREAD - Disk read throughput in KBytes per second for each disk - Stacked lines
    DISKREADu - Disk read throughput in KBytes per second for each disk - Unstacked lines
    DISKWRITE - Disk write throughput in KBytes per second for each disk - Stacked lines
    DISKWRITEu - Disk write throughput in KBytes per second for each disk - Unstacked lines
    DISKBSIZE - Disk block sizes
    DISKXTER - Disk Transfers per second
    JFS - Journaled Filesystem Percent Full
    IPC - Interprocess Communication meaning Semaphores and messages queues. 

----
Graphs not supported
    More than 150 Disks - You have the adapter view for overall Disk stats. Data files with crazy numbers of disks in the thousands are just impossible or graph or manage (IMHO). The first 150 plus the adapter totals is a good compromise.
    Disk service times - see above. Perhaps we need a different set of graphs just for the disk junkies!! Personally, we should get the disk subsystems to do the I/O spreading work and hid a disk mess from the UNIX / Linux Sys admin team.

----
Graphs that are not going to happen and why
	Individual Logical CPU Utilisation (up to 1536 with the new E880 with 192 cores)
        Mostly pointless and misleading - they are timesharing the physical CPU cores.
        Better to study the VM LPAR physical CPU use and UTIL graphs. 

----
Adding new graphs
- I am interested in hearing you ideas on new graphs you would like to have.
- They need to add value for most nmon users
- Adding graphs from the nmon file is pretty easy as we have already worked though the issues of most formatting options
- So it is a cut'n'paste rename the graph and make minor formatting changes for the column, don't forget the button line 
at the bottom.

- Yet more disks graphs for stupidly high numbers of disks is not a good idea. We could give an option to fine the top 20 over used disk names!

----
nmonchart created webpages or Graph failures

If you transform the nmon data but the webpage does not work -  what should you do?
1 Feel free to send me the original nmon file to investigate
2 Mostly, the webpage will probably display OK but the buttons will not work
3 Javascript and Google charts are rather fussy in the syntax.
4 Sometimes the first few graphs work but later in the page ones will not do anything - this is a good indicator of where the syntax issue is in the file.
5 If something like the number of disks changes during the collecting of data then you will find the disk buttons and later buttons will not work.  The nmon data collect does NOT handle this by design. This is to reduce nmon CPU time by a large amount.
6  you could go looking at the .html file looking for oddly formatted lines - I have tried to indent the Java script code to make this possible.
7 If you work a fix please email the original and the fixed nmonchart scripts.

----
Testing of nmonchart

Using the internal to IBM website we had more than 200 nmon files to test.
This includes
Current AIX 6 and 7
Back dated AIX 6 and 7 including some with nmon errors of 5 years ago
Old AIX 5 files - mostly to check utilisation before upgrading to POWER8

Current Linux on Power from SUSE, Ubuntu and Red Hat
Current Linux on x86 and x86_64 from SUSE, Ubuntu and Red Hat and also other hardware like ARM.
Other Linux distro' and some older releases too.
I would like more examples from Mainframe and Linux Distro's like Fedora OpenSUSE, Debian etc.
and collected by nmon 15 from within a VM especially overworked machines showing CPU Steal utilisation time.

On strange, rare or older nmon files a lot of work has gone in to support oddly formatted data
- nmonchart is now pretty robust ... unless you know differently

----
Upload and Generate Graphs Website
==================================
This is just a starter for ten - you will be required to do significant work.
You will have to know HTML, scripting and a little PHP.
I am not offering hand holding support to get your working.

I set up a crude upload your nmon file webpage and a cron job to create the .html files and a further script
to generate a webpage to list the resulting .html files listed by hostname and then the date and listing the OS.

- nmon_upload.html - the upload webpage
- nmon_upload.php - the PHP that actually uploads the file
- nmonchart_cron - the cron job that runs nmonchart on new uploads and generated the webpage of hosts and graphs.

This is provided "as is" but comes with loads of assumptions in the code:
	My apache website /webpages/docs
	the resulting .html are placed here /webpages/docs/nmonchart
	the cron generate index.html of graphs here /webpages/docs/nmonchart/index.html
	the uploads end up here /webpages/docs/nmon_upload
	the final place for the .nmon files /home/nag/nmoncharttmp
I may try to clean these up and make them generic.

I would actually like some help doing a much better job and the above has a 8 MB limit and is slow and
I would like a multiple file upload system.  
The 60 second cron job is also crude.
The index.html needs to automatically refresh once a minute or so.

I actually have a budget to run a WWW upload website to help those not interested in setting up their own.
- expect more information as soon as I can set this up.

Cheers Nigel Griffiths 
nigelargriffiths@hotmail.com
Twitter @mr_nmon

