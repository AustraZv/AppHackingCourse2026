## a) Examine the image h1 https://terokarvinen.com/application-hacking/h1.jpg
head, cat and xxd did not reveal anything too suspicious. Exiftools and file outputted data consistent with a jpeg image.


## b) Analyze the file using binwalk
Binwalk outputted the following information, inside the image was a ZIP file.
<img width="1412" height="224" alt="image" src="https://github.com/user-attachments/assets/de1cea4c-ee75-4a2a-a4ce-bad6a04f671f" />
I used binwalk -h to find out that the -E tag would extract the file. 
<img width="1412" height="224" alt="image" src="https://github.com/user-attachments/assets/acbdd6bd-16a8-4ece-a1a4-90b1d9c4bf54" />
It created a folder extractions for the extracted result, in addition it extracted the compressed archive. 
<img width="1406" height="642" alt="image" src="https://github.com/user-attachments/assets/ce57cc49-f462-4d58-9d72-c1ee74f4274a" />

Here is the directory tree 
