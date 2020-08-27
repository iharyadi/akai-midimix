# Web-MIDI wrapper for AKAI Midimix

This package makes it simple to access your AKAI Midimix's state in browser, using the web-midi API.


# Install

```npm install akai-midimix```

# Initialization
```javascript
import {MidiMix} from "akai-midimix";

const midi = new MidiMix();


midi.connect().then(() => {
    // run some commands right on startup (e.g. setting the toggle lights)
    console.log("Midi connected!");
    midi.m1 = true; // will light up the m1 button - see below for layout
});

midi.addEventListener("cc", (data) => {
    console.log("CC dial/slider turned", data);
});

midi.addEventListener("keydown", (data) => {
    console.log("Button press", data);
});
```

# Names for the buttons and sliders

The midi has 8 columns, each column having 3 dials, 2 buttons, and a slider.
Then, extra, there are the bank left, bank right, solo buttons, and the "master" slider.

To keep things simple, we are making use of the columns. Event data will contain
both, the original code (in keyCode field) and the symbolic field.
C stands for "continuous control", "m" for mute, "r" for rec, and "s" for slider.

```
c1    c2    c3    c4    c5    c6    c7    c8
c1a   c2a   c3a   c4a   c5a   c6a   c7a   c8a   bank_left
c1b   c2b   c3b   c4b   c5b   c6b   c7b   c8b   bank_right

m1    m2    m3    m4    m5    m6    m7    m8    solo
r1    r2    r3    r4    r5    r6    r7    r8


s1    s2    s3    s4    s5    s6    s7    s8    master
```


# Reading dial/slider states and setting the button lighs

Note: There seems to be no way to find out the initial state of the knobs when you connect to it. Luckily, that's what
the "Send All" hardware button is there for - it will generate change events for all the CC controls and the library
will have their state from there on out. Alternatively, the state for individual controls will be set when you
physically poke them.

With the caveat above in mind, to get the dial state, simply go `midi.c1` and so on.

All the buttons, with the exception of "Send All" and "Solo" have an LED that you can turn on.
Simply set the value to true/false accordingly to the button: `midi.bank_left = true`.



# Events

Use midi instance's `addEventListener(eventType, callback)` and `removeEventListener(eventType, callback)` to
subscribe to the events. There are 3 events:

* `cc` - fired on slider/dial turn. The event data is `{code, keyCode, val, prevVal}`
* `keydown` / 'keyup' - fired when any of the buttons are pressed (with the exception of send all).


# Licence & Thanks

This code is licenced under the MIT license,  so you can do with it whatever you want for whatever purpose.

There is barely any documentation out there.

This link was good to get rolling: https://webmidi-examples.glitch.me/

This google doc has reversed-engineered data on the messages, without which I wouldn't know how to turn the lights on.
https://docs.google.com/document/d/1zeRPklp_Mo_XzJZUKu2i-p1VgfBoUWF0JKZ5CzX8aB0/
