# View Free Disk Space and Disk Usage From the Linux Terminal

The df and du commands report on disk space usage from within the Bash shell used on Linux, macOS, and many other Unix-like operating systems. These commands let you easily identify what's using up your system's storage.

## View the Total, Available, and Used Disk Space on Linux

Bash contains two useful commands related to disk space. To find out the available and used disk space, use df (disk filesystems, sometimes called disk free). To discover what's taking up the used disk space, use du (disk usage).

Type df and press enter in a Bash terminal window to get started. You'll see a lot of output similar to the screenshot below. Using df without any options will display the available and used space for all mounted filesystems. At first glance, it might look impenetrable, but it is quite easy to understand.

```bash
df
```

![image_85.png](image_85.png)

Each line of the display is made up of six columns.

+ Fileystem: The name of this filesystem.
+ 1K-Blocks: The number of 1K blocks that are available on this filesystem.
+ Used: The number of 1K blocks that have been used on this file system.
+ Available: The number of 1K blocks that are unused on this file system.
+ Use%: The amount of space used in this file system given as a percentage.
+ File: The filesystem name, if specified on the command line.
+ Mounted on: The mount point of the filesystem.

You can replace the 1K block counts with more useful output by using the -B (block size) option. To use this option, type df, a space, and then -B and a letter from the list of K, M, G, T, P, E, Z or Y. These letters represent the kilo, mega, giga, tera, peta, exa, zeta, and yotta values from the multiple of 1024 scale.

For example, to see the disk usage figures in megabytes, you would use the following command. Note there is no space between the B and M.

```bash
df -BM
```

![image_86.png](image_86.png)

The -h (human readable) option instructs df to use the most applicable unit for the size of each filesystem. In the next output note that there are filesystems with gigabyte, megabyte and even kilobyte sizes.

```bash
df -h
```

![image_87.png](image_87.png)

If you need to see the information represented in numbers of inodes, use the -i (inodes) option. An inode is a data structure used by Linux filesystems to describe files and to store metadata about them. On Linux, inodes hold data such as the name, modification date, position on the hard drive, and so on for each file and directory. This isn't going to be useful to the majority of people, but system administrators must sometimes refer to this type of information.

```bash
df -i
```

![image_88.png](image_88.png)

Unless told not to, df will provide information on all of the mounted file systems. This can lead to a cluttered display with a lot of output. For example, the /dev/loop entries in the lists are pseudo file systems that allow a file to be mounted as though it were a partition. If you use the new Ubuntu snap method of installing applications, you can acquire a lot of these. The space available on these will always be 0 because they aren't really a filesystem, so we don't need to see them.

We can tell df to exclude filesystems of a specific type. To do so, we need to know what type of filesystem we wish to exclude. The -T (print-type) option will give us that information. It instructs df to include the type of filesystem in the output.

```bash
df -T
```

![image_89.png](image_89.png)

The `/dev/loop` entries are all squashfs filesystems. We can exclude them with the following command:

```bash
df -x squashfs
```

![image_90.png](image_90.png)

That gives us a more manageable output. To get a total, we can add the --total option.

```bash
df -x squashfs --total
```

![image_91.png](image_91.png)

We can ask df to only include filesystems of a particular type, by using the -t (type) option.

```bash
df -t ext4
```

![image_92.png](image_92.png)

If we want to see the sizes for a set of filesystems, we can specify them by name. Drive names in Linux are alphabetical. The first drive is called `/dev/sda`, the second drive is `/dev/sdb`, and so on. Partitions are numbered. So `/dev/sda1` is the first partition on drive `/dev/sda`. We tell df to return information on a particular filesystem by passing the name of the filesystem as a command parameter. Let's look at the first partition of the first hard drive.

```bash
df /dev/sda1
```

![image_93.png](image_93.png)

Note that you can use wildcards in the filesystem name, where * represents any set of characters and ? represents any single character. So to look at all partitions on the first drive, we could use:

```bash
df /dev/sda*
```

We can ask df to report on a set of named filesystems. He we are requesting the sizes of the `/dev` and `/run` filesystems, and we'd like a total.

