# Name: Makefile
# Author: Swapneel Datta

# DEVICE ....... The AVR device you compile for
# CLOCK ........ Target AVR clock rate in Hertz
# OBJECTS ...... The object files created from your source files. This list is
#                usually the same as the list of source files with suffix ".o".
# PROGRAMMER ... Options to avrdude which define the hardware you use for
#                uploading to the AVR and the interface where this hardware
#                is connected.
# FUSES ........ Parameters for avrdude to flash the fuses appropriately.

######################## PROJECT AND PATHS ##############################
PROJECT      = BareMinimum
AVR_PATH     = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/bin
AVRDUDE_CONF = /opt/arduino-1.8.5-linux64/arduino-1.8.5/hardware/tools/avr/etc/avrdude.conf

############################### MCU #####################################
DEVICE       = atmega328p
CLOCK        = 16000000

############################# OBJECTS ###################################
PROGRAMMER   = -c arduino -C $(AVRDUDE_CONF) -P /dev/ttyUSB0 -b 57600 
OBJECTS      = $(PROJECT).o
FUSES        = -U lfuse:w:0x64:m -U hfuse:w:0xdd:m -U efuse:w:0xff:m

######################################################################

# Tune the lines below only if you know what you are doing:

AVRDUDE = $(AVR_PATH)/avrdude $(PROGRAMMER) -p $(DEVICE)
COMPILE = $(AVR_PATH)/avr-gcc -Wall -Os -DF_CPU=$(CLOCK) -mmcu=$(DEVICE)

# symbolic targets:
all:	$(PROJECT).hex

.c.o:
	$(COMPILE) -c $< -o $@

.S.o:
	$(COMPILE) -x assembler-with-cpp -c $< -o $@
# "-x assembler-with-cpp" should not be necessary since this is the default
# file type for the .S (with capital S) extension. However, upper case
# characters are not always preserved on Windows. To ensure WinAVR
# compatibility define the file type manually.

.c.s:
	$(COMPILE) -S $< -o $@

flash:	all
	$(AVRDUDE) -U flash:w:$(PROJECT).hex:i

fuse:
	$(AVRDUDE) $(FUSES)

install: flash fuse

# if you use a bootloader, change the command below appropriately:
load: all
	bootloadHID $(PROJECT).hex

clean:
	rm -f $(PROJECT).hex $(PROJECT).elf $(OBJECTS)

# file targets:
$(PROJECT).elf: $(OBJECTS)
	$(COMPILE) -o $(PROJECT).elf $(OBJECTS)

$(PROJECT).hex: $(PROJECT).elf
	rm -f $(PROJECT).hex
	$(AVR_PATH)/avr-objcopy -j .text -j .data -O ihex $(PROJECT).elf $(PROJECT).hex
# If you have an EEPROM section, you must also create a hex file for the
# EEPROM and add it to the "flash" target.

# Targets for code debugging and analysis:
disasm:	$(PROJECT).elf
	$(AVR_PATH)/avr-objdump -d $(PROJECT).elf

cpp:
	$(COMPILE) -E $(PEOJECT).c
