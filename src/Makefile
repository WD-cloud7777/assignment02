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
# Simple makefile for multitarget build system
#
# Use:			make [TARGET] [PLATFORM=OVERRIDES]
#
# Build Targets:
#	<FILE>.i	- Generate preprocessed output <FILE>.i of <FILE>.c
#	<FILE>.s	- Generate assembly <FILE>.s of <FILE>.c
#	<FILE>.asm	- Generate assembly output <FILE>.asm of <FILE>.o / <FILE>.out using the GNU "objdump" utility
#	<FILE>.o	- Compile <FILE>.o object file

#	build		- Build all objects files, i.e., compile and link them into a final executable 
#	compile-all	- Compile all objects files but DO NOT link them
#	run		- Run output executable file
#	clean		- Remove all generated files
#
# Platform Overrides:
#	HOST		- host embedded system
#	MSP432		- cortex-m4
#
#------------------------------------------------------------------------------

include sources.mk

# Compiler Flags
CFLAGS = -Wall -Werror -g -O0 -std=c99

# Flags for GNU "Objdump" utility
OBJDUMP_FLAGS = -d -S

# Flags for GNU "Size" utility
SIZE_FLAGS = -Btd

ifeq ($(PLATFORM),HOST)

# C Compiler
CC = gcc

# C Preprocessor Flags
CPPFLAGS = -DHOST $(INCLUDES)

# Link Editor
LD = $(CC)

# Link Editor Flags
LDFLAGS=-Wl,-Map=$(BASENAME).map

# Objdump Utility
OBJDUMP = objdump

# Size Utility
SIZE = size

BASENAME = c1m2
TARGET   = $(BASENAME).out

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

%.s : %.i
	$(CC) $(CPPFLAGS) -S $< -o $@

%.o : %.s
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

%.asm : %.o
	$(OBJDUMP) $(OBJDUMP_FLAGS) $< > $@

%.asm : %.out
	$(OBJDUMP) $(OBJDUMP_FLAGS) $< > $@

.PHONY: build
build: build_msg $(TARGET)

.PHONY: build_msg
build_msg:
	@printf "\n#\n# Building <$(TARGET)>\n#\n\n"

$(TARGET): $(OBJS)
	$(LD) -o $@ $^ $(LDFLAGS)
	$(SIZE)	$(SIZE_FLAGS) $(TARGET) $(OBJS)

.PHONY: compile-all
compile-all: compile-all_msg $(OBJS)

.PHONY: compile-all_msg
compile_msg:
	@printf "\n#\n# Compiling <$(OBJS)>\n#\n"

.PHONY: run
run:
	./$(TARGET)

.PHONY: clean
clean:
	rm -f $(DEPS) $(PREPROCS) $(ASSEMBS) $(OBJS) $(DISASSEMBS)
	rm -f $(TARGET) $(BASENAME).map

# "include" directive (hyphen "-" included to suppress "No such file or directory" warning messages)
-include $(DEPS)

# Pattern rule and command script for automatically generating dependencies
# (taken from the GNU make manual)
%d: %c
	$(CC) -M $(CPPFLAGS) $< > $@.$$$$;                    \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@;   \
	rm -f $@.$$$$ 
