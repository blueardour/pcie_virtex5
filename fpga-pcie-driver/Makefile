# Makefile for fpga pcie drivers
#
# Will compile and install for other kernel than the currently running,
# given the TARGET parameter (should be the name of a directory in
# /lib/modules) e.g.
# make TARGET=3.10.0-CUSTOM
#
# This makefile compiles the drivers for fpga using PCIe.

ifneq ($(KERNELRELEASE),)
obj-m	:= fpga-pcie.o

else
ifeq ($(TARGET),)
TARGET := $(shell uname -r)
endif
PWD := $(shell pwd)
KDIR := /lib/modules/$(TARGET)/build

default:
	@echo $(TARGET) > module.target
	$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules

clean:
	@rm -f *.ko *.o modules.order Module.symvers *.mod.? .fpga-pcie_*.* *~
	@rm -rf .tmp_versions module.target

install: xillybus_core.ko xillybus_pcie.ko # xillybus_of.ko
	install --mode 0644 fpga-pcie.ko /lib/modules/$(shell cat module.target)/kernel/drivers/char/
	/sbin/depmod -a $(shell cat module.target)
fpga-pcie.ko:
	$(MAKE)
endif
