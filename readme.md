     / _____)             _              | |    
    ( (____  _____ ____ _| |_ _____  ____| |__  
     \____ \| ___ |    (_   _) ___ |/ ___)  _ \ 
     _____) ) ____| | | || |_| ____( (___| | | |
    (______/|_____)_|_|_| \__)_____)\____)_| |_|
        (C)2013 Semtech

SX1272/76 radio drivers plus Ping-Pong firmware and LoRa MAC node firmware implementation.
=====================================

1. Introduction
----------------
The aim of this project is to show an example of the LoRaMac endpoint firmware
implementation.

**REMARK 1:** Next version of the project will include big changes.

*This is the last version based on the Semtech LoRaMac implementation. The next
version will be based on the [IBM 'LoRaWAN in C'](http://www.research.ibm.com/labs/zurich/ics/lrsc/lmic.html)
implementation.*

*The IBM 'LoRaWAN in C' implementation adds the support of the Class A endpoint
fully implemented and Class B endpoints.*

*The biggest change resides on the MAC layer API which is completely different.*

**REMARK 2:** This is a Class A endpoint.

**REMARK 3:** Implements version R3.0 of LoRaMac specification.

*By default the LORAMAC_R3 compiler option is enabled. Disabling this option will enable LoRaMac specification R2.2.1*

2. System schematic and definitions
------------------------------------
The available supported hardware platforms schematics and LoRaMac specification
can be found in the Doc directory.

3. Acknowledgements
-------------------
The mbed (https://mbed.org/) project was used at the beginning as source of 
inspiration.

This program uses the AES algorithm implementation (http://www.gladman.me.uk/)
by Brian Gladman.

This program uses the CMAC algorithm implementation 
(http://www.cse.chalmers.se/research/group/dcs/masters/contikisec/) by 
Lander Casado, Philippas Tsigas.

4. Dependencies
----------------
This program depends on specific hardware platforms. Currently the supported 
platforms are:

    - Bleeper-72
        MCU     : STM32L151RD - 384K FLASH, 48K RAM, Timers, SPI, I2C,
                                USART, 
                                USB 2.0 full-speed device/host/OTG controller,
                                DAC, ADC, DMA
        RADIO   : SX1272
        ANTENNA : Connector for external antenna
        BUTTONS : 1 Reset, 16 position encoder
        LEDS    : 3
        SENSORS : Temperature
        GPS     : Possible through pin header GPS module connection
        SDCARD  : Yes
        EXTENSION HEADER : Yes, 12 pins
        REMARK  : None.

    - Bleeper-76
        MCU     : STM32L151RD - 384K FLASH, 48K RAM, Timers, SPI, I2C,
                                USART, 
                                USB 2.0 full-speed device/host/OTG controller,
                                DAC, ADC, DMA
        RADIO   : SX1276
        ANTENNA : Connector for external antennas (LF+HF)
        BUTTONS : 1 Reset, 16 position encoder
        LEDS    : 3
        SENSORS : Temperature
        GPS     : Possible through pin header GPS module connection
        SDCARD  : No
        EXTENSION HEADER : Yes, 12 pins
        REMARK  : None.

    - LoRaMote
        MCU     : STM32L151CB - 128K FLASH, 10K RAM, Timers, SPI, I2C,
                                USART, 
                                USB 2.0 full-speed device/host/OTG controller,
                                DAC, ADC, DMA
        RADIO   : SX1272
        ANTENNA : Printed circuit antenna
        BUTTONS : No
        LEDS    : 3
        SENSORS : Proximity, Magnetic, 3 axis Accelerometer, Pressure,
                  Temperature
        GPS     : Yes, UP501 module
        SDCARD  : No
        EXTENSION HEADER : Yes, 20 pins
        REMARK  : The MCU and Radio are on an IMST iM880A module

    - SensorNode
        MCU     : STM32L151C8 - 64K FLASH, 10K RAM, Timers, SPI, I2C,
                                USART, 
                                USB 2.0 full-speed device/host/OTG controller,
                                DAC, ADC, DMA
        RADIO   : SX1276
        ANTENNA : Printed circuit antenna
        BUTTONS : Power ON/OFF, General purpose button
        LEDS    : 3
        SENSORS : Proximity, Magnetic, 3 axis Accelerometer, Pressure,
                  Temperature
        GPS     : Yes, SIM39EA module
        SDCARD  : No
        EXTENSION No
        REMARK  : The MCU and Radio are on an NYMTEK Cherry-LCC module

    - SK-iM880A ( IMST starter kit )
        MCU     : STM32L151CB - 128K FLASH, 10K RAM, Timers, SPI, I2C,
                                USART, 
                                USB 2.0 full-speed device/host/OTG controller,
                                DAC, ADC, DMA
        RADIO   : SX1272
        ANTENNA : Connector for external antenna
        BUTTONS : 1 Reset, 3 buttons + 2 DIP-Switch
        LEDS    : 3
        SENSORS : Potentiometer
        GPS     : Possible through pin header GPS module connection
        SDCARD  : No
        EXTENSION HEADER : Yes, all IMST iM880A module pins
        REMARK  : None

5. Usage
---------
Projects for CooCox-CoIDE (partial), Ride7 and Keil Integrated Development Environments are available.

One project is available per application and for each hardware platform in each
development environment. Different targets/configurations have been created in
the different projects in order to select different options such as the usage or
not of a bootloader and the radio frequency band to be used.

6. Changelog
-------------
2015-01-30, v3.1
* General
    1. Started to add support for CooCox CoIDE Integrated Development Environment.
       Currently only LoRaMote and SensorNode platform projects are available.
    2. Updated GCC compiler linker scripts.
    3. Added the support of different tool chains for the HardFault_Handler function.

    4. Corrected Radio drivers I&Q signals inversion to be possible in Rx and in Tx.
       Added some missing radio state machine initialization.
    5. Changed the RSSI values type from int8_t to int16_t. We can have RSSI values below -128 dBm.
    6. Corrected SNR computation on RxDone interrupt.
    7. Updated radio API to support FHSS and CAD handling.
    8. Corrected in SetRxConfig function the FSK modem preamble register name.
    9. Added an invalid bandwidth to the Bandwidths table in order to avoid an error
       when selecting 250 kHz bandwidth when using FSK modem.

    10. Corrected RTC alarm setup which could be set to an invalid date.
    11. Added another timer in order increment the tick counter without blocking the normal timer count.
    12. Added the possibility to switch between low power timers and normal timers on the fly.
    13. I2C driver corrected the 2 bytes internal address management. 
        Corrected buffer read function when more that 1 byte was to be read.
        Added a function to wait for the I2C bus to become IDLE.
    14. Added an I2C EEPROM driver.
    15. Corrected and improved USB Virtual COM Port management files.
        Corrected the USB CDC and USB UART drivers.
    16. Added the possibility to analyze a hard fault interrupt.

* LoRaMac
    1. Corrected RxWindow2 Datarate management.
    2. SrvAckRequested variable was never reset.
    3. Corrected tstIndoor applications for LoRaMac R3.0 support.
    4. LoRaMac added the possibility to configure almost all the MAC parameters.
    5. Corrected the LoRaMacSetNextChannel function. 
    6. Corrected the port 0 MAC command decoding.
    7. Changed all structures declarations to be packed.
    8. Corrected the Acknowledgement retries management when only 1 trial is needed.
       Before the device was issuing at least 2 trials.
    9. Corrected server mac new channel req answer.
    10. Added the functions to read the Up and Down Link sequence counters.
    11. Corrected SRV_MAC_RX2_SETUP_REQ frequency handling. Added a 100 multiplication.
    12. Corrected SRV_MAC_NEW_CHANNEL_REQ. Removed the DutyCycle parameter decoding.
    13. Automatically activate the channel once it is created. 
    14. Corrected NbRepTimeoutTimer initial value. RxWindow2Delay already contains RxWindow1Delay in it.

2014-07-18, v3.0
* General
    1. Added to Radio API the possibility to select the modem.
    2. Corrected RSSI reading formulas as well as changed the RSSI and SNR values from double to int8_t type.
    3. Changed radio callbacks events to timeout when it is a timeout event and error when it is a CRC error.
    4. Radio API updated.
    5. Updated ping-pong applications.
    6. Updated tx-cw applications.
    7. Updated LoRaMac applications in order to handle LoRaMac returned functions calls status.
    8. Updated LoRaMac applications to toggle LED2 each time there is an application payload down link.
    9. Updated tstIndoor application to handle correctly more than 6 channels.
    10. Changed the MPL3115 altitude variable from unsigned to signed value.
    11. Replaced the usage of pow(2, n) by defining POW2 functions. Saves ~2 KBytes of code.
    12. Corrected an issue potentially arriving when LOW_POWER_MODE_ENABLE wasn't defined.
        A timer interrupt could be generated while the TimerList could already be emptied.


* LoRaMac
    1. Implemented LoRaMac specification R3.0 changes.

    2. MAC commands implemented
        * LinkCheckReq                       **YES**
        * LinkCheckAns                       **YES**
        * LinkADRReq                         **YES**
        * LinkADRAns                         **YES**
        * DutyCycleReq                       **YES**
        * DutyCycleAns                       **YES**
        * Rx2SetupReq                        **YES**
        * Rx2SetupAns                        **YES**
        * DevStatusReq                       **YES**
        * DevStatusAns                       **YES**
        * JoinReq                            **YES**
        * JoinAccept                         **YES**
        * NewChannelReq                      **YES**
        * NewChannelAns                      **YES**

    3. Features implemented
        * Possibility to shut-down the device **YES**

            Possible by issuing DutyCycleReq MAC command.
        * Duty cycle management enforcement  **NO**
        * Acknowledgements retries           **YES**
        * Unconfirmed messages retries       **YES**

2014-07-10, v2.3.RC2
* General
    1. Corrected all radios antenna switch low power mode handling.
    2. SX1276: Corrected antenna switch control.

2014-06-06, v2.3.RC1
* General
    1. Added the support for SX1276 radio.
    2. Radio continuous reception mode correction.
    3. Radio driver RxDone callback function API has changed ( size parameter is no more a pointer).
           Previous function prototype:
       
            void    ( *RxDone )( uint8_t *payload, uint16_t *size, double rssi, double snr, uint8_t rawSnr );
       
       New function prototype:
       
            void    ( *RxDone )( uint8_t *payload, uint16_t size, double rssi, double snr, uint8_t rawSnr );

    4. Added Bleeper-76 and SensorNode platforms support.
    5. Added to the radio drivers a function that generates a random value from
       RSSI readings.
    6. Added a project to transmit a continuous wave and a project to measure the
       the radio sensitivity.
    7. Added a bootloader project for the LoRaMote and SensorNode platforms.
    8. The LoRaMac application for Bleeper platforms now sends the Selector and LED status plus the sensors values.
        * The application payload for the Bleeper platforms is as follows:
        
            LoRaMac port 1:
            
                 { 0xX0/0xX1, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 }
                  ----------  ----------  ----------  ----------  ----
                       |           |           |           |        |
                 SELECTOR/LED  PRESSURE   TEMPERATURE  ALTITUDE  BATTERY
                 MSB nibble = SELECTOR               (barometric)
                 LSB bit    = LED
    9. Redefined rand() and srand() standard C functions. These functions are
       redefined in order to get the same behaviour across different compiler
       tool chains implementations.
    10. GPS driver improvements. Made independent of the board platform.
    11. Simplified the RTC management.
    12. Added a function to the timer driver that checks if a timer is already in
       the list or not.
    13. Added the UART Overrun bit exception handling to the UART driver.
    14. Removed dependency of spi-board files to the "__builtin_ffs" function. 
       This function is only available on GNU compiler tool suite. Removed --gnu
       compiler option from Keil projects. Added own __ffs function
       implementation to utilities.h file.
    15. Removed obsolete class1 devices support.
    16. Known bugs correction.

* LoRaMac
    1. MAC commands implemented
        * LinkCheckReq                       **YES**
        * LinkCheckAns                       **YES**
        * LinkADRReq                         **YES**
        * LinkADRAns                         **YES**
        * DutyCycleReq                       **YES** (LoRaMac specification R2.2.1)
        * DutyCycleAns                       **YES** (LoRaMac specification R2.2.1)
        * Rx2SetupReq                        **YES** (LoRaMac specification R2.2.1)
        * Rx2SetupAns                        **YES** (LoRaMac specification R2.2.1)
        * DevStatusReq                       **YES**
        * DevStatusAns                       **YES**
        * JoinReq                            **YES**
        * JoinAccept                         **YES** (LoRaMac specification R2.2.1)
        * NewChannelReq                      **YES** (LoRaMac specification R2.2.1)
        * NewChannelAns                      **YES** (LoRaMac specification R2.2.1)
    2. Features implemented
        * Possibility to shut-down the device **YES**
        
          Possible by issuing DutyCycleReq MAC command.
        * Duty cycle management enforcement  **NO**
        * Acknowledgements retries           **WORK IN PROGRESS**
        
          Not fully debugged. Disabled by default.
        * Unconfirmed messages retries       **WORK IN PROGRESS** (LoRaMac specification R2.2.1)
    3. Implemented LoRaMac specification R2.2.1 changes.
    4. Due to new specification the LoRaMacInitNwkIds LoRaMac API function had
       to be modified.
       
       Previous function prototype:
       
            void LoRaMacInitNwkIds( uint32_t devAddr, uint8_t *nwkSKey, uint8_t *appSKey );
       
       New function prototype:
       
            void LoRaMacInitNwkIds( uint32_t netID, uint32_t devAddr, uint8_t *nwkSKey, uint8_t *appSKey );
    5. Changed the LoRaMac channels management.
    6. LoRaMac channels definition has been moved to LoRaMac-board.h file
       located in each specific board directory.

2014-04-07, v2.2
* General
    1. Added IMST SK-iM880A starter kit board support to the project.
        * The application payload for the SK-iM880A platform is as follows:

        LoRaMac port 3:
        
             { 0x00/0x01, 0x00, 0x00, 0x00 }
              ----------  ----- ----------
                   |        |       |
                  LED     POTI     VDD
    2. Ping-Pong applications have been split per supported board.
    3. Corrected the SX1272 output power management. 
       Added a variable to store the current Radio channel.
       Added missing FSK bit definition.
    4. Made fifo functions coding style coherent with the project.
    5. UART driver is now independent of the used MCU
    
2014-03-28, v2.1
* General
    1. The timers and RTC management has been rewritten.
    2. Improved the UART and UP501 GPS drivers.
    3. Corrected GPIO pin names management.
    4. Corrected the antenna switch management in the SX1272 driver.
    5. Added to the radio driver the possibility to choose the preamble length
       and rxSingle symbol timeout in reception.
    6. Added Hex coder selector driver for the Bleeper board.
    7. Changed copyright Unicode character to (C) in all source files.
    
* LoRaMac
    1. MAC commands implemented
        * LinkCheckReq                 **YES**
        * LinkCheckAns                 **YES**
        * LinkADRReq                   **YES**
        * LinkADRAns                   **YES**
        * DevStatusReq                 **YES**
        * DevStatusAns                 **YES**
        * JoinReq                      **YES**
        * JoinAccept                   **YES**
    2. Added acknowledgements retries management.
      Split the LoRaMacSendOnChannel function in LoRaMacPrepareFrame and
      LoRaMacSendFrameOnChannel. LoRaMacSendOnChannel now calls the 2 newly
      defined functions.
    
      **WARNING**: By default the acknowledgement retries specific code isn't
      enabled. The current http://iot.semtech.com server version doesn't support
      it.
      
    3. Corrected issues on JoinRequest and JoinAccept MAC commands.
      Added LORAMAC_EVENT_INFO_STATUS_MAC_ERROR event info status.

2014-02-21, v2.0

* General
    1. The LoRaMac applications now sends the LED status plus the sensors values.
       For the LoRaMote platform the application also sends the GPS coordinates.
        * The application payload for the Bleeper platform is as follows:
        
            LoRaMac port 1:
            
                 { 0x00/0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 }
                  ----------  ----------  ----------  ----------  ----
                       |           |           |           |        |
                      LED      PRESSURE   TEMPERATURE  ALTITUDE  BATTERY
                                                     (barometric)
        * The application payload for the LoRaMote platform is as follows:
        
            LoRaMac port 2:
            
                 { 0x00/0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 }
                  ----------  ----------  ----------  ----------  ----  ----------------  ----------------  ----------
                       |           |           |           |        |           |                 |              |
                      LED      PRESSURE   TEMPERATURE  ALTITUDE  BATTERY    LATITUDE          LONGITUDE      ALTITUDE 
                                                     (barometric)                                              (gps)
    2. Adapted applications to the new MAC layer API.
    3. Added sensors drivers implementation.
    4. Corrected new or known issues.
* LoRaMac
    1. MAC commands implemented
        * LinkCheckReq                 **YES**
        * LinkCheckAns                 **YES**
        * LinkADRReq                   **YES**
        * LinkADRAns                   **YES**
        * DevStatusReq                 **YES**
        * DevStatusAns                 **YES**
        * JoinReq                      **YES (Not tested)**
        * JoinAccept                   **YES (Not tested)**
    2. New MAC layer application API implementation.
* Timers and RTC.
    1. Still some issues. They will be corrected on next revisions of the firmware.

2014-01-24, v1.1

* LoRaMac
    1. MAC commands implemented
        * LinkCheckReq                 **NO**
        * LinkCheckAns                 **NO**
        * LinkADRReq                   **YES**
        * LinkADRAns                   **YES**
        * DevStatusReq                 **YES**
        * DevStatusAns                 **YES**
    2. Implemented an application LED control
        If the server sends on port 1 an application payload of one byte with
        the following contents:
        
            0: LED off
            1: LED on
        The node transmits periodically on port 1 the LED status on 1st byte and
        the message "Hello World!!!!" the array looks like:
        
            { 0, 'H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd', '!', '!', '!', '!' }
* Timers and RTC.
    1. Corrected issues existing in the previous version
    2. Known bugs are: 
        * There is an issue when launching an asynchronous Timer. Will be solved
          in a future version 

2014-01-20, v1.1.RC1

* Added Doc directory. The directory contains:
    1. LoRa MAC specification
    2. Bleeper board schematic
* LoRaMac has been updated according to Release1 of the specification. Main changes are:
    1. MAC API changed.
    2. Frame format.
    3. ClassA first ADR implementation.
    4. MAC commands implemented
        * LinkCheckReq              **NO**
        * LinkCheckAns              **NO**
        * LinkADRReq                **YES**
        * LinkADRAns                **NO**
        * DevStatusReq              **NO**
        * DevStatusAns              **NO**
    
* Timers and RTC rewriting. Known bugs are: 
    1. The Radio wakeup time is taken in account for all timings. 
    2. When opening the second reception window the microcontroller sometimes doesn't enter in low power mode.

2013-11-28, v1.0

* Initial version of the LoRa MAC node firmware implementation.
