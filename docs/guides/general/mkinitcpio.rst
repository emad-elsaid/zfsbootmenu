Building with mkinitcpio
========================

ZFSBootMenu also supports the `mkinitcpio <https://gitlab.archlinux.org/archlinux/mkinitcpio/mkinitcpio/>`_ initramfs
generator used by Arch Linux, but it must be configured first.

Since `version 2.0.0 <https://github.com/zbm-dev/zfsbootmenu/releases/tag/v2.0.0>`_, ZFSBootMenu will install a standard
:zbm:`mkinitcpio.conf <etc/zfsbootmenu/mkinitcpio.conf>` in the ``/etc/zfsbootmenu`` configuration directory. This file
is generally the same as a standard ``mkinitcpio.conf``, except some additional declarations may be added to control
aspects of the ``zfsbootmenu`` mkinitcpio module. The configuration file includes extensive inline documentation in the
form of comments; configuration options specific to ZFSBootMenu are also described in the
:ref:`zfsbootmenu(7) <zbm-mkinitcpio-options>` manual page.

ZFSBootMenu still expects to use Dracut by default. To override this behavior and instead use mkinitcpio, edit
``/etc/zfsbootmenu/config.yaml`` and add the following options:

.. code-block:: yaml

  Global:
    InitCPIO: true
    ## NOTE: The following three lines are OPTIONAL
    InitCPIOHookDirs:
      - /etc/zfsbootmenu/initcpio
      - /usr/lib/initcpio

.. note::

  In some ZFSBootMenu guides, like :doc:`remote-access`, some mkinitcpio modules will be installed to
  ``/etc/zfsbootmenu/initcpio`` to keep them isolated from system-installed modules. To accommodate this non-standard
  installation, ``InitCPIOHookDirs`` must be defined in ``/etc/zfsbootmenu/config.yaml``. Furthermore, because
  overriding the hook directory causes mkinitcpio to ignore its default module path, the default ``/usr/lib/initcpio``
  must be manually specified. If all hooks are installed in ``/usr/lib/initcpio`` or ``/etc/initcpio``, the ZFSBootMenu
  configuration does **not** need to specify ``InitCPIOHookDirs``.

Without further changes, running ``generate-zbm`` should now produce a ZBM image based on mkinitcpio rather than Dracut.

Because further configuration may require additional mkinitcpio modules, and these must be run before the
``zfsbootmenu`` module in the initramfs, edit ``/etc/zfsbootmenu/mkinitcpio.conf`` and **remove** any ``zfsbootmenu``
entry in the ``HOOKS`` definition. As the standard configuration file notes, the ``zfsbootmenu`` module is required for
ZFSBootMenu to function, but ``generate-zbm`` will forcefully add it at this at the end of the module list. Thus, the
simplest way to ensure that additions to the ``HOOKS`` array occur *before* the ``zfsbootmenu`` module is to omit the
latter from the configuration. The standard ``HOOKS`` line in ``/etc/zfsbootmenu/mkinitcpio.conf`` should therefore be
something like::

  HOOKS=(base udev autodetect modconf block filesystems keyboard)
