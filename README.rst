==============================
 VCT socfpga BSP Manifest
==============================

The various branches (other than this one) available here will configure the repo build
for the appropriate branches in each repository and clone them in the typical fashion,
ie, with either poky or oe-core as the parent directory and the additional metadata
layers underneath (as documented in the upstream setup).

The 2 main choices are:

* The yocto BSP (and poky reference distribution) with meta-openembedded metadata
  plus the altera socfpga BSP and `meta-small-arm-extra`_

* The Angstrom metadata with the altera socfpga BSP
  and meta-small-arm-extra for additional recipes

  - The more recent release layers have additional metadata (layers) such as
    meta-qt; add to bblayers.conf as needed
  - There are additional kernel and u-boot recipes in meta-small-arm-extra
    based on the latest patches and mainline branches (and some from the
    `LinuxOnArm wiki`_)

.. _LinuxOnArm wiki: https://eewiki.net/display/linuxonarm/BeagleBone+Black
.. _meta-small-arm-extra: https://github.com/sarnold/meta-small-arm-extra

Use the branch button to select a build branch; note that upstream mainline
support is still not quite complete; newer/experimental recipes are found in
meta-small-arm-extra.

Follow the steps in the readme for the branch you select, then use the same branch
name with the "repo init" command.  This will checkout the
corresponding branch HEAD commits on the configured branch for each repository.

You can get status and more using the ``repo`` command in the top level directory
once you've run the ``repo init`` and ``repo sync`` commands.  Try::

  $ repo info
  $ repo show
  $ repo status

See the default.xml file for repo and branch details; the following example is generic
so go back and select a branch from this repo.

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
  $ repo init -u https://github.com/VCTLabs/vct-socfpga-bsp-platform -b poky-krogoth
  $ repo sync

At the end of the above commands you have all the metadata you need to start
building with poky and meta-oe on krogoth and master branches.

To start a simple image build::

  $ cd poky
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to cyclone5
  $ bitbake core-image-minimal

You can use any directory (build-dir above) to host your build. The above commands will build an image for beaglebone using the core yocto BSP machine config and the default yocto-linux kernel. You can replace the default BSP config with either meta-ti or the meta-socfpga BSP. This will provide a more Beagle-centric set of defaults for kernel and bootloader, as well as a bigger selection of kernels and TI support tools.

The main source code is checked out in the bsp dir above, and the build dir will default
to poky/build-dir unless you choose a different path above.

Source code
-----------

Download the manifest source here::

  $ git clone https://github.com/VCTLabs/vct-socfpga-bsp-platform

Using Development and Testing/Release Branches
----------------------------------------------

Replace the repo init command above with one of the following:

For developers - krogoth

::

  $ repo init -u https://github.com/VCTLabs/vct-socfpga-bsp-platform -b poky-krogoth

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/VCTLabs/vct-socfpga-bsp-platform -b poky-master


