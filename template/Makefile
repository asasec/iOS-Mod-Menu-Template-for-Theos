# THEOS_DEVICE_IP = 127.0.0.1 -p 2222
ARCHS = arm64 arm64e
DEBUG = 0
FINALPACKAGE = 1
FOR_RELEASE = 1

# 0 to treat warnings as errors, 1 otherwise.
IGNORE_WARNINGS = 1
GO_EASY_ON_ME = 1

# 0 to compile for rootful jailbreaks, 1 otherwise.
ROOTLESS = 0

ifeq ($(ROOTLESS), 1)
THEOS_PACKAGE_SCHEME = rootless
endif 

MOBILE_THEOS = 0
ifeq ($(MOBILE_THEOS),1)
  SDK_PATH = $(THEOS)/sdks/iPhoneOS11.2.sdk/
  $(info ===> Setting SYSROOT to $(SDK_PATH)...)
  SYSROOT = $(SDK_PATH)
else
  TARGET = iphone:clang:latest:14.0
endif

## Common frameworks ##
PROJ_COMMON_FRAMEWORKS = UIKit Foundation Security QuartzCore CoreGraphics CoreText

## source files ##
KITTYMEMORY_SRC = $(wildcard KittyMemory/*.cpp)
SCLALERTVIEW_SRC = $(wildcard SCLAlertView/*.m)
MENU_SRC = Menu.mm

include $(THEOS)/makefiles/common.mk

TWEAK_NAME = MyModMenu

MyModMenu_CFLAGS = -fobjc-arc
MyModMenu_CCFLAGS = -std=c++11 -fno-rtti -fno-exceptions -DNDEBUG

ifeq ($(IGNORE_WARNINGS),1)
  MyModMenu_CFLAGS += -w
  MyModMenu_CCFLAGS += -w
endif

MyModMenu_FILES = Tweak.xm $(MENU_SRC) $(KITTYMEMORY_SRC) $(SCLALERTVIEW_SRC)
MyModMenu_LIBRARIES += substrate
MyModMenu_FRAMEWORKS = $(PROJ_COMMON_FRAMEWORKS)

include $(THEOS_MAKE_PATH)/tweak.mk

internal-package-check::
	@chmod 777 versionCheck.sh || true
	@./versionCheck.sh || true

after-install::
	install.exec "killall -9 SpringBoard || :"
