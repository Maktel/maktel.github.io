---
theme: post
title: "Calibri and Cambria fonts in Libreoffice and MS Office inside Wine on Ubuntu"
categories: [linux]
tags: [fonts, libreoffice, ubuntu, linux, wine, ms office]
---

# Backstory
On Ubuntu I use LibreOffice, but sometimes it is not capable of certain tasks – so few months ago I installed MS Office 2013 and ran it with Wine. Well, it didn't solve all of my problems, but most of the time I can accomplish my tasks using powers of both programs combined.
However, there was one particular issue I couldn't efficiently solve: fonts. Neither LibreOffice nor MS Office could render Microsoft fonts like Calibri and Cambria. It was especially annoying when working on somebody else's documents with read-only content. The whole layout of the files was lost during rendering and documents looked different on other machines, since different font was used in place of default Calibri.
I came up with lazy, yet brilliant solution – I just edited all documents on Windows installed on virtual machine. Doesn't seem like a viable long term solution.

# First try – install font alternative to Calibri
Carlito and Caladea are 2 fonts that are metric-compatible with Calibri and Cambria, respectively. Although they don't look the same, the document layout should stay the same. I installed these fonts in all 4 of their variants (regular, bold, italic, bold italic).
LibreOffice immediately recognised them as a replacement for Microsoft fonts and rendered a document the way it would be rendered with original fonts. Unfortunately, exporting to PDF sometimes failed to properly encode text, resulting in unreadable gibberish. Also, Word running in Wine didn't recognise the alternative fonts.

# Second try – install fonts for Wine
I tried to install fonts using winetricks, since LibreOffice seemed to be working (except for the PDF export, but I could live without it). Winetricks installer allowed me to install some fonts, but Calibri apparently wasn't included in any of the packages.
Since fonts in Windows are stored as ordinary .ttf files somewhere in system files, I decided to copy them manually to `~/.wine/drive_c/windows/Fonts` directory. Looked like it should do the trick, but it was unsuccessful.

# Third attempt – install fonts for Ubuntu
Maybe after all, the missing fonts in Wine were problem on Ubuntu's side, not Wine's? 
I moved the font files from Wine's Fonts directory into `/usr/share/fonts/truetype/` (you can create a directory for them if you like), effectively installing them for both LibreOffice and Wine. 
After quick testing, everything seemed to work just fine. The only thing that isn't perfect, is displaying of Polish diacritics when zooming in or out on the document, although it is purely cosmetical.
