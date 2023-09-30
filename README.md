FIX ME LIST:
* Generating *.png files, the red and blue color are inverted (fix provided)
* --indir is NYI (Not Yet Implemented) => Maybe it should be
* Executing on Idle.bin only produce a small `story.json` file
* Executing on Intro.bin only produce left eye images (from eye001.png to eye037.png) where there should be the double with left and right eyes
* Naming of the file could contain the sequence identifier and the image in that sequence to help sort the images
* Near line 209 in snxrom: # TODO add eye & seq data
* 

Error: (notice the new feature to change the story_id)
´C:\Project\TeddyRuxPin\SNXROM>python snxrom.py --infile Story11.bin --newid 1 --outfile NewStory01.bin > out_story11to1.txt´

´´´
  File "C:\Project\TeddyRuxPin\SNXROM\snxrom.py", line 354, in main
    rom.saveBin(pathlib.Path(outfile))
  File "C:\Project\TeddyRuxPin\SNXROM\snxrom.py", line 250, in saveBin
    f.write(self.content)
  File "C:\Project\TeddyRuxPin\SNXROM\snxrom.py", line 223, in content
    assets.append(makeEyeData(eyeImage))
  File "C:\Project\TeddyRuxPin\SNXROM\snxrom.py", line 179, in makeEyeData
    r = numpy.array(eyeImage.getdata(0), dtype=numpy.uint16)
AttributeError: 'int' object has no attribute 'getdata'
´´´



Workflow:
 * Install [rhubarb](https://github.com/DanielSWolf/rhubarb-lip-sync) and `pip install g722_1_mod`.
 * Get an original `Intro.bin` file (in fact, back up all the contents of Teddy)
 * Record your new audio in `audio.wav` at 16kHz or 32kHz sample rate
 * Create a json mouth position file using [rhubarb](https://github.com/DanielSWolf/rhubarb-lip-sync): `rhubarb -f json -o mouth.json audio.wav`, or create a compatible file any way you like
 * Create the new Intro.bin file: `python3 earpatch.py  --wav audio.wav --rhubarb-json mouth.json orig/Intro.bin Intro.bin`
   * eye animations are generated randomly by default. You can control their frequency with `--random-eyes-median` and `--random-eyes-std-dev` command line parameters.
     The default is an eye animation around once every 30s.
   * Disable eye animations with `--no-random-eyes`
 * Plug in Teddy and copy the new Intro.bin to the Books folder of the USB drive that appears
 * Eject / Safely Remove the drive
 * Power Teddy off and back on

Typical commandline:
```
python3 earpatch.py  --wav audio.wav --rhubarb-json mouth.json orig/Intro.bin Intro.bin
```

Your new file will be played in lieu of the original Intro.

Installation troubleshooting:

If you get this error:
```
Traceback (most recent call last):
  File "earpatch.py", line 170, in <module>
    earpatch()
TypeError: command.<locals>.decorator() missing 1 required positional argument: 'f'
```

Make sure you use click version 8.1 or above.
This command can help: `pip install -U click`

License: MIT
