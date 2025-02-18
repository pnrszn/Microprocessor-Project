CENG329PROJECT
 GROUPMEMBERS:
 ● Pınar Süzen 202111043
 ● Şevval Kaymazlı 202211039
 Microprocessor Number: I31
 YOUTUBELINKTOTHEPROJECTVIDEO:
 https://youtu.be/jumWhcVw1Zk?si=jS4iXix0Wa6Zqh2f

EXPLANATION
Main Loop:
 Reset: Interrupt flags are cleared, the ports to be utilized in the program are initialized, the
       stack pointer is reset to the starting position, the watchdog timer is being stopped, and all
       lights are turned off. Ports P1.1, P1.2, P1.3, P1.4, P1.5, P1.7, P2.0, and P2.2 are configured as
       digital I/O, while P1.1, P1.2, P1.4, P1.5, P1.7, P2.0, and P2.2 are set as output ports and P1.3
       is configured as an input. Additionally, P2.4 and P2.5 are enabled in order to be used as
       buttons, whereas P2.3 and P2.7 are enabled in order to be used as lights. Pull-up resistors are
       utilized for the button ports, whereas pull-down resistors are used for the light ports. Finally,
       all segments are turned off, and the register R6, used to identify the dash, is initialized to
       zero.
 Three: Firstly, all segments are turned off. Then the necessary segments (a, b, c, d, g) are
        turned on so that the number "3" is shown on the 7-segment display. After that, “Delay”
        subroutine is called to create an approximately 1 second delay between numbers.
 Two:   Firstly, all segments are turned off. Then the necessary segments (a, b, d, e, g) are turned
       on so that the number "2" is shown on the 7-segment display. After that, “Delay” subroutine
       is called to create an approximately 1 second delay between numbers.
 One:  Firstly, all segments are turned off. Then the necessary segments (b, c) are turned on so
       that the number "1" is shown on the 7-segment display. After that, “Delay” subroutine is
       called to create an approximately 1 second delay between numbers.
 Zero: Firstly, all segments are turned off. Then the necessary segments (a, b, c, d, e, f) are
       turned on so that the number "0" is shown on the 7-segment display. After that, “Delay”
       subroutine is called to create an approximately 1 second delay between numbers.
 Dash: Firstly, all segments are turned off. Then the necessary segment (g) is turned on so that
       the “-” symbol is shown on the 7-segment display. After that, a number is assigned to the
       register R6 to identify the game state later in the “but_ISR” interrupt. Then the “Check”
       subroutine is called to determine whether one of the players pressed their buttons designated
       to light up the leds or not. “-” symbol will be displayed until one of the players presses their
       button.   
Check: First of all, the program waits for 3 seconds. After the three-second delay, if neither
       player presses any button or the button has not been pressed, the program remains in a
       standby state while displaying the symbol “-”, awaiting a press signal to start an another
       round by going back to the “RESET” label.
Reset_button: The program transitions to this label through an interrupt triggered by the
       designated port P1.3, which is the button on the MSP430 microprocessor, within the Port 1.
But_ISR: The program transitions to this label through an interrupt triggered by the
       designated ports P2.5 or P2.4, which is for the buttons designated to control the leds, within
       the Port 2. After that, if the register R6 is not equal to 5, indicating that the program is not at
       the “Dash” and the game is not finished yet, the program transitions to the label “Fail.”
       Conversely, if R6 equals 5, the program proceeds to execute and transitions to the label
       “Success.”
Success: The program checks the button ports to determine which button is pressed (the
       program considers only the first pressed button), then turns on the led connected to the
       pressed button.
Fail: The program checks the button ports to determine which button is pressed (the program
       considers only the first pressed button), then turns on the opponent of the defeated player’s
       led.
Green_on: Green led is turned on. Then the program jumps to the “Dash” label. 
Yellow_on: Yellow led is turned on. Then the program jumps to the “Dash” label.
Delay: This subroutine enables the program to wait for a specified duration, determined by a
       pre-initialized value stored in the register R5.
Interrupt Vectors:
       .sect ".int03"
       .short But_ISR : Specifies the address of the ISR for Port 2 interrupts. When a Port 2
       interrupt is triggered, the microcontroller jumps to the “But_ISR” label.
       .sect ".int02"
       .short Reset_button : Specifies the address of the ISR for Port 1 interrupts. When a Port 1
       interrupt is triggered, the microcontroller jumps to the “Reset_button” label.
       .sect ".reset"
       .short RESET : Specifies the address of the program's main entry point labeled “RESET”.
