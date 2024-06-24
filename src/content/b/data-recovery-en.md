---
title: "[EN] MacGyver style data recovery - enjoying a catastrophe"
description: "Help"
pubDate: "Jun 24 2024"
category: "linux   - "
---

## MacGyver style data recovery - enjoying a catastrophe

Imagine, if you will, making a critical mistake. One that threatens your
livelihood, your personal information, and your mental wellbeing.

Having Microsoft Windows installed.

Not long ago, there was an incident. I let Windows update itself,
and after reboot, the partition table was *gone*.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Partition Table:</strong> A data structure on a disk that describes how the disk is divided into logical sections called partitions. It tells the operating system how to access the disk's contents.
</div>

I have been putting off Windows updates for as long as I physically could,
the role of that OS on my computer was wholly secondary - there are a couple
of video games with rootkit-level, very aggressive anticheat, that I like
to sometimes play with friends. Unfortunately, I can't play those on Linux.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Rootkit:</strong> A type of malicious software designed to provide unauthorized access to a computer or network while actively hiding its presence. Rootkits can be particularly difficult to detect and remove, as they often modify the operating system to conceal their activities. In the context of anti-cheat software, "rootkit-level" implies deep system integration for comprehensive monitoring of game integrity.

There is one particularly egregious one made by a company whose name starts with *R* and ends with *iot*.
</div>

So maybe, I am *partly* to blame for everything imploding.

To make things more interesting, I was far away from home, in what is pretty much
an overgrown village, with only the following technology available to me:
- One USB drive that had the OpenBSD installer on it
- Router/Modem on the top of a shelf with only one very short ethernet cable
- My Phone
- The laptop which was now effectively dead

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">OpenBSD:</strong> A free and open-source Unix-like operating system focused on security and code correctness. It's known for its proactive security
features and is often used in firewalls, network servers, and by security professionals.

Last but not least, used by hipsters such as myself.
</div>

I couldn't get a second flash drive here, since it was weekend (Friday, which is
the *worst possible time*, naturally).

Since I couldn't flash a Linux onto the flash drive and do data recovery
the classic way, I had to *improvise* a bit.

### Huh, who's MacGyver?

If you're too young to remember, MacGyver was this 80s TV show hero who could solve
any problem with a paperclip, chewing gum, and sheer ingenuity. Kind of like what
we Linux users do when Windows inevitably blows up (many such cases - I had to enjoy
my eGPU drivers breaking about once every 1-2 months).

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">eGPU:</strong> External Graphics Processing Unit. A portable graphics card that can be connected to a laptop or desktop computer, in my case via Thunderbolt 3.
Which is good, great, but also proprietary Intel technology, and outside of Linux, Mac and Windows, I don't believe there is another operating system with drivers.
I hope that USB4 will solve this, as it should include eGPU functionality in a more open standard. It also sucked that you could not have TB3 on an AMD laptop.
</div>

MacGyver was before my time, but I did enjoy watching Richard Dean Anderson for years
and years in Stargate SG-1 and others.

But I digress. Back to our tale of woe and partition tables gone AWOL.

The OpenBSD USB was my paperclip, since it was something I could boot, and connect
to the internet.

I also found out that `testdisk` was available as a package for OpenBSD. However,
the installer is not really a LiveCD you might be used to from Linux - it cannot install
packages temporarily.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">testdisk:</strong> A powerful open-source data recovery software. It's primarily used to recover lost partitions and make non-booting disks bootable again. testdisk can:
<ul>
<li>Recover deleted partitions</li>
<li>Rebuild boot sectors</li>
<li>Recover deleted files from file systems</li>
<li>Copy files from deleted file systems</li>
</ul>
It's a valuable tool for system administrators and users facing data loss situations.
</div>

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">LiveCD:</strong> A bootable CD or USB drive containing an operating system that runs directly from the removable media without being installed on the computer's hard drive.
It's often used for system recovery or trying out new operating systems.
</div>

