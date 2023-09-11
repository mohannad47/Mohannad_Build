#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
ifeq ($(PLATFORM),MSP432)

	PLATFORMFLAG = -DMSP432
# Architectures Specific Flags
	LINKER_FILE = ../msp432p401r.lds
	CPU = cortex-m4
	ARCH = thumb
	MARCH = armv7e-m
	MFLOAT = hard
	MFPU = fpv4-sp-d16
	SPECS = nosys.specs

# Compiler Flags and Defines
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	TARGET = c1m2
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	CFLAGS = 	-std=c99 -mcpu=$(CPU) -m$(ARCH) -march=$(MARCH) \
				-mfloat-abi=$(MFLOAT) -mfpu=$(MFPU) \
				--specs=$(SPECS) -Wall -Werror -g -O0
	CPPFLAGS = $(INCLUDES)
else
	PLATFORMFLAG = -DHOST
# Architectures Specific Flags

# Compiler Flags and Defines
	CC = gcc
	TARGET = c1m2
	LDFLAGS = -Wl,-Map=$(TARGET).map 
	CFLAGS = -std=c99 -Wall -Werror -g -O0
	CPPFLAGS = $(INCLUDES)
endif


.PHONY: build
build: all
all:c1m2.out

c1m2.out: main.o memory.o
	$(CC) $(CFLAGS) $^ -o $@

%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $^ -o $@ $(PLATFORMFLAG)
%.i: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -E $^ -o $@ $(PLATFORMFLAG)
%.asm: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -S $^ -o $@ $(PLATFORMFLAG)


.PHONY: clean
clean:
	rm -f main.o memory.o c1m2.out main.i main.asm
