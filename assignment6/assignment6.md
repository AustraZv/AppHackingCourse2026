## a) Examine the image h1 https://terokarvinen.com/application-hacking/h1.jpg
head, cat, strings and xxd did not reveal anything too suspicious. Exiftools and file outputted data consistent with a jpeg image.


## b) Analyze the file using binwalk
Binwalk outputted the following information, inside the image was a ZIP file.
<img width="1412" height="224" alt="image" src="https://github.com/user-attachments/assets/de1cea4c-ee75-4a2a-a4ce-bad6a04f671f" />
I used binwalk -h to find out that the -E tag would extract the file. 
<img width="1412" height="224" alt="image" src="https://github.com/user-attachments/assets/acbdd6bd-16a8-4ece-a1a4-90b1d9c4bf54" />
It created a folder extractions for the extracted result, in addition it extracted the compressed archive. 
<img width="1406" height="642" alt="image" src="https://github.com/user-attachments/assets/ce57cc49-f462-4d58-9d72-c1ee74f4274a" />

Here is the directory tree 
<img width="1420" height="678" alt="image" src="https://github.com/user-attachments/assets/8ba3ef6c-9dbf-438e-8cee-7081b1934f46" />

The names of the xml files and the folders made me suspect that this was a word document, after looking into it, turns out a word document is just a zip file of .xml files that describe it, thus I recompressed the files and changed the file ending to .docx


This is the resulting document
<img width="1510" height="2094" alt="image" src="https://github.com/user-attachments/assets/b3c08320-330c-4e5c-9732-726d589a6700" />

## c) FOSS (Free Android OpenSource). Explore Android applications from Offa's (2024) list. https://github.com/offa/android-foss
I decided to investigate Etar, an open source calendar app, I chose it because I've used it before and was curious to look inside.

# JADX 
After running the jadx command in the terminal, the decompiler produced these directories as output, full of various java files, with functions and objects all being serperate, some files are just a singular function.
<img width="1550" height="1754" alt="image" src="https://github.com/user-attachments/assets/e7818042-bd43-4b76-80d5-7f898905faea" />

Ill further investigate the code in the gui version of jadx
# JADX-gui
<img width="2166" height="1458" alt="image" src="https://github.com/user-attachments/assets/5a60e1b9-8744-44cd-bd4a-deda90167689" />
After opening the same .apk in jadx-gui, i used the go to application button to find the core of the app, its very small, and in typical java fashion, everything is split into multiple files. The decompiler does treat it all like a single project but it is a lot harder to navigate than Ghidra. Even for complex files, I generally find managing binaries in ghidra to be more straight-forward, but maybe that is because my first coding experience came from c++.

<img width="1550" height="1754" alt="image" src="https://github.com/user-attachments/assets/11b85e8b-3f8c-4c4f-9f65-e0a47826a5fe" />

The main of the application is located in a file called "AllInOneActivity" 

The file contains various functions for many core operations, such as creation of toolbars and the basic ui.

<img width="1550" height="1754" alt="image" src="https://github.com/user-attachments/assets/7630c26a-631b-4d3a-9337-6204564d0c92" />

# ZIP
After renaming and unzipping the file, the resources of the file were visible, some of the structure was maintained but all the classes were put into .dex files, that have compiled binary code.
<img width="1810" height="1856" alt="image" src="https://github.com/user-attachments/assets/ba089ed1-463b-44cd-94a7-b127180ef917" />
# byte-unpacker