Guess what I had to do?

*Install OpenBSD back on the flash drive* (the installer loads into RAM, luckily).

This was not a difficult process, and I have to commend OpenBSD developers for making
the installer very user-friendly.

After installation, I had a pleasant surprise: OpenBSD is one of the few BSDs that
has drivers for the network card I put into my ThinkPad - Intel AX200.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Intel AX-200:</strong> A Wi-Fi 6 (802.11ax) and Bluetooth 5.1 wireless network adapter. It's known for its high-speed capabilities and improved performance in crowded network environments.
Support for this newer hardware can sometimes be limited in certain operating systems. I bought it for two reasons:
<ol>
<li>The Intel AC-7260 that my Thinkpad T480s shipped with was being a little sus, and I suspected it might die. My suspicion was confirmed years later (last weekend)
when I tried reinserting it, so I could have DragonFly BSD with Wifi support. Did not even show up as a device in Linux, let alone BSD. Then I also found a D-Link Wifi dongle with
a Realtek chip that I thought was really common. Wrong again, Dfly has drivers for many, not this one. There are no drivers for AX-200. Critical miss on all 3 options for wireless I had. :)</li>
<li>I read an article about Wifi 6 and I thought it was really cool, and I impulsively bought a 5k CZK router to go along with that card. It's the best router I own, great success.</li>
</ol>
</div>

This meant I could continue the rest of my recovery journey sitting.

### Installing testdisk on OpenBSD and rediscovering partitions

I am a tinkerer, so I had my main Linux split into *multiple partitions*, each with different
filesystem (EXT4, F2FS, XFS, BTRFS)

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Filesystems:</strong> Methods of organizing and storing data on storage devices. Different filesystems have various features and performance characteristics:
<ul>
<li>EXT4: Fourth extended filesystem, commonly used in Linux</li>
<li>F2FS: Flash-Friendly File System, designed for flash storage devices</li>
<li>XFS: High-performance journaling filesystem</li>
<li>BTRFS: B-tree File System, focused on fault tolerance and easy administration</li>
</ul>
</div>

I really wanted to try them all, see how they work and experiment with them. And I did, and it was great. However, between this, secondary Linux,
and Windows, I had about *16-18 partitions* on the drive. Oops, that's a lot.

Installing `testdisk` was trivial, all I had to do was

```sh
pkg_add testdisk
```

(which you may have to run with `doas`, the OpenBSD `sudo` analogue)

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">doas:</strong> A command similar to sudo, but designed to be simpler and more secure. It's the default privilege escalation tool in OpenBSD, allowing users to execute commands with different permissions.
</div>

After a couple moments, I could launch `testdisk`, and be greeted with the familiar screen:

```
TestDisk 7.2, Data Recovery Utility, February 2024
Christophe GRENIER <grenier@cgsecurity.org>
https://www.cgsecurity.org


TestDisk is free data recovery software designed to help recover lost
partitions and/or make non-booting disks bootable again when these symptoms
are caused by faulty software, certain types of viruses or human error.
It can also be used to repair some filesystem errors.

Information gathered during TestDisk use can be recorded for later
review. If you choose to create the text file, testdisk.log , it
will contain TestDisk options, technical information and various
outputs; including any folder/file names TestDisk was used to find and
list onscreen.

Use arrow keys to select, then press Enter key:
>[ Create ] Create a new log file
 [ Append ] Append information to log file
 [ No Log ] Don't record anything
```
<small>There may be [nothing sweeter than a Sunday](https://www.youtube.com/watch?v=FNTKEGVSGo8), but testdisk opening screen comes a close second</small>

TestDisk is my savior, my knight in shining armor. There was only one small issue: It wasn't compiled
with support for all the filesystems I was using. Still, I was *ecstatic*, because my home partition was `EXT4`.


### Recovering keys and other secrets from my /home partition

I knew the partition was there, and that I can recover my SSH keys (and other keys), and other secrets, so there we go,
I can run `testdisk` on the NVME SSD that's in my laptop and voilá, I should be able to get everything out.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">NVME SSD:</strong> NVMe (Non-Volatile Memory Express) is a host controller interface and storage protocol that significantly speeds up the data transfer between client systems and SSDs. NVMe SSDs offer much faster read/write speeds compared to traditional SATA SSDs.
</div>

There was one small problem:
- Even though from factory, this Thinkpad had a 256GB NVME SSD drive, I *may have made some upgrades*. What was 256GB
  was now a 4TB Western Digital WD Black drive (I had good experience with them, and I have a second, smaller WD Black drive,
  which is still running perfectly well despite suffering far more abuse)
- Going through 4 terabytes takes a *long time*

And so I had to wait, and wait. And wait. This part of the process took about 5 hours.

#### Sike

New fun problem: `testdisk` found *way more partitions* than I knew there were. My theory is that it either dug up partitions from
ages past (which it shouldn't, this drive is pretty new), or because I had bit copy images of disks stored on the SSD, it recognizes the
partitions in them.

Nevertheless, `testdisk` recognized far more partitions than it should have.

![](./macgyver/sweaty_bear.jpg)

I was worried there for a second.

But then my curiosity and proclivity to tinkering kicked in again, and I started looking at the partitions. I was able
to identify my /home partition without a shadow of a doubt, and that let me use the `testdisk` individual file
recovery feature to copy out keys, and secrets.

Thanks to that, I would be able to work again.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">SSH Keys:</strong> Secure Shell (SSH) keys are cryptographic key pairs used for secure communication and authentication in network protocols. They provide a more secure alternative to password-based logins, especially for remote system access and management.
</div>

> I keep all of my work stuff on a racked server with an AMD EPYC, so I can compile all them Rust programs
> fast enough. I access this server via SSH, and develop directly on the server (the `kakoune` editor works well enough
> over SSH)

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">AMD EPYC:</strong> A line of high-performance x86-64 microprocessors designed by AMD for server and data center use. EPYC processors are known for their high core counts, making them excellent for parallel processing tasks like compilation.
</div>

### Next steps

The fact that I found more partitions than there should be, inspired me to handle things *very carefully*.
At this point, I was also still on OpenBSD, and I wanted to return to Linux, as I knew the tools there better.

On Linux, I am able to mount a partition without a partition table like this:

```sh
sudo mount -t ext4 -o ro,offset=<offset of partition> /dev/nvme0n1 /mnt/mypartition
```
<small><small>mind the units - Bytes != Kilobytes != Blocks</small></small>

You find the offset via `fdisk` (on properly working disks), or in our case, with `testdisk`.

The process on OpenBSD is a bit more involved:

```sh
vnconfig vnd0 /dev/rsd0c -o <offset>
mount -t ext2fs -o ro /dev/vnd0c /mnt/mypartition
```

First, we need to declare a virtual disk device at the correct offset, then we can mount it.

I had the option of reconstructing *a* partition table via `testdisk`. However, that would run the risk of
restoring an incorrect partition table, and I wanted to be *very careful* about that.

This led me to formulate another plan:
1. Create an image of the whole 4TB disk
2. Upload the image to my server machine, which has an additional, completely empty, 10TB disk
3. Pull out the partitions one by one, and look in them

The first part is the easiest:

```sh
dd if=/dev/nvme0n1 of=/somewhere/disk.img status=progress bs=1M
```

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">dd:</strong> A command-line utility for Unix and Unix-like operating systems. Its primary purpose is to convert and copy files, but it can also be used to create disk images. The name "dd" allegedly comes from "data definition", a JCL statement.
</div>

Armed with offsets and sizes from `testdisk`, doing the last part is very easy.

You can just use `dd` again:

```sh
dd if=original_disk_image.img of=extracted_partition.img bs=1 skip=$OFFSET count=$SIZE
```
<small>on Linux, you can add `status=progress` for flavor. If your partition size and offset are in blocks, don't use `bs=1`</small>

A lot of people surprisingly don't know, that `dd` is essentially just a smarter `cp`, and that
Linux really does follow the "everything is a file" mantra. So dd can take files and devices as either parameter,
and it can also take input from stdin (if you don't set `if=`) or output to stdout (if you don't set `of=`).

These are the true two challenges I needed to solve:
- Where do I store the image? It will have 4TB
- How do I transfer it over network effectively - the disk was fairly new, and definitely there would be tons of zeroes, which would
  compress really well
  - My internet is not the fastest here

To tackle the first problem, I have decided to order a flash drive (so I could get Linux running) and a 5TB external hard drive.
Those would come on Monday, so in the meantime, I went to touch grass:

![](./macgyver/grass.png)
<small>look at it, midjourney generated the correct amount of fingers, for once!</small>

I did play around with OpenBSD a little. One thing that I found particularly interesting is `pledge()`. It is
a security measure that allows a process to voluntarily restrict the resources it uses, you are essentially *pledging you will
only use what you say*.

If you don't, the kernel will shoot you in the head.

This is similar to Linux's **seccomp**, but is much simpler to use.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">seccomp:</strong> Short for "secure computing mode", it's a computer security facility in the Linux kernel. seccomp allows a process to make a one-way transition into a "secure" state where it cannot
make any system calls except the ones allowed.
</div>

Consider `pledge()`:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    // Pledge to only use stdio and rpath (for file reading)
    if (pledge("stdio rpath", NULL) == -1) {
        perror("pledge");
        return 1;
    }

    FILE *file = fopen("/etc/hosts", "r");
    if (file == NULL) {
        perror("Error opening file");
        return 1;
    }

    char buffer[256];
    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        printf("%s", buffer);
    }

    fclose(file);
    return 0;
}
```

versus a roughly equivalent `seccomp`:

```c
#include <stdio.h>
#include <stdlib.h>
#include <seccomp.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    scmp_filter_ctx ctx;

    ctx = seccomp_init(SCMP_ACT_KILL); // Kill process if it makes a non-allowed syscall

    // Allow necessary system calls
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(read), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(write), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(open), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(close), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(fstat), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(exit_group), 0);

    if (seccomp_load(ctx) < 0) {
        perror("seccomp_load");
        return 1;
    }

    FILE *file = fopen("/etc/hosts", "r");
    if (file == NULL) {
        perror("Error opening file");
        return 1;
    }

    char buffer[256];
    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        printf("%s", buffer);
    }

    fclose(file);
    return 0;
}
```

Note that to be fair, we have to admit that seccomp is a very granular tool for limiting syscalls, and
it is very configurable. Just that the cost of entry is significantly higher than `pledge()`, and so
most devs do not use it.

### Conclusion

Before I made the 4TB image copy, I downloaded an iso of [Void Linux](https://voidlinux.org/), flashed it to the second
flash drive, and switched to Linux. I would be running from the live environment for a couple days.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Void Linux:</strong> An independent Linux distribution that uses the XBPS package manager and runit init system. It's known for its rolling release model, minimalism, and focus on stability.
</div>

Creating the 4TB image on the external HDD took almost 3 days of pure time. I used the fastest USB port
I had, and I formatted the HDD as `ext4` (I need a file system that can handle large files, and was as universal as possible and NOT NTFS).
Three days was not that bad of a time.

Once that was done, I had an HDD with an image of my original disk, and I could do two things:
- Work with the image
- Install things on my drive again

And so I am back on Linux. Artix, with S6 init and `linux-cachyos-bore` kernel.

In the second part of my quest, we will be writing a Rust program to resumably transfer the 4TB file with zstandard compression.