```bash
df -h --total /dev /run
```

![image_94.png](image_94.png)

To further customize the display, we can tell df which columns to include. To do so use the --output option and provide a comma-separated list of the required column names. Make sure not to include any spaces in the comma separated list.

+ source: The name of the filesystem.
+ fstype: The type of the filesystem.
+ itotal: The size of the filesystem in inodes.
+ iused: The space used on the filesystem in inodes. 
+ iavail: The available space on the filesystem in inodes. 
+ ipcent: The percentage of used space on the filesystem in inodes, as a percentage. 
+ size: The size of the filesystem, by default in 1K blocks. 
+ used: The space used on the filesystem, by default in 1K blocks. 
+ avail: The available space on the filesystem, by default in 1K blocks. 
+ pcent: The percentage of used space on the filesystem in inodes, by default in 1K blocks. 
+ file: The filesystem name if specified on the command line. 
+ target: The mount point for the filesystem.

Let's ask df to report on the first partition on the first drive, with human readable numbers, and with the columns source, fstype, size, used, avail, and pcent:

```bash
df -h /dev/sda1 --output=source,fstype,size,used,avail,pcent
```

![image_95.png](image_95.png)

Long commands are perfect candidates to be turned into an alias. We can create an alias dfc (for df custom ) by typing the following and pressing Enter:

```bash
alias dfc="df -h /dev/sda1 --output=source,fstype,size,used,avail,pcent"
```

![image_96.png](image_96.png)

Typing dfc and pressing enter will have the same effect as typing in the long command. To make this alias permanent add it to your .bashrc or .bash_aliasesfile.

We've been looking at ways to refine the output from df so that the information it displays matches your requirements. If you want to take the opposite approach and have df return all the information it possibly can use the -a (all) option and the --output option as shown below. The -a (all) option asks df to include every filesystem, and using the --output option without a comma-separated list of columns causes df to include every column.

```bash
df -a --output
```

![image_97.png](image_97.png)

Piping the output from df through the less command is a convenient way to review the large amount of output this can produce.

```bash
df -a --output | less
```

## Find Out What's Taking Up the Used Disk Space
Let's do some investigation and find out what's taking up space on this PC. We'll start with one of our df commands.

```bash
df -h -t ext4
```

![image_98.png](image_98.png)

There is 78% disk space used on the first partition of the first hard drive. We can use the du command to show which folders are holding the most data. Issuing the du command with no options will display a list of all directories and sub-directories below the directory the du command was issued in. If you do this from your home folder the listing will be very long.

```bash
du
```

![image_99.png](image_99.png)

The output format is very simple. Each line shows the size and name of a directory. By default, the size is shown in 1K blocks. To force du to use a different block size, use the -B (block size) option. To use this option type du, a space, and then -B and a letter from the list of K, M, G, T, P, E, Z, and Y, as we did above for df . To use 1M blocks, use this command:

```bash
du -BM
```

![image_100.png](image_100.png)

Just like df, du has a human-readable option, -h, which uses a range of block sizes according to the size of each directory.

```bash
du -h
```

![image_101.png](image_101.png)

The -s (summarize) option gives a total for each directory without displaying the sub-directories within each directory. The following command asks du to return information in summary format, in human readable numbers, for all directories (*) below the current working directory.

```bash
du -h -s *
```

![image_102.png](image_102.png)

The Picture folder holds the most data by far. We can ask du to sort the folders in size from largest to smallest.

```bash
du -sm Pictures/* | sort -nr
```

![image_103.png](image_103.png)

By refining the information returned by df and du it is easy to find out how much hard disk space is in use, and to discover what is taking up that space. You can then make an informed decision about moving some data to other storage, adding another hard drive to your computer or deleting redundant data.

These commands have a lot of options. We described the most useful options here, but you can see a complete listing of the options for the df command and for the du command in the Linux man pages.

**Bibliography:** [https://www.howtogeek.com/409611/how-to-view-free-disk-space-and-disk-usage-from-the-linux-terminal/](https://www.howtogeek.com/409611/how-to-view-free-disk-space-and-disk-usage-from-the-linux-terminal/)