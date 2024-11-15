# THIS DOCUMENT IS A MARKDOWN VERSION OF THE ORIGINAL README!

We are not the original authors of this program, we are just preserving the documentation on this really obscure tool!

## Multer 2.1

by Lamesoft / Lamers

English translation by voyager44

Additional fixes and localization by cs127, polyzium, RepellantMold, 0xHexaDecimator [on discord](discord.gg/dhc3xApywF)

Converted to Markdown formatting by RepellantMold


## 1. What is the program used for?

Multer is used for conversion of ProTracker modules written in a special way
to multichannel format (6 - 32 channels).

In other words, you can write multichannel modules on ProTracker. These
modules, after conversion using Multer, can be played back on any player that
has the ability to play back multichannel modules (like Hippo Player,
DeliTracker, PS3M, AccessiblePlayer, etc.), and most PC programs (like
InteriaPlayer, CubicPlayer, and FastTracker).

If the Amiga is equipped with a turbo card and fast type memory, modules can be played with frequency 58kHz with 14-bit audio mixing (for example, DeliTracker does an amazing job).

To play sound mixed at this frequency, you need to enable Multisync screen mode (load the Multisync mode driver for the Multisync monitor), also while playing back the module (after previously
setting that exact frequency in the program preferences), switch the computer
to Multisync screen mode (note - if the monitor does not support Multisync
mode, turn it off while the mode is displayed).


## 2. For users of the previous version of Multer (1.x).

The method of connecting patterns into multipatterns - now so-called "songdata" (sic), is set outside ProTracker (using a text editor - check "Process of writing modules").  
The operation of the program has also changed now (no internal interface - it can now be added as an option in DirectoryOpus; see "DirectoryOpus Integration").  
The program can also be assigned to, for example, CygnusEd, creating an automatic compiler (an idea conceived and made by Maxym).


## 3. Process of writing modules.

> [!NOTE]
> The variable names are case-sensitive!

Multer creates multichannel patterns (hereafter referred to as multipatterns) by combining several 4-channel patterns.  
Multer loads from the logical device "MULTER:" a MUL-LIST file, which is multichannel module songdata.
You can create this file using any text editor, such as CygnusEd.

The file has the following format:
```
MULTER=xx
; file header specifying the number of channels (two numbers, for example: 08,
; 12, 14, 22, 24, 32; the number needs to be even and between 6 and 32). The
; line "MULTER=xx" must be the first line of the file.

SOUR=dh1:modules/multer/mod.example(+linebreak)
DEST=ram:mod.example-m(+linebreak)
ADDM=dh1:modules/multer/mod.example-2(+linebreak)

; SOUR= - input module (needed)
; DEST= - output module (needed)
; ADDM= - additional module (optional)
; (+linebreak) means that the line must end with the last character of the
; module filename and it cannot have any additional comments on the same line.

MULTIPATTERNS:(+linebreak)
xx xx xx .. :yyy  ; xx - component pattern number (two digits),
                  ; yyy - multipattern number (three digits).
                  ; If the first character of the component pattern number is
                  ; a letter, it means the pattern is taken from the
                  ; additional module (ADDM). A corresponds to 0, B to 1,
                  ; C to 2, etc. Below are examples (for 16-channel module -
                  ; 4 component patterns). Multipattern number 999 means the
                  ; end of editing multipatterns).

00 02 04 01 :000
00 02 05 03 :001
00 02 A0 01 :002  (some comment)
00 A1 A2 03 :003
            :999

; Comments can be placed after each line without special markings
; (the semicolons in this text are used to distinguish comments from actual
; data, but they are not necessary) The "MULTIPATTERNS:" line is necessary to
; determine the beginning of the multipattern data.



POSITIONS:
=yyy  ; yyy - multipattern number at position 0 in the module
=yyy  ; yyy - multipattern number at position 1 in the module
...
=yyy  ; yyy - multipattern number at the next position in the module
=999  ; multipattern number 999 indicates the end of the multipattern list.

; Comments can be placed both before and after the position number, so the
; following are all valid: "POS 01 =002", "=002 this is the first position",
; "=002", "0001=002". The "POSITIONS:" line is necessary to determine the
; beginning of the position data (songdata).

; Example MUL-LIST files (with and without comments) should be included in the
; program archive.
```

## 4. DirectoryOpus integration.

