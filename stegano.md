
###
Author: Shreyas Srinivasa
NetSec 2023 - Lecture 9 Steganography Activity

# Steganography with steghide

In this activity, we will hide a text within an image using steghide, a steganography tool

## Step 1: Download the files from Moodle
Download the file - "stego-Activity.zip" from moodle and unpack it. 

## Step 2: Install steghide
Use the command below to install steghide
```
sudo apt install steghide
```

## Step 3: Generate the steg file
We will now use steghide to inject the text into the image with the following command

```
cd steg-Activity
steghide embed -ef info.txt -cf BugsBunny.jpg -sf stego-image.png -v
```
## Step 4: Check the file size
After we generate the steg image, go to the path of the folder in the file explorer and check the size of BugsBunny.jpg and steg-image.png- Notice the increased file size of the generated image

## Step 5: Extracting the injected text from the steg image
Now we will extract the information from the generated image
```
steghide extract -sf steg-image.png -xf extracted_data.txt
```
show the extracted text
```
cat extracted_data.txt
```