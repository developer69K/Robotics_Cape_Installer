# makefile for robotics cape battery monitor service

SERVICE		:= rc_battery_monitor

# directories
SRCDIR		:= src
BINDIR		:= bin
INCLUDEDIR	:= ../../library/include
LIBDIR		:= ../../library/lib

# file definitions for rules
TARGET		:= $(BINDIR)/rc_battery_monitor
INCLUDES	:= $(shell find $(INCLUDEDIR) -name '*.h')
SOURCES		:= $(shell find $(SRCDIR) -name '*.c')

# compiler and linker binaries
CC		:= gcc
LINKER		:= gcc

# compiler and linker flags
WFLAGS		:= -Wall -Wextra -Werror=float-equal -Wuninitialized -Wunused-variable -Wdouble-promotion
CFLAGS		:= -g -pthread -I $(INCLUDEDIR)
LDFLAGS		:= -lm -lrt -pthread -L $(LIBDIR) -lroboticscape

# commands
RM		:= rm -rf
INSTALL		:= install -m 755
INSTALLNONEXEC	:= install -m 644
INSTALLDIR	:= install -d -m 755

# prefix variable for making debian package
prefix		?= /usr


all: $(TARGET)

install:
	@$(MAKE)
	@$(INSTALLDIR) $(DESTDIR)$(prefix)/bin
	@$(INSTALL) $(TARGET) $(DESTDIR)$(prefix)/bin/
	@$(INSTALLDIR) $(DESTDIR)/lib/systemd/system
	@$(INSTALLNONEXEC) $(SERVICE).service $(DESTDIR)/lib/systemd/system/
	@$(INSTALLDIR) $(DESTDIR)/etc/modules-load.d/
	@echo ""
	@echo "rc_battery_monitor Service Install Complete"
	@echo "run the following to manually enable:"
	@echo "sudo systemctl daemon-reload"
	@echo "sudo systemctl enable rc_battery_monitor"
	@echo "sudo systemctl start rc_battery_monitor"
	@echo ""

clean:
	@$(RM) $(BINDIR)
	@echo "rc_battery_monitor Service Clean Complete"

uninstall:
	@$(RM) $(DESTDIR)/lib/systemd/system/$(SERVICE).service
	@$(RM) $(DESTDIR)$(prefix)/$(TARGET)
	@echo "rc_battery_monitor Service Uninstall Complete"


$(BINDIR)/% : $(SRCDIR)/%.c
	@mkdir -p $(BINDIR)
	@$(CC) -o $@ $< $(CFLAGS) $(WFLAGS) $(DEBUGFLAG) $(LDFLAGS)
	@echo "made: $@"
