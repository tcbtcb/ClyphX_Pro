# Clyph X stuff

## what I wanted to accomplish

I make dub, dub techno, dub house, dub, dub gypsy, dub acoustic, dub
tennis, and just plain dub. So the primary thing I wanted, the most
important thing by far was a 'dub channel' that I could use essentially
as a pre fader send bus for tracks. Search youtube for 'Scientist stop
and frisk dub' for an idea of how the bus works. 

After the dub channel and the routing, some extra bells and whistles like
editing the macros on a device and some 'standard' mappings would also
be helpful but not necessary.

For reference I'm attaching a screen cap of the fader box I'm using. 

## initial problems with mapping

In an ideal world I would not have changed a single mapping on the
device. That is, ideally I could swap out another platform M and just
turn it on and evertyhing worked. I didn't find that possible for a few
reasons. Mackie Control mode used pitchbend on different channels for
the faders. So PB 1 for fader one, PB 2 for fader 2 and so on. That was
a non starter. Mackie HUI mode was even more annoying, sending strante
low level MIDI messages that were incredibly annoying to work with. 

As a fallback I decided to take their Mackie Control Mode and just
change the faders. So evertyhing is transmitting on channel 1. All
buttons and knobs are unchanged. The faders are set to CC 0-8 since
there are nine faders on the device. 

After working on the first draft of the mappings for a while, I found it
easist to separate the jobs of the various files, just for my own
sanity. The Button/Encoder files map the devices so they have names.
Then 'the things I want to do with the things' are organized into
macros. Then X-controls call one or more macros when pushed. 

## the dub channel 

After trying a few different methods I settled on using channel 8 as the
volume for the bus and channel 9 taking the audio in from channel 8 and
the fader also controlling the feedback on the delay. So channel 8
controls the volume of the 'dry' signal to be dubbed. If fader 8 is at
zero you can't dub anything. Fader 9 controls the level into the delay
and the repeats. 

I started by putting a multichain device on channel 8, with a plug-in in
'side chain monitoring mode' set to do nothing else. Then I set the
Record button to open or close the chain for each of the live tracks in
the set. So I could send zero, one, two, or every track into the delay
line. Again, that's basically a pre-fader bus, recreated with a hundred
mouse clicks. 

## dual channel banks

After messing around for a while and really enjoying the dub channel I
decided that 7 faders wasn't quite enough. So I used the two buttons in
the top right hand corner to select between two sets of seven tracks.
That is, the dub channel is always on channels 8 and 9, but if I press
the 'channel left' button the other seven faders + buttons are in
control of tracks 1-7, and if I press the 'channel right' button the
faders + buttons control channels 10-16. Looking at this line
```
BTN_BANK_ONE = NOTE, 1, 48 , 0, 127, $FADER_BANK_ONE$; $LISTENER_ONE$; $ENCODER_BUTTON_FILTER_ONE$ $ENCODER_KNOB_FILTER_ONE$; $SELECT_BUTTON_SELECT_ONE$; $MUTE_BUTTON_MUTE_ONE$
```

shows the LONG list of macros that are being called by those buttons. I
was worried that it would be just too much MIDI to go over the wire, but
so far it has been no problem. That matches up the select, mute, 'dub
bus,' and rotary button/encoder to be as desired. 

## bandpass filter

Another signature sound in this style of music is the sweeping bandpass
filter on an individual track. For this I made a mapping for the top
button encoder to toggle the on/off status of a named device on the
selected channel, and for the rotary to control the cutoff frequency of
the filter. That's a 'near static' mapping but so far has worked very
well. 

## A and B sends

I for these dubby tracks I don't use a lot of sends. One more send
reverb and one send delay is really enough if you're trying ot do this
stuff in real time. After a few tries with the knobs I finally settled
on just using the faders in 'send mode' instead. Better visual feedback
and then the knobs are always controlling the bandpass filter. So the
'bank left' and 'trans left' buttons set the faders to send A and B for
chanels 1-7, respectively. Bank and trans right do the same for channels
10-17. 

## device control

Finally, I have a lot of devices already created for my synths and
devices, so a one button 'faders take over device' control was helpful.
That's mapped to the 'return' button on the far right bottom, next to
the main (and unused) scrub knob. 

## TODOs

So far I haven't decided on any job for the 'Solo' knob on the channels.
I also still have a bunch of buttons availalbe if I want more stuff. I'm
sure eventually I will, but right now I'm still trying to figure out
what else I need. I've been practicing enough that my right hand sits
pretty comfy on the top right six buttons, but eventually I'll want more
I'm sure. 

## fun story

No tech project is complete, in my opinion, until you do something
incredibly stupid. So for this one when I was trying to figure out the
initial mappings I thought clyphx wasn't sending MIDI out to the
controllers. I asked a few tims on the nativeKONTROL board and
eventually realized that while the buttons and faders would **send**
MIDI with the bus powered USB input plugged in, the motorized faders
won't motorize unless they are plugged into the wall. I had my platform
M on a power strip that was unplugged, so the fader would send MIDI just
fine but not move. Facepalm. 
