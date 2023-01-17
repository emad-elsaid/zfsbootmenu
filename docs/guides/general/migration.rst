Migrating to ZFSBootMenu
========================

Historically, a root-on-ZFS Linux installation is broken up into two parts, a ZFS pool for the distribution and a separate filesystem explicitly for `/boot`. A few common varations are outlined below.

Two ZFS pools
~~~~~~~~~~~~~

Commonly used with Ubuntu systems, the system is laid out with two distinct ZFS pools. On a UEFI system, an ESP is still needed to hold the first stage EFI bootloader.

* ``rpool`` to hold the OS and user data, with full feature-flag support.
* ``bpool`` to hold ``/boot``, with only the features supported by GRUB enabled.
* ``ESP``   to hold ``/boot/efi``.

The EFI System Partition (ESP) on these systems might be restrictively small.

One ZFS pool, ``/boot`` on the ESP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This variation uses a single ZFS pool and a larger ESP:

* ``rpool`` to hold the OS and user data, with full feature-flag support.
* ``/boot`` uses the entire ESP.

One ZFS pool, ``/boot`` on EXT4 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This variation uses a single ZFS pool and a filesystem that GRUB or syslinux is able to read, commonly EXT4 or XFS. On a UEFI system, an ESP is still needed to hold the first stage EFI bootloader.

* ``rpool`` to hold the OS and user data, with full feature-flag support.
* ``/boot`` on a dedicated partition, backed by EXT4, XFS or some other in-kernel filesystem.
* ``ESP``   to hold ``/boot/efi``.

..
  vim: softtabstop=2 shiftwidth=2 textwidth=120
