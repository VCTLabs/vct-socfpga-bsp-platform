VCT socfpga BSP startup
=======================

To get the BSP you need to have repo installed and use it as:

Install the repo utility
------------------------

::

  $ mkdir ~/bin
  $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  $ chmod a+x ~/bin/repo

Download the BSP source
-----------------------

::

  $ PATH=${PATH}:~/bin
  $ mkdir socfpga-bsp
  $ cd socfpga-bsp
  $ repo init -u https://github.com/VCTLabs/vct-socfpga-bsp-platform -b poky-dunfell
  $ repo sync

At the end of the commands you have every metadata you need to start work with.
The branch you select above will checkout the corresponding branch HEAD commits
on the configured branch for each repository.

See the default.xml file for repo and branch details.

NOTE: the manifest file currently contains more than one BSP layer, so you must
choose only one in a given build. You must only enable one of the following in your
bblayers.conf file:

* meta-intel-fpga-refdes - The official Yocto Project BSP layer for Intel SoCFPGA Golden Software Reference Design (GSRD)
* meta-intel-fpga - The official OpenEmbedded/Yocto BSP layer for Intel SoCFPGA platforms
* meta-altera - The official OpenEmbedded/Yocto BSP layer for Altera SoCFPGA platforms

To start a simple image build::

  $ cd poky
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to arria10
  $ ${EDITOR} conf/bblayers.conf          # set only one BSP
  $ bitbake core-image-minimal

You can use any directory (build-dir above) to host your build.  The above commands will
build an image for beaglebone using the core yocto BSP machine config and the default
yocto-linux kernel.  You can replace the altera BSP with yocto and meta-small-arm-extra,
but only if you select the newer kernel and u-boot recipes (not yet implemented).
This will provide a more current set of defaults for kernel and
bootloader, as well as increase selection of available kernels.

The main source code is checked out in the bsp dir above, and the build dir will default
to poky/build-dir unless you choose a different path above.

.. note:: You need to replace the meta-yocto-bsp layer with 
          the meta-altera layer (for the Altera BSP).  Additional kernels
          have not been packaged yet, and will appear on a different branch.

Source code
-----------

Download the manifest source here::

  $ git clone https://github.com/VCTLabs/vct-socfpga-bsp-platform

Using Development and Testing Branches
--------------------------------------

Replace the repo init command above with one of the following:

For developers - kirkstone

::

  $ repo init -u https://github.com/VCTLabs/vct-socfpga-bsp-platform -b poky-kirkstone

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/VCTLabs/vct-socfpga-bsp-platform -b poky-master

