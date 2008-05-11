topdir:=../..

SUBDIRS =
include $(topdir)/autoconf/targets.mak

all-install: install-suse
all-uninstall: uninstall-suse

install-suse:
	$(call DISTINST,SuSE)
   # unlink old rc script (if not doing a DESTDIR install)
	$(if $(DESTDIR),,$(V)/sbin/chkconfig --del apcupsd)
   # install new rc script
	$(call MKDIR,/etc/rc.d/init.d)
	$(call INSTDATA,744,apcupsd,/etc/rc.d/init.d)
   # save old halt script
	$(call COPY,/etc/rc.d/halt,/etc/rc.d/halt.old)
   # insert apcupsd callout into halt script
	@echo "  PATCH" $(DESTDIR)/etc/rc.d/halt
	$(V)awk -f awkhaltprog $(DESTDIR)/etc/rc.d/halt.old >$(DESTDIR)/etc/rc.d/halt
	$(V)chmod 744 $(DESTDIR)/etc/rc.d/halt
   # link new rc script (if not doing a DESTDIR install)
	$(if $(DESTDIR),,$(V)/sbin/chkconfig --add apcupsd)
	@echo "================================================="
	@echo "apcupsd script installation for SuSE $(DISTVER) complete." 
	@echo " "
	@echo "You should now edit  /etc/apcupsd/apcupsd.conf  to correspond"
	@echo "to your setup then start the apcupsd daemon with:"
	@echo " "
	@echo "/etc/rc.d/apcupsd start"
	@echo " "
	@echo "thereafter when you reboot, it will be stopped and started"
	@echo "automatically."
	@echo " "
	@echo "Please check that your halt script in:"
	@echo "    /etc/rc.d/halt"
	@echo "was properly updated (see installation section of manual)"
	@echo "================================================="

uninstall-suse:
   # unlink old rc script (if not doing a DESTDIR install)
	$(if $(DESTDIR),,$(V)/sbin/chkconfig --del apcupsd)
	$(call UNINST,/etc/rc.d/apcupsd)
	$(call COPY,$(DESTDIR)/etc/rc.d/halt.old,/etc/rc.d/halt)
	$(call UNINST,/etc/rc.d/halt.old)