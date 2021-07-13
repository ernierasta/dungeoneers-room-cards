#  Michael Lundstedt - Dungeoneers - Unofficial Room Cards #

**Description**: In short - in progress of making usable [Dungeoneers](https://boardgamegeek.com/boardgame/336195/dungeoneers) Room Cards. 

Michael created very interesting dungeon crawler, so in this repo you will find room cards which will shorten room generation process. I had tryied to keep original rooms description and rules where possible, but few cards were shortened. Even with that, font may be a still a bit small.

As many of board gamers do not have github account to create issues, if you have any comments post them [here on BGG](https://boardgamegeek.com/thread/2688109/idea-dedicated-room-cards).

If you want to create pdf's from scratch - you will need to:

- buy Dungeoneers from: [DriveThruRPG](https://www.drivethrurpg.com/product/357029/Dungeoneers?term=dungeoneers), [PNPArcade](https://www.pnparcade.com/collections/new-games/products/dungeoneers) or [RandomSkill](https://randomskill.games/product/dungeoneers/)
- [Inkscape](https://inkscape.org/)
- [countersheetextension](https://github.com/lifelike/countersheetsextension)
- pdftotext and pdfimages from [Xpdf command line tools](http://www.xpdfreader.com/download.html)
- image editor f.e: [Gimp](https://www.gimp.org/)

If you want to automate image enlarging (may be not worth it under Windows):
- [ImageMagick](https://imagemagick.org/script/download.php)
- Linux. ;-)

I have two PDF version, all should be printable on A4 and Letter pages. For friend from USA - bottom part of cutting lines will be probably not printed, but it should be ok. If not, let me know!

- Duplex printig layout.
- Gutter fold layout.

## How to - from extracting data from componnents PDF to creating PDF with cards

**This is technical stuff, you probably just want to download PDF mentioned above. ;-)**

You will not likely run it at once, as few manual steps are neccessary to make it work,
I will guide you (or myself - 3 months later ;-) ), how everything was done.

Get text from pdf to txt file.
`pdftotext -f 51 -l 56 -raw ../Dungeoneers_Quest_Book_1.5.pdf src/room-tables.txt`
-raw option gets better cell order from pdf tables

Prepare **room-tables.txt&** in vim, use **Shift+j** to join lines to one long line. Then format room titles by 
adding \*Title\*\\n (bold + new line). Also, change '\' to something else or it will be interpreted as italic.
After getting text I use **LibreOffice** and manualy move text into csv. It is quite easy after joining lines in vim.

Get b&w room images from pdf:
```bash
mkdir -p src/images/org
pdfimages -f 51 -l 56 -all Room_cards.pdf src/images/org/bwrooms
```

Change filename to something reasonable (4x2.png).

Add padding to room images (they were renamed by hand; also I had messed up, so end up allignment by align tool in Gimp)
```bash
cd src/images; for f in `ls --color=never org/*.png`; do echo $f; convert $f -gravity south -background none -extent 320x193 $f; done; cd ../..
```
Then get rid of white color in Gimp: Color - Color to Alpha ... and then CTRL+F for all images.

Now add filename to csv file, generate layouts until you are happy. Then modify lines below to document layout settings.

Let's get fancy and generate whole layouts without opening Inkscape :-)
```bash
mkidir -p src/temp/1
PYTHONPATH=/usr/share/inkscape/extensions/ python3 $HOME/.config/inkscape/extensions/countersheet.py -d src/rooms.csv -I src/images -p src/temp/1/ -z 2mm -r 3mm -D true -O 5mm -S 4mm -B true src/rooms.svg > src/result1.svg
```
