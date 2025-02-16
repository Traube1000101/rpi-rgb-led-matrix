# Creates RGB matrix library

OBJECTS=gpio.o led-matrix.o options-initialize.o framebuffer.o \
        thread.o bdf-font.o graphics.o led-matrix-c.o hardware-mapping.o \
        pixel-mapper.o multiplex-mappers.o \
	content-streamer.o

TARGET=librgbmatrix

DEFINES+=-DDISABLE_BUSY_WAITING # Use non blocking delay for the single core rpi
DEFINES+=-DDEFAULT_HARDWARE='"regular"' # Use regular GPIO layout
DEFINES+=$(USER_DEFINES) # Add additional make defines

INCDIR=../include
OUTDIR=../../out
CFLAGS=-W -Wall -Wextra -Wno-unused-parameter -O3 -g -fPIC $(DEFINES) -mcpu=arm1176jzf-s -mfpu=vfp -mfloat-abi=hard
CXXFLAGS=$(CFLAGS) -fno-exceptions -std=c++11

all : $(TARGET).a $(TARGET).so.1

$(TARGET).a : $(OBJECTS)
	$(AR) rcs $@ $^

$(TARGET).so.1 : $(OBJECTS)
	$(CXX) -shared -Wl,-soname,$@ -o $(OUTDIR)/$@ $^ -lpthread  -lrt -lm -lpthread

led-matrix.o: led-matrix.cc $(INCDIR)/led-matrix.h
thread.o : thread.cc $(INCDIR)/thread.h
framebuffer.o: framebuffer.cc framebuffer-internal.h
graphics.o: graphics.cc utf8-internal.h

%.o : %.cc compiler-flags
	$(CXX) -I$(INCDIR) $(CXXFLAGS) -c -o $@ $<

%.o : %.c compiler-flags
	$(CC)  -I$(INCDIR) $(CFLAGS) -c -o $@ $<

clean:
	rm -f $(OBJECTS) $(TARGET).a $(OUTDIR)/$(TARGET).so.1 compiler-flags

compiler-flags: FORCE
	@echo '$(CXX) $(CXXFLAGS)' | cmp -s - $@ || echo '$(CXX) $(CXXFLAGS)' > $@

.PHONY: FORCE
