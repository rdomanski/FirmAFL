# Tracecap is owned and copyright (C) BitBlaze, 2007-2009.
# All rights reserved.
# Do not copy, disclose, or distribute without explicit written
# permission.

include config-plugin.mak
include $(SRC_PATH)/$(TARGET_DIR)/config-target.mak
include $(SRC_PATH)/config-host.mak

#zyw
GLIB_CFLAGS=-pthread -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include

#zyw TARGET_BASE_ARCH  -I$(SRC_PATH)/include
DEFINES=-I. -I$(SRC_PATH)/include -I$(SRC_PATH) -I$(SRC_PATH)/plugins -I$(SRC_PATH)/fpu -I$(SRC_PATH)/shared -I$(SRC_PATH)/target/$(TARGET_BASE_ARCH) -I$(SRC_PATH)/$(TARGET_DIR) -I$(SRC_PATH)/slirp 
DEFINES+=-D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -D_GNU_SOURCE -DNEED_CPU_H
DEFINES+=-I$(GLIB_CFLAGS)

#zyw
DEFINES+=-I$(SRC_PATH)/linux-headers -I$(SRC_PATH)/tcg -I$(SRC_PATH)/tcg/$(TARGET_BASE_ARCH) 

CC=gcc
CPP=g++
CFLAGS= -Wall -O2 -g -fPIC -MMD 
# CFLAGS=-Wall -g -fPIC 
LDFLAGS=-g -shared

OBJS= callbacktests.o 
# temu, qemu-tools removed as target
all: callbacktests.so 

qemu-tools:
	make -C $(SRC_PATH) all

decaf:
	make -C $(TARGET_DIR) all

%.o: %.c 
	$(CC) $(CFLAGS) $(DEFINES) -c -o $@ $<

%.o: %.cpp
	$(CPP) $(CFLAGS) $(DEFINES) -c -o $@ $<

callbacktests.so: $(SHARED_LIBS) $(OBJS)
	$(CPP) $(LDFLAGS) $^ -o $@ $(LIBS)
	ar cru libcallbacktests.a $@

callbacktests-static.so: $(OBJS)
	$(CPP) -static-libgcc -Wl,-static $(LDFLAGS) $^ -o $@ $(LIBS)

clean:
	rm -f *.o *.d *.so *.a *~ $(PLUGIN) 

realclean:
	rm -f *.o *.d *.so *.a *~ $(PLUGIN) config-plugin.h config-plugin.mak

# Include automatically generated dependency files
-include $(wildcard *.d)

