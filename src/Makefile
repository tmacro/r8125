################################################################################
#
# r8125 is the Linux device driver released for Realtek 2.5Gigabit Ethernet
# controllers with PCI-Express interface.
#
# Copyright(c) 2019 Realtek Semiconductor Corp. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by the Free
# Software Foundation; either version 2 of the License, or (at your option)
# any later version.
#
# This program is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for
# more details.
#
# You should have received a copy of the GNU General Public License along with
# this program; if not, see <http://www.gnu.org/licenses/>.
#
# Author:
# Realtek NIC software team <nicfae@realtek.com>
# No. 2, Innovation Road II, Hsinchu Science Park, Hsinchu 300, Taiwan
#
################################################################################

################################################################################
#  This product is covered by one or more of the following patents:
#  US6,570,884, US6,115,776, and US6,327,625.
################################################################################

CONFIG_SOC_LAN = n
ENABLE_REALWOW_SUPPORT = n
ENABLE_DASH_SUPPORT = n
ENABLE_DASH_PRINTER_SUPPORT = n
CONFIG_DOWN_SPEED_100 = n
CONFIG_ASPM = y
ENABLE_S5WOL = y
ENABLE_S5_KEEP_CURR_MAC = n
ENABLE_EEE = y
ENABLE_S0_MAGIC_PACKET = n
ENABLE_TX_NO_CLOSE = y

ifneq ($(KERNELRELEASE),)
	obj-m := r8125.o
	r8125-objs := r8125_n.o rtl_eeprom.o rtltool.o
	ifeq ($(CONFIG_SOC_LAN), y)
		EXTRA_CFLAGS += -DCONFIG_SOC_LAN
	endif
	ifeq ($(ENABLE_REALWOW_SUPPORT), y)
	    r8125-objs += r8125_realwow.o
		EXTRA_CFLAGS += -DENABLE_REALWOW_SUPPORT
	endif
	ifeq ($(ENABLE_DASH_SUPPORT), y)
	    r8125-objs += r8125_dash.o
		EXTRA_CFLAGS += -DENABLE_DASH_SUPPORT
	endif
	ifeq ($(ENABLE_DASH_PRINTER_SUPPORT), y)
	    r8125-objs += r8125_dash.o
		EXTRA_CFLAGS += -DENABLE_DASH_SUPPORT -DENABLE_DASH_PRINTER_SUPPORT
	endif
	EXTRA_CFLAGS += -DCONFIG_R8125_NAPI
	EXTRA_CFLAGS += -DCONFIG_R8125_VLAN
	ifeq ($(CONFIG_DOWN_SPEED_100), y)
		EXTRA_CFLAGS += -DCONFIG_DOWN_SPEED_100
	endif
	ifeq ($(CONFIG_ASPM), y)
		EXTRA_CFLAGS += -DCONFIG_ASPM
	endif
	ifeq ($(ENABLE_S5WOL), y)
		EXTRA_CFLAGS += -DENABLE_S5WOL
	endif
	ifeq ($(ENABLE_S5_KEEP_CURR_MAC), y)
		EXTRA_CFLAGS += -DENABLE_S5_KEEP_CURR_MAC
	endif
	ifeq ($(ENABLE_EEE), y)
		EXTRA_CFLAGS += -DENABLE_EEE
	endif
	ifeq ($(ENABLE_S0_MAGIC_PACKET), y)
		EXTRA_CFLAGS += -DENABLE_S0_MAGIC_PACKET
	endif
	ifeq ($(ENABLE_TX_NO_CLOSE), y)
		EXTRA_CFLAGS += -DENABLE_TX_NO_CLOSE
	endif
else

ifeq ($(BASEDIR),)
BASEDIR := /lib/modules/$(shell uname -r)
endif

ifeq ($(KERNELDIR),)
KERNELDIR ?= $(BASEDIR)/build
endif

PWD :=$(shell pwd)
DRIVERDIR := $(shell find $(BASEDIR)/kernel/drivers/net/ethernet -name realtek -type d)
ifeq ($(DRIVERDIR),)
	DRIVERDIR := $(shell find $(BASEDIR)/kernel/drivers/net -name realtek -type d)
endif
ifeq ($(DRIVERDIR),)
	DRIVERDIR := $(BASEDIR)/kernel/drivers/net
endif
RTKDIR := $(subst $(BASEDIR)/,,$(DRIVERDIR))

.PHONY: all
all: print_vars clean modules install

print_vars:
	@echo
	@echo "CC: " $(CC)
	@echo "CCVERSION: " $(CCVERSION)
	@echo "KERNEL_GCC_VERSION: " $(KERNEL_GCC_VERSION)
	@echo "KVER: " $(KVER)
	@echo "KMAJ: " $(KMAJ)
	@echo "KMIN: " $(KMIN)
	@echo "KREV: " $(KREV)
	@echo "BASEDIR: " $(BASEDIR)
	@echo "DRIVERDIR: " $(DRIVERDIR)
	@echo "PWD: " $(PWD)
	@echo "RTKDIR: " $(RTKDIR)
	@echo

.PHONY:modules
modules:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) modules

.PHONY:clean
clean:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) clean

.PHONY:install
install:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) INSTALL_MOD_DIR=$(RTKDIR) modules_install

endif
