KERNELRELEASE	:= `uname -r`
KVERCLEAN       := $(shell echo $(KERNELRELEASE) | cut -f1 -d-)
KVERMAJ         := $(shell echo $(KVERCLEAN) | cut -f1 -d.)
KVERMIN         := $(shell echo $(KVERCLEAN) | cut -f2 -d.)
PWD		:= $(shell pwd)
HEADERSUBFOLDER  = $(shell [ $(KVERMAJ) -eq 2 -a $(KVERMIN) -lt 7 ] && echo /dvb || echo )

dvb_hdhomerun-objs := dvb_hdhomerun_init.o dvb_hdhomerun_data.o dvb_hdhomerun_control.o
obj-m += dvb_hdhomerun.o
obj-m += dvb_hdhomerun_core.o
obj-m += dvb_hdhomerun_fe.o

ccflags-y	+= -I$(src)/headers/$(KVERMAJ).$(KVERMIN) -D__DVB_CORE__

all: headers dvb_hdhomerun

headers: headers/$(KVERMAJ).$(KVERMIN)
	@echo Downloading kernel header dependencies...
	#@$(foreach var,$(shell grep -h ^\\\#include\ \" $(PWD)/headers/$(KVERMAJ).$(KVERMIN)/*.[ch] 2>/dev/null | sort -u | sed 's/\#include "\(.*\)"/\1/'),curl -# -L -o $(PWD)/headers/$(KVERMAJ).$(KVERMIN)/$(var) https://github.com/torvalds/linux/raw/v$(KVERMAJ).$(KVERMIN)/drivers/media$(HEADERSUBFOLDER)/dvb-core/$(var);)
	@$(foreach var,$(shell grep -h ^\\\#include\ \" $(PWD)/headers/$(KVERMAJ).$(KVERMIN)/*.[ch] 2>/dev/null | sort -u | sed 's/\#include "\(.*\)"/\1/'),curl -# -L -o $(PWD)/headers/$(KVERMAJ).$(KVERMIN)/$(var) https://github.com/torvalds/linux/tree/v$(KVERMAJ).$(KVERMIN)/include/$(var);)

headers/$(KVERMAJ).$(KVERMIN):
	@echo Downloading kernel headers...
	@mkdir -p $(PWD)/headers/$(KVERMAJ).$(KVERMIN)
	#@$(foreach var,$(shell grep -h ^\\\#include\ \" *.[ch] 2>/dev/null | grep -v hdhomerun | sort -u | sed 's/\#include "\(.*\)"/\1/'),curl -# -L -o $(PWD)/headers/$(KVERMAJ).$(KVERMIN)/$(var) https://github.com/torvalds/linux/raw/v$(KVERMAJ).$(KVERMIN)/drivers/media$(HEADERSUBFOLDER)/dvb-core/$(var);)
	@$(foreach var,$(shell grep -h ^\\\#include\ \" *.[ch] 2>/dev/null | grep -v hdhomerun | sort -u | sed 's/\#include "\(.*\)"/\1/'),curl -# -L -o $(PWD)/headers/$(KVERMAJ).$(KVERMIN)/$(var) https://github.com/torvalds/linux/tree/v$(KVERMAJ).$(KVERMIN)/include/$(var);)

dvb_hdhomerun:
	$(MAKE) -C /lib/modules/$(KERNELRELEASE)/build M=$(PWD) modules

install: dvb_hdhomerun
	$(MAKE) -C /lib/modules/$(KERNELRELEASE)/build M=$(PWD) modules_install

clean:
	$(MAKE) -C /lib/modules/$(KERNELRELEASE)/build M=$(PWD) clean
