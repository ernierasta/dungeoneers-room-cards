# Dungeoneers Unofficial Room Cards #

Description: In short - in progress of making usable Dungeoneers Room Cards.

```shell
#!bin/sh

# ER2021: How to - from extracting data from componnents PDF to creating PDF with cards

# This is Linux shell script, ignore if you do not know what it is :-)
# You will not likely run it at once, as few manual steps are neccessary to make it work,
# I will guide you (or myself - 3 months later ;-) ), how everything was done.

# get text from pdf to txt file
pdftotext -f 51 -l 56 -raw ../Dungeoneers_Quest_Book_1.5.pdf src/room-tables.txt
# -raw option gets better cell order from pdf tables

# Prepare room-tables.txt in vim, use Shift+j to join lines to one long line. Then format room titles by 
# adding *Title*\n (bold + new line). Also, change '\' to something else or it will be interpreted as italic.
# After getting text I use libreoffice and manualy move text into csv. It is quite easy after joining lines in vim.

# get b&w room images from pdf
mkdir -p src/images/org
pdfimages -f 51 -l 56 -all Room_cards.pdf src/images/org/bwrooms

# change filename to something reasonable (4x2.png).

# add padding to room images (they were renamed by hand; also I had messed up, so end up allignment by align tool in Gimp)
cd src/images; for f in `ls --color=never org/*.png`; do echo $f; convert $f -gravity south -background none -extent 320x193 $f; done; cd ../..

# Then get rid of white color in Gimp: Color - Color to Alpha ... and then CTRL+F for all images.

# now add filename to csv file, generate layouts until you are happy. Then modify lines below to document layout settings.

# let's get fancy and generate whole layouts without opening inkscape :-)
mkidir -p src/temp/1
PYTHONPATH=/usr/share/inkscape/extensions/ python3 $HOME/.config/inkscape/extensions/countersheet.py -d src/rooms.csv -I src/images -p src/temp/1/ -z 2mm -r 3mm -D true -O 5mm -S 4mm -B true src/rooms.svg > src/result1.svg
```
