#---------------------------------------------------------------------------------
.SUFFIXES:
#---------------------------------------------------------------------------------
define n


endef

ifeq ($(strip $(DEVKITARM)),)
$(error "Please set DEVKITARM in your environment. export DEVKITARM=<path to>devkitARM")
endif

ifeq ($(strip $(LOVEPOTION_3DS)),)

export ERR_MSG := \
$nPlease set LOVEPOTION_3DS in your environment.\
$nThis should be the path to your Love Potion projects.\
$nDo *NOT* save the *.elf file anywhere else.\
$nexport LOVEPOTION_3DS=<path to>/LovePotion.elf
$(error $(ERR_MSG))
endif

TOPDIR ?= $(CURDIR)
include $(DEVKITARM)/3ds_rules

#---------------------------------------------------------------------------------
# TARGET is the name of the output files
# BUILD is the directory where object files & intermediate files will be placed
# ROMFS is the directory containing your LOVE game
#
# APP_TITLE is the name of the app stored in the .3dsx file (Optional)
# APP_AUTHOR is the author of the app stored in the .3dsx file (Optional)
# APP_VERSION is the version of the app stored in the .3dsx file (Optional)
# APP_TITLEID is the titleID of the app stored in the .3dsx file (Optional)
# APP_DESCRIPTION is the description of the application
#
# ICON is the filename of the icon (.png), relative to the project folder.
#---------------------------------------------------------------------------------
TARGET          := $(notdir $(CURDIR))
BUILD           := $(TOPDIR)

ROMFS           := game

APP_TITLE       := Löve Potion
APP_AUTHOR      := TurtleP
APP_TITLEID     := 0x1043
APP_VERSION     := 1.1
APP_DESCRIPTION := LOVE for 3DS

ICON            := icon.png

#---------------------------------------------------------------------------------
# build options
#---------------------------------------------------------------------------------

export OUTPUT    :=    $(TARGET)
export TOPDIR    :=    $(CURDIR)

ifeq ($(strip $(ICON)),)
	icons := $(wildcard *.png)
	ifneq (,$(findstring $(TARGET).png,$(icons)))
		export APP_ICON := $(TOPDIR)/$(TARGET).png
	else
		ifneq (,$(findstring icon.png,$(icons)))
			export APP_ICON := $(TOPDIR)/icon.png
		endif
	endif
else
	export APP_ICON := $(TOPDIR)/$(ICON)
endif

#---------------------------------------------------------------------------------
# main targets
#---------------------------------------------------------------------------------

all: $(OUTPUT).smdh $(OUTPUT).3dsx

clean:
	@echo clean ...
	@rm -fr $(TARGET).3dsx $(OUTPUT).smdh $(OUTPUT).cia banner.bnr icon.icn

$(OUTPUT).smdh:
	@echo "Building smdh.."
	@smdhtool --create "$(APP_TITLE)" "$(APP_DESCRIPTION)" "$(APP_AUTHOR)" "$(APP_ICON)" $@

$(OUTPUT).3dsx:
	@echo "Building 3dsx.."
	@3dsxtool $(LOVEPOTION_3DS) $@ --smdh=$(OUTPUT).smdh --romfs=$(ROMFS)
