## How are core dumps created?
To understand this flow from an application developers point of view you need to recall that a when a process starts the operating system creates a wrapper around the application code to provide consistent management on top of the program. You don't have to explicitly handle the core dump in application code.

![core dump process](/images/core-dump-process.png)

1.  Typically a kernel event occours and the kernel [notifies the process of this event by signal](https://github.com/torvalds/linux/blob/b4f633221f0aeac102e463a4be46a643b2e3b819/kernel/signal.c#L2733). 

2. The process wrapper handles the signal usually through [coredump action](https://github.com/torvalds/linux/blob/18bf34080c4c3beb6699181986cc97dd712498fe/fs/coredump.c#L567). 

3. In order to create the file the kernel traverses all the Virtual Memory Areas that belongs to the process and generates the contents to an ELF format. 

4. If the core_pattern starts with a | the ELF file is [piped](https://github.com/torvalds/linux/blob/18bf34080c4c3beb6699181986cc97dd712498fe/fs/coredump.c#L627) to a downstream process. 

5. Else the core is [sent to a file](https://github.com/torvalds/linux/blob/18bf34080c4c3beb6699181986cc97dd712498fe/fs/coredump.c#L696)