To use this program more conveniently, it's worth using DirectoryOpus.
You should add a new Filetype or button.

List of commands in the window:

```
Command - Verify MULTING MODULE?
AmigaDos - assign MULTER: ram:T
AmigaDos - copy {f} ram:T/MUL-LIST
AmigaDos - dh0:progs/MULTER2.1
```

For Filetype, you should insert the above lines as DoubleClick and set in `"EditClass": File class - [MULTER], ClassID - [MULTER], Match - [MULTER=].`

This way, you can use this program more easily without being "tied" to the
"MUL-LIST" name for a file containing module data.


## 5. Problems:

**Q: During playback, some composition patterns are missing, or patterns are played in a "weird" way.**  
**A: ProTracker saves patterns from zero to the highest declared in the order list.
At least one of the positions must include the number of the highest component pattern.
To make sure, you can enter the value 63 (or 99 if 100PATT mode is on) at any position of the module.
This will not affect the length of the resulting module.**

**Q: The output module didn't include samples from the additional module (ADDM=).**  
**A: Multer reads only patterns from the additional module, while instruments are taken exclusively from the main module.**

**Q: Multer does not convert modules because it takes up too much memory.**  
**A: The program needs more than twice as much memory as the size of the input module.
If an additional module (ADDM) is included, the program also loads it into memory.
Remember that an additional module doesn't need to have instruments.
Try to remove samples from the additional module before conversion.
Also, try to optimize the module before conversion using the ModuleInfo program (option S enabled, but you should dismiss the question about deleting unnecessary patterns).**

**Q: The output module sounds terrible, it's out of tune and plays unevenly.**  
**A: Don't get too discouraged! Practice diligently, and you'll surely achieve satisfactory results! :-)**

**Q: While writing a module, the pizza that I put in the oven got burned.**  
**A: Write modules after eating a meal.**

**Q: I can't pass the second level.**  
**A: Try gathering additional weapons. If that doesn't help, type "EXTRALIFE" (while holding SHIFT).
You'll surely see the desired "CONGRATULATIONS!" message.**

**Q: My head hurts.**  
**A: Take a pill.**


## 6. Compatibility with ModuleInfo1.5e:

Enable option S and load the input module.
When asked whether to delete unused patterns, dismiss it
(if asked to delete patterns that are outside the module, also answer negatively).


## 7. Config file:

You can use a configuration file. To do this, place a text file called
"MULTER2.1-CONFIG" in the "S:" directory containing:
```
*Q* - the program will not display a message screen.
*W* - (only if option *Q* is enabled) - the program will use the RequestChoice
      file from the "C:" directory to inform about the completion of the
      conversion.the
*E* - in the last samples of the module, the program will insert a message in
      English instead of Polish.
*N* - in the last samples of the module, the program will insert a message in
      Norwegian instead of Polish.
*A-xxxxxx* - execute a script before exiting the program. xxxxxx is a
      DOS command to execute (the % sign will insert the name of the output
      module.) Example: *A-dh0:progs/hip "%"*
```
Example "MULTER2.1-CONFIG" file:
```
*Q*
*N*
*A-dh0:progs/hip "%"*
```

## 8. Terms and conditions of distribution.

Multer is a freeware program, which means you don't have to pay any fees to use it.
However, if the program proves useful, you can send me an email with a cool module to pb@boo.pl as a gift.  
`// cs: boo.pl seems to no longer exist :(`  
I prohibit the use of this program in any shareware program packages without prior contact with me and obtaining paid permission to publish the program (10 PLN).


## 9. Installation of the program and the requirements.

To install the program, you need to copy it over.  
Using the program from a floppy may cause neurosis.  
  `// cs: can confirm`  
The program requires more than 2 times as much memory as the size of the input module plus the size of the additional module plus the size of the program itself.
In total... with 1 MB you can forget that you have a computer, with 2 MB you can do something, and with more than that, it's actually pleasant.


## 10. Acknowledgment:

Thank you Maxym for your criticism and comments.  
I hope I met your expectations - if something is still missing, I'll certainly try to address it in the next version.


                          --------------------------
                      LAMERS - LAMESOFT PRODUCTION 1997
                          --------------------------
                        English translation by voyager44
                              discord: voyager44
                          --------------------------
                     additional fixes and localization by
                  cs127, polyzium, repellantmold, hexadecimator
                                  on discord
                             discord.gg/dhc3xApywF
