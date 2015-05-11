# Elantech-fix
Copied code from Mateusz Jończyk's patch that fixes Elantech touch pad on Gigabyte laptops (e.g., P35G v2). https://bugzilla.kernel.org/show_bug.cgi?id=81331

Instructions:

Here are some simple steps that might help you:

(Until it's stated explicitly, you don't need root privileges to perform a given task below.)
 
1. Download kernel source code from www.kernel.org (e.g., linux-3.17.4.tar.xz, which is the most recent stable kernel release) & untar it (say in your home directory):
    tar -xJf linux-3.17.4.tar.xz

2. Download the patch (https://bugzilla.kernel.org/attachment.cgi?id=153821&action=diff&context=patch&collapsed=&headers=1&format=raw) to your home directory as say elantech-final.patch and then apply it:
    cd linux-3.17.4 (assuming this is the directory where the kernel source was untarred)
    cat ~/elantech-final.patch | patch -p1   (if there is an error message, then something went wrong)

3. You can start off with your distribution's .config as a starting point (although they tend to take a lot of time & hard drive space to compile) & customise your kernel further if need be:
    cp /boot/config-<most recent distribution kernel version> .config
    make oldconfig
    make menuconfig (optional step required only if you want to customise it further)

4. Now compile the kernel, kernel modules & install them:
    make bzImage modules  (you can speed it up by something like this: "make -j4 bzImage modules", where make is told to run 4 compiling jobs simultaneously)
    make modules_install install  (this step needs root privileges to work, as it'd be modifying your /boot, /lib file systems & boot loader etc.)

5. Optional: You may validate whether an entry has been added to the boot loader configuration file (if there was no error at the end of step 4, it generally means all has been successful):
    cat /etc/grub2-efi.cfg (where you should see a section for 3.17.4+; that's the location of the EFI compatible grub2 config file in Fedora & it needs root privileges to view; it may be slightly different in your distribution though.)

6. Reboot the laptop & boot the newly installed kernel to confirm whether it boots successfully & the trackpad works.

Good luck.
