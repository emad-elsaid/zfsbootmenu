Frequently Asked Questions
==========================

Change Font Size
----------------

.. tabs::

  .. group-tab:: Prebuilt

    ZFSBootMenu binary releases are built on top of Dracut. ``rd.vconsole.font`` controls the font face and size. Terminus fonts are included in the release and recovery images.  

    ``ter-v32b`` is suitable for high DPI (4k / UHD) screens. 

  .. group-tab:: Dracut

    ``rd.vconsole.font`` controls the font face and size. Confirm font availablility in your generated ZFSBootMenu initramfs/EFI bundle.

  .. group-tab:: initcpio

    Add the ``consolefont`` hook to ``/etc/zfsbootmenu/mkinitcpio.conf`` after defining a font in ``/etc/vconsole.conf``. 

Change Keymap
-------------

.. tabs::

  .. group-tab:: Prebuilt

    ZFSBootMenu binary releases are built on top of Dracut. ``rd.vconsole.keymap`` controls the keymap.

  .. group-tab:: Dracut

    ``rd.vconsole.keymap`` controls the keymap.

  .. group-tab:: initcpio

    Add the ``keymap`` hook to ``/etc/zfsbootmenu/mkinitcpio.conf`` after defining a keymap in ``/etc/locale.conf``. 

Pass Parameters to ZFSBootMenu
------------------------------

ZFSBootMenu is built on top of a full Linux kernel and either ``Dracut`` or ``initcpio``. The process used to pass in parameters varies based on system configuration.

.. tabs::

  .. group-tab:: Direct

    With direct EFI booting, an entry to boot ``zfsbootmenu.EFI`` is usually added via ``efibootmgr``. The helper tool :zbm:`bin/zbm-kcl` (:doc:`man/zbm-kcl.8` zbm-kcl documentation) can be used to embed a kernel commandline in the EFI file. Alternatively, ``efibootmgr`` can add optional parameters to the boot entry.

  .. group-tab:: rEFInd

    The file ``refind_linux.conf`` in the same directory as the ZFSBootMenu EFI or kernel/initramfs pair can be used to pass a kernel commandline.

  .. group-tab:: syslinux

    Refer to :zbm:`contrib/syslinux-update.sh` for help with adjusting the generated `syslinux.conf` configuration file.
