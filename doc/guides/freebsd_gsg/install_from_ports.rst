..  SPDX-License-Identifier: BSD-3-Clause
    Copyright(c) 2010-2014 Intel Corporation.

.. _install_from_ports:

Installing DPDK from the Ports Collection
=========================================

The easiest way to get up and running with the DPDK on FreeBSD is to
install it from the ports collection. Details of getting and using the ports
collection are documented in the
`FreeBSD Handbook <http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/index.html>`_.

Installing the DPDK FreeBSD Port
--------------------------------

On a system with the ports collection installed in ``/usr/ports``, the DPDK
can be installed using the commands:

.. code-block:: console

    cd /usr/ports/net/dpdk

    make install

After the installation of the DPDK port, instructions will be printed on
how to install the kernel modules required to use the DPDK. A more
complete version of these instructions can be found in the sections
:ref:`loading_contigmem` and :ref:`loading_nic_uio`. Normally, lines like
those below would be added to the file ``/boot/loader.conf``.

.. code-block:: console

    # Reserve 2 x 1G blocks of contiguous memory using contigmem driver:
    hw.contigmem.num_buffers=2
    hw.contigmem.buffer_size=1073741824
    contigmem_load="YES"

    # Identify NIC devices for DPDK apps to use and load nic_uio driver:
    hw.nic_uio.bdfs="2:0:0,2:0:1"
    nic_uio_load="YES"

.. note::

   Please ensure that the latest patches are applied to third party libraries
   and software to avoid any known vulnerabilities.


Compiling and Running the Example Applications
----------------------------------------------

When the DPDK has been installed from the ports collection it installs
its example applications in ``/usr/local/share/dpdk/examples`` - also accessible via
symlink as ``/usr/local/share/examples/dpdk``. These examples can be compiled and
run as described in :ref:`compiling_sample_apps`. In this case, the required
environmental variables should be set as below:

* ``RTE_SDK=/usr/local/share/dpdk``

* ``RTE_TARGET=x86_64-native-freebsd-clang``

.. note::

   To install a copy of the DPDK compiled using gcc, please download the
   official DPDK package from http://core.dpdk.org/download/ and install manually using
   the instructions given in the next chapter, :ref:`building_from_source`

An example application can therefore be copied to a user's home directory and
compiled and run as below:

.. code-block:: console

    export RTE_SDK=/usr/local/share/dpdk

    export RTE_TARGET=x86_64-native-freebsd-clang

    cp -r /usr/local/share/dpdk/examples/helloworld .

    cd helloworld/

    gmake
      CC main.o
      LD helloworld
      INSTALL-APP helloworld
      INSTALL-MAP helloworld.map

    sudo ./build//helloworld -l 0-3
    EAL: Sysctl reports 8 cpus
    EAL: Detected 8 lcore(s)
    EAL: Detected 1 NUMA nodes
    EAL: Multi-process socket /var/run/dpdk/rte/mp_socket
    EAL: Selected IOVA mode 'PA'
    EAL: Contigmem driver has 2 buffers, each of size 1GB
    EAL: Mapped memory segment 0 @ 0x1040000000: physaddr:0x180000000, len 1073741824
    EAL: Mapped memory segment 1 @ 0x1080000000: physaddr:0x1c0000000, len 1073741824
    EAL: PCI device 0000:00:19.0 on NUMA socket 0
    EAL:   probe driver: 8086:153b net_e1000_em
    EAL:   0000:00:19.0 not managed by UIO driver, skipping
    EAL: PCI device 0000:01:00.0 on NUMA socket 0
    EAL:   probe driver: 8086:1572 net_i40e
    EAL:   0000:01:00.0 not managed by UIO driver, skipping
    EAL: PCI device 0000:01:00.1 on NUMA socket 0
    EAL:   probe driver: 8086:1572 net_i40e
    EAL:   0000:01:00.1 not managed by UIO driver, skipping
    EAL: PCI device 0000:01:00.2 on NUMA socket 0
    EAL:   probe driver: 8086:1572 net_i40e
    EAL:   0000:01:00.2 not managed by UIO driver, skipping
    EAL: PCI device 0000:01:00.3 on NUMA socket 0
    EAL:   probe driver: 8086:1572 net_i40e
    EAL:   0000:01:00.3 not managed by UIO driver, skipping
    hello from core 1
    hello from core 2
    hello from core 3
    hello from core 0


.. note::

   To run a DPDK process as a non-root user, adjust the permissions on
   the ``/dev/contigmem`` and ``/dev/uio device`` nodes as described in section
   :ref:`running_non_root`

.. note::

   For an explanation of the command-line parameters that can be passed to an
   DPDK application, see section :ref:`running_sample_app`.
