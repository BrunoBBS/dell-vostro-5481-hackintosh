# AppleALC codec creation Guide Journal

> Gruide (from 2012!!)
> [here](https://osxlatitude.com/forums/topic/1946-complete-applehda-patching-guide/)
> and also an archived pdf in this folder

- [1. Calculating Codec verb commands and PathMaps](##1-calculating-codec-verb-commands-and-pathmaps)
- [2. Patching XML(Platforms and layout) files.](##2-patching-xml-platforms-and-layout--files)
- [3. AppleHDA Binary Patch](##3-applehda-binary-patch)
- [4. HDMI Audio Patch(coming soon)](##4-hdmi-audio-patch-coming-soon-)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 1. Calculating Codec verb commands and PathMaps
### Part 1
#### Section 1 - Getting the codec dump
> Got the dump using linux 

#### Section 2 - Analyzing the codec dump and extracting relevant information

> I suggest starting from 7 to 4 and ~~temporarily~~ delete the lines that were
> already copied

1. Codec: ALC3204(ALC236)
2. Address: 0
3. Vendor Id (Convert this hex value into decimal value): 0x10ec0236 (283902518)
4. Pin Complex Nodes with Control Name
```yaml
Node 0x12 [Pin Complex] wcaps 0x40040b: Stereo Amp-In
  Control: name="Internal Mic Boost Volume", index=0, device=0
    ControlAmp: chs=3, dir=In, idx=0, ofs=0
  Amp-In caps: ofs=0x00, nsteps=0x03, stepsize=0x27, mute=0
  Amp-In vals:  [0x00 0x00]
  Pincap 0x00000020: IN
  Pin Default 0x90a60140: [Fixed] Mic at Int N/A
    Conn = Digital, Color = Unknown
    DefAssociation = 0x4, Sequence = 0x0
    Misc = NO_PRESENCE
  Pin-ctls: 0x20: IN
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
Node 0x14 [Pin Complex] wcaps 0x40058d: Stereo Amp-Out
  Control: name="Speaker Playback Switch", index=0, device=0
    ControlAmp: chs=3, dir=Out, idx=0, ofs=0
  Amp-Out caps: ofs=0x00, nsteps=0x00, stepsize=0x00, mute=1
  Amp-Out vals:  [0x00 0x00]
  Pincap 0x00010014: OUT EAPD Detect
  EAPD 0x2: EAPD
  Pin Default 0x90170110: [Fixed] Speaker at Int N/A
    Conn = Analog, Color = Unknown
    DefAssociation = 0x1, Sequence = 0x0
    Misc = NO_PRESENCE
  Pin-ctls: 0x40: OUT
  Unsolicited: tag=00, enabled=0
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
  Connection: 1
     0x02
Node 0x19 [Pin Complex] wcaps 0x40048b: Stereo Amp-In
  Control: name="Headset Mic Boost Volume", index=0, device=0
    ControlAmp: chs=3, dir=In, idx=0, ofs=0
  Amp-In caps: ofs=0x00, nsteps=0x03, stepsize=0x27, mute=0
  Amp-In vals:  [0x00 0x00]
  Pincap 0x00003724: IN Detect
    Vref caps: HIZ 50 GRD 80 100
  Pin Default 0x411111f0: [N/A] Speaker at Ext Rear
    Conn = 1/8, Color = Black
    DefAssociation = 0xf, Sequence = 0x0
    Misc = NO_PRESENCE
  Pin-ctls: 0x24: IN VREF_80
  Unsolicited: tag=00, enabled=0
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
Node 0x1a [Pin Complex] wcaps 0x40048b: Stereo Amp-In
  Control: name="Headphone Mic Boost Volume", index=0, device=0
    ControlAmp: chs=3, dir=In, idx=0, ofs=0
  Amp-In caps: ofs=0x00, nsteps=0x03, stepsize=0x27, mute=0
  Amp-In vals:  [0x00 0x00]
  Pincap 0x00003724: IN Detect
    Vref caps: HIZ 50 GRD 80 100
  Pin Default 0x411111f0: [N/A] Speaker at Ext Rear
    Conn = 1/8, Color = Black
    DefAssociation = 0xf, Sequence = 0x0
    Misc = NO_PRESENCE
  Pin-ctls: 0x20: IN VREF_HIZ
  Unsolicited: tag=00, enabled=0
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
Node 0x21 [Pin Complex] wcaps 0x40058d: Stereo Amp-Out
  Control: name="Headphone Playback Switch", index=0, device=0
    ControlAmp: chs=3, dir=Out, idx=0, ofs=0
  Amp-Out caps: ofs=0x00, nsteps=0x00, stepsize=0x00, mute=1
  Amp-Out vals:  [0x80 0x80]
  Pincap 0x0001001c: OUT HP EAPD Detect
  EAPD 0x2: EAPD
  Pin Default 0x02211020: [Jack] HP Out at Ext Front
    Conn = 1/8, Color = Black
    DefAssociation = 0x2, Sequence = 0x0
  Pin-ctls: 0xc0: OUT HP
  Unsolicited: tag=01, enabled=1
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
  Connection: 2
     0x02 0x03*
```
5. Audio Mixer/Selector Nodes
```yaml
Node 0x22 [Audio Mixer] wcaps 0x20010b: Stereo Amp-In
  Amp-In caps: ofs=0x00, nsteps=0x00, stepsize=0x00, mute=1
  Amp-In vals:  [0x80 0x80] [0x80 0x80] [0x80 0x80] [0x80 0x80] [0x80 0x80]
  Connection: 5
     0x18 0x19 0x1a 0x1b 0x1d
Node 0x23 [Audio Mixer] wcaps 0x20010b: Stereo Amp-In
  Amp-In caps: ofs=0x00, nsteps=0x00, stepsize=0x00, mute=1
  Amp-In vals:  [0x80 0x80] [0x80 0x80] [0x80 0x80] [0x80 0x80] [0x80 0x80] [0x00 0x00]
  Connection: 6
     0x18 0x19 0x1a 0x1b 0x1d 0x12
Node 0x24 [Audio Selector] wcaps 0x300101: Stereo
  Connection: 2
     0x12* 0x13
```
6. Audio Output Nodes
```yaml
Node 0x02 [Audio Output] wcaps 0x41d: Stereo Amp-Out
  Control: name="Speaker Playback Volume", index=0, device=0
    ControlAmp: chs=3, dir=Out, idx=0, ofs=0
  Amp-Out caps: ofs=0x57, nsteps=0x57, stepsize=0x02, mute=
  Amp-Out vals:  [0x56 0x56]
  Converter: stream=0, channel=0
  PCM:
    rates [0x60]: 44100 48000
    bits [0xe]: 16 20 24
    formats [0x1]: PCM
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
Node 0x03 [Audio Output] wcaps 0x41d: Stereo Amp-Out
  Control: name="Headphone Playback Volume", index=0, device=0
    ControlAmp: chs=3, dir=Out, idx=0, ofs=0
  Device: name="ALC3204 Analog", type="Audio", device=0
  Amp-Out caps: ofs=0x57, nsteps=0x57, stepsize=0x02, mute=0
  Amp-Out vals:  [0x00 0x00]
  Converter: stream=0, channel=0
  PCM:
    rates [0x60]: 44100 48000
    bits [0xe]: 16 20 24
    formats [0x1]: PCM
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
Node 0x06 [Audio Output] wcaps 0x611: Stereo Digital
  Converter: stream=0, channel=0
  Digital:
  Digital category: 0x0
  IEC Coding Type: 0x0
  PCM:
    rates [0x5e0]: 44100 48000 88200 96000 192000
    bits [0xe]: 16 20 24
    formats [0x1]: PCM
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
```
7. Audio Input Nodes
```yaml
Node 0x07 [Audio Input] wcaps 0x10051b: Stereo Amp-In
  Amp-In caps: ofs=0x17, nsteps=0x3f, stepsize=0x02, mute=1
  Amp-In vals:  [0x97 0x97]
  Converter: stream=0, channel=0
  SDI-Select: 0
  PCM:
    rates [0x560]: 44100 48000 96000 192000
    bits [0xe]: 16 20 24
    formats [0x1]: PCM
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
  Connection: 1
     0x24
Node 0x08 [Audio Input] wcaps 0x10051b: Stereo Amp-In
  Control: name="Capture Volume", index=0, device=0
    ControlAmp: chs=3, dir=In, idx=0, ofs=0
  Control: name="Capture Switch", index=0, device=0
    ControlAmp: chs=3, dir=In, idx=0, ofs=0
  Device: name="ALC3204 Analog", type="Audio", device=0
  Amp-In caps: ofs=0x17, nsteps=0x3f, stepsize=0x02, mute=1
  Amp-In vals:  [0x3b 0x3b]
  Converter: stream=0, channel=0
  SDI-Select: 0
  PCM:
    rates [0x560]: 44100 48000 96000 192000
    bits [0xe]: 16 20 24
    formats [0x1]: PCM
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
  Connection: 1
     0x23
Node 0x09 [Audio Input] wcaps 0x10051b: Stereo Amp-In
  Amp-In caps: ofs=0x17, nsteps=0x3f, stepsize=0x02, mute=1
  Amp-In vals:  [0x97 0x97]
  Converter: stream=0, channel=0
  SDI-Select: 0
  PCM:
    rates [0x560]: 44100 48000 96000 192000
    bits [0xe]: 16 20 24
    formats [0x1]: PCM
  Power states:  D0 D1 D2 D3 EPSS
  Power: setting=D0, actual=D0
  Connection: 1
     0x22
```

### Part 2 - Extracting the verb data
#### Section 1 - Extracting the values 'Pin Default', 'EAPD' and 'Node ID' from the Pin Complex Nodes

Pin Complex Nodes with Control Name

```yaml
Node 0x12 : Pin Default 0x90a60140
Node 0x14 : Pin Default 0x90170110, EAPD 0x2 
Node 0x19 : Pin Default 0x411111f0
Node 0x1a : Pin Default 0x411111f0
Node 0x21 : Pin Default 0x02211020, EAPD 0x2
```

#### Section 2 - Extracting the verb data

```yaml
Node 0x12 : Pin Default 0x90a60140
Extracted verb data: 40 01 a6 90
  Control: name="Internal Mic Boost Volume", index=0, device=0

Node 0x14 : Pin Default 0x90170110, EAPD 0x2 
Extracted verb data: 10 01 17 90
  Control: name="Speaker Playback Switch", index=0, device=0

Node 0x19 : Pin Default 0x411111f0
Extracted verb data: f0 11 11 41
  Control: name="Headset Mic Boost Volume", index=0, device=0

Node 0x1a : Pin Default 0x411111f0
Extracted verb data: f0 11 11 41
  Control: name="Headphone Mic Boost Volume", index=0, device=0

Node 0x21 : Pin Default 0x02211020, EAPD 0x2
Extracted verb data: 20 10 21 02
  Control: name="Headphone Playback Switch", index=0, device=0
```

#### Correcting the verb data

- Note 1: No changes
- Note 2: byte 3 (<-) of ext Mics (nodes 19 and 1a) changed from 11 to 10
- Note 3: No changes
- Note 4: Change every node to have a serial first byte
- Note 5: No changes
- Note 6: Internal Mic (node 12) byte 2 changed from a6 to a0
- Note 7: Ext Mics (nodes 19 and 1a) byte 2 changed from 11 to 81

```yaml
Node 0x12 (18): Pin Default 0x90a60140
Extracted verb data: 10 01 a0 90
  Control: name="Internal Mic Boost Volume", index=0, device=0

Node 0x14 (20): Pin Default 0x90170110, EAPD 0x2 
Extracted verb data: 20 01 17 90
  Control: name="Speaker Playback Switch", index=0, device=0

Node 0x19 (25): Pin Default 0x411111f0
Extracted verb data: 30 10 81 41
  Control: name="Headset Mic Boost Volume", index=0, device=0

Node 0x1a (26): Pin Default 0x411111f0
Extracted verb data: 40 10 81 41
  Control: name="Headphone Mic Boost Volume", index=0, device=0

Node 0x21 (33): Pin Default 0x02211020, EAPD 0x2
Extracted verb data: 50 10 21 02
  Control: name="Headphone Playback Switch", index=0, device=0
```

### Part 3 - Calculating the Codec verb commands 

```yaml
Node 0x12 :
0 + 12 + 71c + 10 = 01271c10
0 + 12 + 71d + 01 = 01271d01
0 + 12 + 71e + a0 = 01271ea0
0 + 12 + 71f + 90 = 01271f90

Node 0x14 :
0 + 14 + 71c + 20 = 01471c20
0 + 14 + 71d + 01 = 01471d01
0 + 14 + 71e + 17 = 01471e17
0 + 14 + 71f + 90 = 01471f90
0 + 14 + 70c + 02 = 01470c02

Node 0x19 :
0 + 19 + 71c + 30 = 01971c30
0 + 19 + 71d + 10 = 01971d10
0 + 19 + 71e + 81 = 01971e81
0 + 19 + 71f + 41 = 01971f41

Node 0x1a :
0 + 1a + 71c + 40 = 01a71c40
0 + 1a + 71d + 10 = 01a71d10
0 + 1a + 71e + 81 = 01a71e81
0 + 1a + 71f + 41 = 01a71f41

Node 0x21 :
0 + 21 + 71c + 50 = 02171c50
0 + 21 + 71d + 10 = 02171d10
0 + 21 + 71e + 21 = 02171e21
0 + 21 + 71f + 02 = 02171f02
0 + 21 + 70c + 02 = 02170c02


Disable unused pin complexes:

Node 0x13
01371cf0 01371d00 01371e00 01371f40

Node 0x18
01871cf0 01871d00 01871e00 01871f40

Node 0x1b
01b71cf0 01b71d00 01b71e00 01b71f40

Node 0x1d
01d71cf0 01d71d00 01d71e00 01d71f40

Node 0x1e
01e71cf0 01e71d00 01e71e00 01e71f40
```

### Final codec commands

```
<01271c10 01271d01 01271ea0 01271f90
 01371cf0 01371d00 01371e00 01371f40
 01471c20 01471d01 01471e17 01471f90 01470c02
 01871cf0 01871d00 01871e00 01871f40
 01971c30 01971d10 01971e81 01971f41
 01a71c40 01a71d10 01a71e81 01a71f41
 01b71cf0 01b71d00 01b71e00 01b71f40
 01d71cf0 01d71d00 01d71e00 01d71f40
 01e71cf0 01e71d00 01e71e00 01e71f40
 02171c50 02171d10 02171e21 02171f02 02170c02>
```

### Part 4 - Calculating PathMaps

Output devices Pathmap

```yaml
Node 0x14 Speaker : 
0x14->0x02, Decimal - 20 -> 02 
Node 0x21 Headphones : 
0x21->0x03, Decimal - 33 -> 03
```

Input devices Pathmap

> Audio Inputs 0x08 and 0x09 go to different mixers, but the mixers connect to
> the same pin complexes

```yaml
Node 0x12 Internal Mic : 
0x07->0x24->0x12, Decimal - 7 -> 36 -> 18
18 -> 36 -> 7 (0x12 -> 0x24 -> 0x07)
Node 0x19 Headset Mic :
0x08->0x23->0x19, Decimal - 8 -> 35 -> 25
25 -> 35 -> 8 (0x19 -> 0x23 -> 0x08)
Node 0x1a Headphone Mic:
0x09->0x22->0x1a, Decimal - 9 -> 34 -> 26
26 -> 34 -> 9 (0x1a -> 0x22 -> 0x09)
