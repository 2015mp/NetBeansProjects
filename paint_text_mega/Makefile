COM_PORT = COM5
BAUD_RATE = 115200
ARDUINO_VERSION = 167
ARDUINO_BASE_DIR = C:/arduino
INCLUDE_LIBS = Firmata;
INCLUDE_EXTLIBS = Adafruit-GFX-Library;SWTFT-Shield;Touch-Screen-Library;NewliquidCrystal;
INCLUDE_BASELIBS = Wire;Wire/utility;
#ARDUINO_MODEL = atmega2560
#ARDUINO_PROGRAMMER = arduino
#ARDUINO_PINS_DIR = ${ARDUINO_BASE_DIR}/hardware/arduino/avr/variants/standard

# Arduino NetBeans plugin: https://github.com/jaquesclaudino/arduino-netbeans

#COM_PORT = /dev/ttyACM0
#BAUD_RATE = 115200
#ARDUINO_VERSION = 167
#ARDUINO_BASE_DIR = /home/jaques/opt/arduino-1.6.7
#INCLUDE_LIBS=Firmata;../hardware/arduino/avr/libraries/SoftwareSerial;

# Arduino Uno:
#ARDUINO_MODEL = atmega328p
#ARDUINO_PROGRAMMER = arduino
#ARDUINO_PINS_DIR = ${ARDUINO_BASE_DIR}/hardware/arduino/avr/variants/standard

# Arduino Mega 2560:
ARDUINO_MODEL = atmega2560
ARDUINO_PROGRAMMER = wiring
ARDUINO_PINS_DIR = ${ARDUINO_BASE_DIR}/hardware/arduino/avr/variants/mega

# Arduino Duemilanove:
#BAUD_RATE = 57600

############################# END OF USER CHANGES #############################

ARDUINO_CORE_DIR = ${ARDUINO_BASE_DIR}/hardware/arduino/avr/cores/arduino
ARDUINO_LIB_DIR = ${ARDUINO_BASE_DIR}/libraries
ARDUINO_BASELIBS_DIR= ${ARDUINO_BASE_DIR}/hardware/arduino/avr/libraries
ARDUINO_EXTLIB_DIR = C:/Users/mp/Documents/Arduino/libraries
LIB_CORE_DIR = lib/core
LIB_LIBS_DIR = lib/libs
LIB_EXTLIBS_DIR = lib/ext
LIB_BASE_DIR = lib/base
ARDUINO_LIB_CORE = ${LIB_CORE_DIR}/arduinocore.a
ARDUINO_LIB_LIBS = ${LIB_LIBS_DIR}/arduinolibs.a
ARDUINO_LIB_EXTLIBS = ${LIB_EXTLIBS_DIR}/arduinoext.a
ARDUINO_LIB_BASE = ${LIB_BASE_DIR}/arduinobase.a
AVR_DUDE = ${ARDUINO_BASE_DIR}/hardware/tools/avr/bin/avrdude -C ${ARDUINO_BASE_DIR}/hardware/tools/avr/etc/avrdude.conf
	
INCLUDE = -c -g \
-I${ARDUINO_CORE_DIR} \
-I${ARDUINO_PINS_DIR} \
$(patsubst %,-I${ARDUINO_LIB_DIR}/%,$(subst ;, ,$(INCLUDE_LIBS))) \
$(patsubst %,-I${ARDUINO_EXTLIB_DIR}/%,$(subst ;, ,$(INCLUDE_EXTLIBS))) \
$(patsubst %,-I${ARDUINO_BASELIBS_DIR}/%,$(subst ;, ,$(INCLUDE_BASELIBS)))

FLAGS_GCC = -c -g -Os -Wall -ffunction-sections -fdata-sections -mmcu=${ARDUINO_MODEL} -DF_CPU=16000000L -MMD -DUSB_VID=null -DUSB_PID=null -DARDUINO=${ARDUINO_VERSION}
FLAGS_GPP = ${FLAGS_GCC} -fno-exceptions
FLAGS_LINKER = ${ARDUINO_LIB_CORE} ${ARDUINO_LIB_LIBS} $(ARDUINO_LIB_EXTLIBS) $(ARDUINO_LIB_BASE) -Os -Wl,--gc-sections,--relax -mmcu=${ARDUINO_MODEL} -lm
CMD_AVR_GCC = avr-gcc ${FLAGS_GCC} ${INCLUDE}
CMD_AVR_GPP = avr-g++ ${FLAGS_GPP} ${INCLUDE}
CMD_AVR_AR = avr-ar rcs

