======================
 Virtualization Guide
======================

SUSE Studio helps you to build your appliance in a variety of formats.

Logical Volume Manager (LVM) applies to the `Disk image, VMware, OVF and
Preload ISO
formats <http://susestudio.com/help/create/appliance-formats.html>`__
only!

Virtual formats
===============

VMware / VirtualBox / KVM (.vmdk)
---------------------------------

Use this format if you want to start your appliance as a virtual machine
on VMware, VirtualBox, or KVM-based hypervisors. This is also an easy
method to test an appliance without formatting any hard disk.

-  VirtualBox and VMware virtualization applications are available for
   most host operating systems.
-  KVM virtualization is for Linux only - See also: `openSUSE -
   Virtualization with
   KVM <http://doc.opensuse.org/documentation/html/openSUSE/opensuse-kvm/book.kvm.html>`__

OVF virtual machine (.ovf)
--------------------------

Open Virtualization Format (OVF) is an open, standards-based format for
virtual machines. A variety of hypervisors, including SUSE Cloud,
VirtualBox, VMware ESX, IBM SmartCloud, and Oracle VM, support creating
virtual machines by importing an .ovf file.

In order to use an OVF image, you may require the ovftool from VMware.
Due to licensing issues we cannot distribute the tools with SUSE Studio
at the moment. Download links, installation instructions, and
documentation are available at
`http://communities.vmware.com/community/vmtn/server/vsphere/automationtools/ovf <http://communities.vmware.com/community/vmtn/server/vsphere/automationtools/ovf>`__.
