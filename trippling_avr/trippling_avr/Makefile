PROJECT=main
SOURCES=$(wildcard *.c)
MMCU=attiny25
F_CPU=8000000
ISPFLAGS=-c usbasp
CFLAGS=-mmcu=$(MMCU) -g -Wall -Os -std=c99 -DF_CPU=$(F_CPU)

$(PROJECT).hex: $(PROJECT).elf
	avr-objcopy -j .text -j .data -O ihex $(PROJECT).elf $(PROJECT).hex

$(PROJECT).elf: $(SOURCES)
	avr-gcc $(CFLAGS) -I. -o $(PROJECT).elf $(SOURCES)

program: $(PROJECT).hex
	avrdude $(ISPFLAGS) -p $(MMCU) -U -Uefuse:w:0xFF:m -Uhfuse:w:0xD7:m -Ulfuse:w:0xE2:m
	avrdude $(ISPFLAGS) -p $(MMCU) -U flash:w:$(PROJECT).hex

size: $(PROJECT).elf
	avr-size --mcu=$(MMCU) --format=avr $(PROJECT).elf

dump: $(PROJECT).elf
	avr-objdump -h -S -I. $(PROJECT).elf

clean:
	find . -depth -name '*.elf' -print -delete
	find . -depth -name '*.hex' -print -delete
	find . -depth -name '*.o'   -print -delete
	find . -depth -name '*.pyc' -print -delete

all: clean $(PROJECT).hex program