CORE_CPP_SOURCES = $(wildcard ${ARDUINO_CORE_DIR}/*.cpp)
CORE_C_SOURCES = $(wildcard ${ARDUINO_CORE_DIR}/*.c)
COBJECTS=$(CORE_CPP_SOURCES:.cpp=.cpp.o) $(CORE_C_SOURCES:.c=.c.o)
CORE_OBJECTS=$(subst ${ARDUINO_CORE_DIR},${LIB_CORE_DIR},${COBJECTS})

LIB_CPP_SOURCES = $(wildcard $(patsubst %,${ARDUINO_LIB_DIR}/%/*.cpp,$(subst ;, ,$(INCLUDE_LIBS))))
LIB_CPP_EXT_SOURCES = $(wildcard $(patsubst %,${ARDUINO_EXTLIB_DIR}/%/*.cpp,$(subst ;, ,$(INCLUDE_EXTLIBS))))
LIB_CPP_BASE_SOURCES = $(wildcard $(patsubst %,${ARDUINO_BASELIBS_DIR}/%/*.cpp,$(subst ;, ,$(INCLUDE_BASELIBS))))
LIB_C_SOURCES = $(wildcard $(patsubst %,${ARDUINO_LIB_DIR}/%/*.c,$(subst ;, ,$(INCLUDE_LIBS))) )
LIB_C_EXT_SOURCES = $(wildcard $(patsubst %,${ARDUINO_EXTLIB_DIR}/%/*.c,$(subst ;, ,$(INCLUDE_EXTLIBS))) )
LIB_C_BASE_SOURCES = $(wildcard $(patsubst %,${ARDUINO_BASELIBS_DIR}/%/*.c,$(subst ;, ,$(INCLUDE_BASELIBS))) )
LOBJECTS=$(LIB_CPP_SOURCES:.cpp=.cpp.o) $(LIB_C_SOURCES:.c=.c.o)
LOBJECTS_EXT=$(LIB_CPP_EXT_SOURCES:.cpp=.cpp.o) $(LIB_C_EXT_SOURCES:.c=.c.o)
LOBJECTS_BASE=$(LIB_CPP_BASE_SOURCES:.cpp=.cpp.o) $(LIB_C_BASE_SOURCES:.c=.c.o)
LIB_OBJECTS=$(subst ${ARDUINO_LIB_DIR},${LIB_LIBS_DIR},${LOBJECTS})
LIB_EXT_OBJECTS=$(subst ${ARDUINO_EXTLIB_DIR},${LIB_EXTLIBS_DIR},${LOBJECTS_EXT})
LIB_BASE_OBJECTS=$(subst ${ARDUINO_BASELIBS_DIR},${LIB_BASE_DIR},${LOBJECTS_BASE})

# Environment 
MKDIR=mkdir
CP=cp
CCADMIN=CCadmin

# build
build: .build-post	
${ARDUINO_LIB_CORE}: ${CORE_OBJECTS}
	${CMD_AVR_AR} ${ARDUINO_LIB_CORE} ${CORE_OBJECTS}
	
${ARDUINO_LIB_LIBS}: ${LIB_OBJECTS} ${CORE_OBJECTS}
	${CMD_AVR_AR} ${ARDUINO_LIB_LIBS} ${CORE_OBJECTS} ${LIB_OBJECTS}

${ARDUINO_LIB_EXTLIBS}: ${LIB_EXT_OBJECTS} ${CORE_OBJECTS}
	${CMD_AVR_AR} ${ARDUINO_LIB_EXTLIBS} ${CORE_OBJECTS} ${LIB_EXT_OBJECTS}

${ARDUINO_LIB_BASE}: ${LIB_BASE_OBJECTS} ${CORE_OBJECTS}
	${CMD_AVR_AR} ${ARDUINO_LIB_BASE} ${CORE_OBJECTS} ${LIB_BASE_OBJECTS}
	
libraries: ${ARDUINO_LIB_CORE} $(ARDUINO_LIB_EXTLIBS) $(ARDUINO_LIB_BASE) ${ARDUINO_LIB_LIBS}

${LIB_CORE_DIR}/%.cpp.o: ${ARDUINO_CORE_DIR}/%.cpp
	mkdir -p $(dir $@)
	${CMD_AVR_GPP} $< -o $@

${LIB_CORE_DIR}/%.c.o: ${ARDUINO_CORE_DIR}/%.c
	mkdir -p $(dir $@)
	${CMD_AVR_GCC} $< -o $@

${LIB_BASE_DIR}/%.cpp.o: ${ARDUINO_BASELIBS_DIR}/%.cpp
	mkdir -p $(dir $@)
	${CMD_AVR_GPP} $< -o $@

${LIB_BASE_DIR}/%.c.o: ${ARDUINO_BASELIBS_DIR}/%.c
	mkdir -p $(dir $@)
	${CMD_AVR_GCC} $< -o $@

${LIB_EXTLIBS_DIR}/%.cpp.o: ${ARDUINO_EXTLIB_DIR}/%.cpp
	mkdir -p $(dir $@)
	${CMD_AVR_GPP} $< -o $@

${LIB_EXTLIBS_DIR}/%.c.o: ${ARDUINO_EXTLIB_DIR}/%.c
	mkdir -p $(dir $@)
	${CMD_AVR_GCC} $< -o $@

${LIB_LIBS_DIR}/%.cpp.o: ${ARDUINO_LIB_DIR}/%.cpp  
	mkdir -p $(dir $@)
	${CMD_AVR_GPP} $< -o $@

${LIB_LIBS_DIR}/%.c.o: ${ARDUINO_LIB_DIR}/%.c
	mkdir -p $(dir $@)
	${CMD_AVR_GCC} $< -o $@

.build-pre: .build-pre-pre libraries

#Unconditional Build
.build-pre-pre:
ifeq "$(wildcard $(ARDUINO_BASE_DIR))" ""
	echo "Folder $(ARDUINO_BASE_DIR) not found. Please fix it on Makefile. Remember the path can not contain spaces."
	test -d ${ARDUINO_BASE_DIR}
endif
	
.build-post: .build-impl	
	avr-objcopy -O ihex ${CND_ARTIFACT_PATH_${CONF}} ${CND_ARTIFACT_PATH_${CONF}}.hex
	avr-size --mcu=${ARDUINO_MODEL} -C ${CND_ARTIFACT_PATH_${CONF}}

#upload: .build-post
upload: 
	${AVR_DUDE} -v -p${ARDUINO_MODEL} -c${ARDUINO_PROGRAMMER} -P ${COM_PORT} -b${BAUD_RATE} -D -Uflash:w:${CND_ARTIFACT_PATH_${CONF}}.hex:i


# clean
clean: .clean-post

.clean-pre:
# Add your pre 'clean' code here...

.clean-post: .clean-impl
# Add your post 'clean' code here...


# clobber
clobber: .clobber-post

.clobber-pre:
# Add your pre 'clobber' code here...

.clobber-post: .clobber-impl
# Add your post 'clobber' code here...


# all
all: .all-post

.all-pre:
# Add your pre 'all' code here...

.all-post: .all-impl
# Add your post 'all' code here...


# build tests
build-tests: .build-tests-post

.build-tests-pre:
# Add your pre 'build-tests' code here...

.build-tests-post: .build-tests-impl
# Add your post 'build-tests' code here...


# run tests
test: .test-post

.test-pre: build-tests
# Add your pre 'test' code here...

.test-post: .test-impl
# Add your post 'test' code here...


# help
help: .help-post

.help-pre:
# Add your pre 'help' code here...

.help-post: .help-impl
# Add your post 'help' code here...


# include project implementation makefile
include nbproject/Makefile-impl.mk

# include project make variables
include nbproject/Makefile-variables.mk

