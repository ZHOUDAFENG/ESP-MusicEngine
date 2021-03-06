# ESP-MusicEngine

## DEPRECATED / NOT MAINTAINED
This library is replaced by the [MmlMusicPWM](https://github.com/maxint-rd/MmlMusicPWM) library, which is an extension of the new device independant [MML Music](https://github.com/maxint-rd/MmlMusic) library. That library supports not only the ESP8266, but also various Atmel MCUs, such as the ATmega328 and Atmega168 and even the ATtiny85. The MmlMusic base library also supports playing polyphone MML music from multiple tracks on multi-channel sound devices such as the SN76489 complex sound generator.

### Credits/references
MusicEngine library ported from mBed to Arduino (tested IDE v1.6.10 and v1.8.2). Supports ESP8266 and ATmega (tested 328 and 168)<br>
MusicEngine class / Retro  Music Engine<br>
Original author: Chris Taylor (taylorza). Open source license: Apache 2.0<br>
see https://developer.mbed.org/users/taylorza/code/MusicEngine/<br>
Ported from mBed to Arduino by MMOLE (maxint-rd), inherited Apache license.

MusicEngine provides a means to play Music Macro Language sequences asynchronously. Where the tone() API-function allows for playing one single note, the MusicEngine.play() method can play an entire music score.<br>
The music is played using an interrupt routine that changes the PWM frequency according the specific notes being played. This means we can do other things while the music keeps playing.

Learn more about Music Macro Language (MML) on wikipedia:<br>
   http://en.wikipedia.org/wiki/Music_Macro_Language<br>
   For downloadable MML music see http://www.archeagemmllibrary.com/<br>
Extensive MML reference guide (not all commands supported):<br>
   http://woolyss.com/chipmusic/chipmusic-mml/ppmck_guide.php<br>
Info about using PWM and other methods to generate sound:<br>
   https://developer.mbed.org/users/4180_1/notebook/using-a-speaker-for-audio-output/

================================
### Installation/Usage
The current version can be downloaded as an Arduino library using the Sketch|Library menu. Just add the zipfile library and the enclosed examples should appear in the menu automatically. On the ESP8266 the Ticker library is used. For ATmega a Timer2 interrupt is used.

Initialisation outside of Setup():
```
  // define pin, include header and initialize class
  #include <MusicEngine.h>
  #define BUZ_PIN 14
  MusicEngine music(BUZ_PIN);
```

Then to play music, call the play method where you want:
```
music.play("T240 L16 O6 C D E F G");
```
Note: the music will keep on playing using a Ticker interrupt.

### Supported MML Syntax
Short syntax overview:<br>

Command | Description
------------ | -------------
&nbsp;  Tnnn | Set tempo [32-255]. Examples: T120, T240<br>
&nbsp;  Vnnn | Set volume [0-128]. Note: limited effect on PWM-volume. Examples: V1, T120<br>
&nbsp;  Lnn  | Set default note length [1-64]. Examples: L8, L16<br>
&nbsp;  Mx   | Set timing. Mn=default, Ml=legato, Ms=staccato<br>
&nbsp;  On   | Set octave [0-7]. Examples: O6, O7<br>
&nbsp;  A-G  | Play whole note. Example: C<br>
&nbsp;  Ann-Gnn  | Play note of alternative length [1-64]. Example: C4, A16<br>
&nbsp;  Nnn  | Play frequency [0-96]. Example: N48<br>
&nbsp;  #    | Play sharp note. Example: C#<br>
&nbsp;  &plus;    | Alternative for #<br>
&nbsp;  &minus;   | Play flat note. Example: D-<br>
&nbsp;  R    | Rest. Example:  CDEC r CDEC<br>
&nbsp;  P    | Alternative for R. Example:  CDEC p CDEC<br>
&nbsp;  .    | Longer note. Example: CDEC.&nbsp;<br>
&nbsp;  &gt; | shift octave up.  Example: CDE&gt;CDE.&nbsp;<br>
&nbsp;  &lt; | shift octave down.  Example: CDE&lt;CDE.&nbsp;<br>

The supported MML-commands are a subset that may not completely cover all available music scores.
If notes seem missing, check your score against the syntax above and replace unknown commands by equivalent supported alternatives. The music notation is case-insensitive. Spaces are not required but can be used for readability.

### Features & limitations
- The current version of this library supports ESP8266 and Atmel ATmega328 and ATmega168 MCUs. Support for ATtiny85 is being added, so stay tuned!
- This version currently supports only single channel playback on a buzzer of speaker. Playback using a complex sound generator is under investigation.
- Known bug: when ending the play-string with a number (eg. "T120 O4 G16") the player may read beyond the end of the string and play whatever is next in memory. Workaround: use alternative notation (eg. "T120 O4 L16 G") or an addional terminator (eg. "T120 O4 G16\0").
