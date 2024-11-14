include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

ifeq ($(SUBTARGET),arm)
  KERNEL_LOADADDR := 0x41208000
  DTS_CPPFLAGS-y += -D__ARM32__
else
  KERNEL_LOADADDR := 0x41000000
  DTS_CPPFLAGS-y += -D__AARCH64__
endif

define Device/Default
  PROFILES := Default
  DEVICE_DTS_DIR := ../dts
  DEVICE_DTS = $$(SOC)-$(lastword $(subst _, ,$(1)))
  DTS_CPPFLAGS += $(DTS_CPPFLAGS-y)
  IMAGES := sysupgrade.tar
  IMAGE/sysupgrade.tar := sysupgrade-tar | append-metadata
endef

define Device/FitImage
  KERNEL_SUFFIX := -fit-uImage.itb
  KERNEL = kernel-bin | lzma | fit lzma $$(KDIR)/image-$$(DEVICE_DTS).dtb
  KERNEL_NAME := Image
endef

define Device/UbiFit
  KERNEL_IN_UBI := 1
  IMAGES := nand-factory.ubi nand-sysupgrade.bin
  IMAGE/nand-factory.ubi := append-ubi
  IMAGE/nand-sysupgrade.bin := sysupgrade-tar | append-metadata
endef

define Device/xiaomi_cr880x
  $(call Device/FitImage)
  $(call Device/UbiFit)
  SOC := ipq5000
  DEVICE_VENDOR := Xiaomi
  DEVICE_MODEL := CR880X (M79 Version)
  BLOCKSIZE := 128k
  PAGESIZE := 2048
  DEVICE_DTS_CONFIG := config@mp02.1
  IMAGES := nand-factory.ubi
  DEVICE_PACKAGES := \
	ath11k-firmware-ipq5018 \
	ath11k-firmware-qcn6122 \
	ipq-wifi-xiaomi_cr880x
endef
TARGET_DEVICES += xiaomi_cr880x

$(eval $(call BuildImage))
