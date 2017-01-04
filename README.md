# DeckLEDDriver
High Amp RGB LED Driver with pot and webpage control


LED Driver program
  v0.1 Terry Myers EZSoft-inc.com

Functional Requirements:
  1. Control a string of common anode RGB LEDs mounted underneath the railings of a deck via a dimmer switch or webpage
  2. Control a string of white LEDs mounted underneath a deck via a dimmer switch or motion detection

Detailed Design
  A server grade 12VDC nom. power supply supplies a rail of 12V to the arduino which has an ethernet shield and some protoshields for wiring
    This power supply has a 2A 12V rail that is always on and an input that when sinked to ground turns on a 60A rail which will power the LEDs
    The arduino can control this input via a transistor.  A SPDT with off toggle switch on the case allows the user to enable the arduino to control the power, or turn power on, or off.
  The arduino hosts a basic webpage that allows the user to control the lighting on the Deck, and allows access to some basic "cool" programs
  If the dimmer switch is moved, the RGB Deck LEDs are dimmed white according to the dimmer position.  The dimmer always makes the RGB white
    The RGB LEDs are color corrected to 3000K by capping the max value of the green and blue LED
    Dimmer switches use a voltage dividor.  The dimmer switch inputs are linearlized
    LED perceived brightness is nto linear with power, there he overall LED brightness is linearized to human perception with a gamma correction algorythm.
    Both dimmer switch analog inputs are heavily smoothed with a low pass filter.
  The Under Deck LEDs are just white LEDs and can only be controlled via an identical linearized dimmer switch
    Again the dimmer switch is linearized and the brightness is linearized to human perception.
    Generic 12V powered motion sensors (3 wire: Power, ground, relay switched 12V) are used to sense motion.
      The relay switched signal is passed through an LM7805 as a kind of "relay" of sorts as an input to the arduino.  An actual mechical relay should be used, but I didn't have one at the time of build.
    If motion is sensed the under deck LEDs will fade on for a preprogrammed time, then fade off.
  time.gov is polled once a day and an acurate time is maintained in a register.
    The motion sensor inputs are disabled during daylight according to monthly averages of civil twilight hours around Philadelphia PA
  All LEDs have a timeout timer if left on for more than 3 hours they will turn off
  All LEDs will turn off at midnigth on a one shot.