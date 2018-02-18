#******************************************************************************
# Copyright (C) 2018 by William Dallari
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. William Dallari is not liable for any misuse of this material. 
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
PLATFORM =

# Compiler Flags
CFLAGS = -Wall -Werror -g -O0 -std=c99

ifeq ($(PLATFORM),HOST)

# C Compiler
CC = gcc

# C Preprocessor Flags
CPPFLAGS = -DHOST $(INCLUDES)

# Link Editor
LD = $(CC)

OBJDUMP = objdump

# Flags for GNU "Objdump" utility
OBJDUMP_FLAGS = -d -S

endif

ifeq ($(PLATFORM),MSP432)
# Architectures Specific Flags
LINKER_FILE = -T msp432p401r.lds

CPU   = cortex-m4
ARCH  = armv7e-m
SPECS = nosys.specs
endif

%.i : %.c
	$(CC) $(CPPFLAGS) -E $< -o $@

%.s : %.c
	$(CC) $(CPPFLAGS) -S $< -o $@

%.asm : %.s
	$(OBJDUMP) $(OBJDUMP_FLAGS) $< > $@


