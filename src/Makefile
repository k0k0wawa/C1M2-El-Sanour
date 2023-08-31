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
# Simple makefile for the MSP432 or HOST machine
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.i - Generates the preprocessed output
#      <FILE>.asm - Generates the assemply output
#      <FILE>.o - Generates the opject file
#      compile-all - Builds but doesn't link all the object files
#      build - Builds and links all object files
#      clean - removes all generated files
#
# Platform Overrides:
#      CPU - ARM Cortex Architecture (cortex-m0plus, cortex-m4)
#      ARCH - ARM Architecture (arm, thumb)
#      SPECS - Specs file to give the linker (nosys.specs, nano.specs)
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides

# General Flags
GCFLAGS = -Wall   \
	  -Werror \
	  -g      \
	  -O0     \
	  -std=c99

GLDFLAGS = -Wl,-Map=c1m2.map
CPPFLAGS = $(INCLUDES)

# Architecutures Specific Flags
CPU = cortex-m4
STATE = thumb
SPECS = nosys.specs
FPU = fpv4-sp-d16
ABI = hard
ARCH = armv7e-m

# Platform Specific Flags
LINKER_FILE = ./../msp432p401r.lds


OBJS = $(SOURCES:.c=.o)
DEP = $(SOURCES:.c=.d)
ASM = $(SOURCES:.c=.asm)
PRE = $(SOURCES:.c=.i)

ifeq ($(PLATFORM), HOST)
    # Compiler Flags and Defines
    CC = gcc
    CFLAGS = -DHOST $(GCFLAGS)
    LDFLAGS = $(GLDFLAGS)
else
    CC = arm-none-eabi-gcc
    CFLAGS = -DMSP432 $(GCFLAGS) -mcpu=$(CPU) -m$(STATE)        \
	     --specs=$(SPECS) -march=$(ARCH) -mfloat-abi=$(ABI) \
	     -mfpu=$(FPU)
    LDFLAGS = $(GLDFLAGS) -T $(LINKER_FILE)
endif

# Architectures Specific Flags
#LINKER_FILE = 
#CPU = 
#ARCH = 
#SPECS = 

# Compiler Flags and Defines
#CC = 
#LD = 
#LDFLAGS = 
#CFLAGS = 
#CPPFLAGs = 

%.i : %.c
	$(CC) -E $^ $(CPPFLAGS) $(CFLAGS) -o $@

%.asm : %.c
	$(CC) -S $^ $(CPPFLAGS) $(CFLAGS) -o $@

%.o : %.c
	$(CC) -c $^ $(CPPFLAGS) $(CFLAGS) -o $@

%.d : %.c
	$(CC) -M $^ $(CPPFLAGS) $(CFLAGS) > $@

.PHONY: compile-all
compile-all: $(OBJS)

.PHONY: build
build: c1m2.out

c1m2.out: $(OBJS) $(DEP)
	$(CC) $(OBJS) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm $(OBJS) $(DEP) $(ASM) $(PRE) c1m2.map c1m2.out
