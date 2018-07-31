# Makefile for AVR C++ projects

# ----- Update the settings of your project here -----
# Hardware
MCU     = atmega328p # see `make show-mcu`
OSC     = 16000000UL
PROJECT = BareMinimum

# ----- These configurations are quite likely not to be changed -----

# Binaries
GCC     = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/bin/avr-gcc
G++     = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/bin/avr-g++
RM      = rm -f
AVRDUDE = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/bin/avrdude
OBJCOPY = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/bin/avr-objcopy

# Define extensions for C, C++ and assembly
EXT_C   = c
EXT_C++ = cpp
EXT_ASM = asm

# ----- No changes should be necessary below this line -----

OBJECTS      = \
		$(patsubst %.$(EXT_C),%.o,$(wildcard *.$(EXT_C))) \
		$(patsubst %.$(EXT_C++),%.o,$(wildcard *.$(EXT_C++))) \
		$(patsubst %.$(EXT_ASM),%.o,$(wildcard *.$(EXT_ASM)))

AVRDUDE_CONF = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/etc/avrdude.conf

################ Flags for C ###############
CFLAGS = $(INC)
CFLAGS += -Os
CFLAGS += -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums
CFLAGS += -Wall -Wstrict-prototypes
CFLAGS += -DF_OSC=$(OSC)
CFLAGS += -mmcu=$(MCU)

############## Flags for C++ ###############
C++FLAGS = $(INC)
C++FLAGS += -Os
C++FLAGS += -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums
C++FLAGS += -Wall
C++FLAGS += -DF_OSC=$(OSC)
C++FLAGS += -mmcu=$(MCU)

############ Flags for Assembly ############
ASMFLAGS = $(INC)
ASMFLAGS += -Os
ASMFLAGS += -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums
ASMFLAGS += -Wall -Wstrict-prototypes
ASMFLAGS += -DF_OSC=$(OSC)
ASMFLAGS += -x assembler-with-cpp
ASMFLAGS += -mmcu=$(MCU)

########### Flags for flashing ############
BURNFLAGS = $(INC)
BURNFLAGS += -c arduino -C $(AVRDUDE_CONF) 
BURNFLAGS += -P /dev/ttyUSB0 -b 57600
BURNFLAGS += -p $(MCU)

###########################################
default: $(PROJECT).elf
	echo $(OBJECTS)

%.elf: $(OBJECTS)
	$(GCC) $(CFLAGS) $(OBJECTS) --output $@ $(LDFLAGS)

%.o : %.$(EXT_C)
	$(GCC) $< $(CFLAGS) -c -o $@

%.o : %.$(EXT_C++)
	$(G++) $< $(C++FLAGS) -c -o $@

%.o : %.$(EXT_ASM)
	$(G++) $< $(ASMFLAGS) -c -o $@

clean:
	$(RM) $(PROJECT).elf $(PROJECT).hex $(OBJECTS)

help:
	@echo "usage:"
	@echo "  make <target>"
	@echo ""
	@echo "targets:"
	@echo "  clean     Remove any non-source files"
	@echo "  config    Shows the current configuration"
	@echo "  help      Shows this help"
	@echo "  show-mcu  Show list of all possible MCUs"

config:
	@echo "configuration:"
	@echo ""
	@echo "Binaries for:"
	@echo "  C compiler:   $(GCC)"
	@echo "  C++ compiler: $(G++)"
	@echo "  Programmer:   $(AVRDUDE)"
	@echo "  remove file:  $(RM)"
	@echo ""
	@echo "Hardware settings:"
	@echo "  MCU: $(MCU)"
	@echo "  OSC: $(OSC)"
	@echo ""
	@echo "Defaults:"
	@echo "  C-files:   *.$(EXT_C)"
	@echo "  C++-files: *.$(EXT_C++)"
	@echo "  ASM-files: *.$(EXT_ASM)"

show-mcu:
	$(G++) --help=target

hexify: default
	$(OBJCOPY) -j .text -j .data -O ihex $(PROJECT).elf $(PROJECT).hex

flash: hexify
	$(AVRDUDE) $(BURNFLAGS) -U flash:w:$(PROJECT).hex:i
