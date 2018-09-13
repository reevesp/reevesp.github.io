---
layout: post
title:  "Struggling to Print an email-ticket from the Windows10 Mail App"
---
This is what happened when my relative asked for help when he couldn't print his email.
The email in question was a ticket to an event from drinkup.london, it was a short document with a barcode in the middle- the sort that you print at home, take to the event, and the door staff will scan the barcode.

I naively thought this would be easy to resolve. This sort of thing is the bread-and-butter of family IT guys.

###### Three hours later...

We were using the Windows 10 mail program (MS refers to it as the "Mail App", or just "Mail").

First, I checked the printer. _Could I print from other programs?_ Yes, I could print some generic files that were lying around on the desktop.

_Could I print other emails from Windows 10 Mail?_ Yes, in fact I can print any other email it seemed- apart from this particular email message.

Maybe Win10 Mail could print this message into another format. _Could I print the email as a pdf? Yes!_ ...but the PDF does not open.

Something else I had noticed by this point was that the email kept deleting itself from the inbox. After I had to restore the mail from the `Deleted Items` folder, I decided to take a backup and saved the message as a file on the desktop. I noticed that the mail saved as an .eml file, and EML (email markup language) is just a type of XML.

Because **I love XML**, I decided to read through the mail's source. I opened the mail as XML.

```
<!-- START:: EMAIL CONTENT -->
<!-- Note: I've snipped loads of waffle out of the mail. This is just the markup for the visual part-->
<!-- I read the rest of the email as XML too, but there was nothing relevant -->
Thank you for booking a ticket to <span>The Whisky Exchange Whisky Show - SUNDAY EARLYBIRD</span>through DrinkUp.London.
<br />
<br />
<div class=3D"ticket">
<img class=3D"barcode" src=3D"https://sdl.blob.core.windows.net/abv-global-barcodes/3a-SOME-32-CHARACTER-ID-STRING-2.jpg" alt=3D"Image Barcode" />
<div><span class=3D"highlight">Unique Ticket Code: </span><span>0123456789</span></div>
```
I was immediately suspiscious of the fact that the barcode image was as remote URL to be loaded and marked-up.

I tried going to the URL directly in a browser (_perhaps I can just print the barcode image?_). `400 Bad Request`.

I googled around, and I found a forum post where it  mentioned that the remote server might dislike how the printer driver requests the remote content, and that he resolved his issue by setting the "Print as Image" setting.

Further googling revealed that the printer we were working with did not have this setting.

Next, I tried to remove the need for the printer driver to request the remote image.

I checked whether disconnecting Windows10 from the internet forced the use of a cached copy of the image. It didn't, or at least Windows10 had not cached the image..

I looked to see whether I could force win10 mail to `Work Offline`, similar to MS Outlook. I found that Win10 Mail doesn't have the `Work Offline` option.

_Could I force win10 mail to download the external image and cache it?_

... I could!

To do this I navigated to:
```
Settings > Reading Pane > External Content Heading
```
and I configured the settings as follows:
```
[] Apply to all accounts #I checked this box

<>Automatically download external images and style formats except S/MIME Email #I changed this setting to ON

<>Automatically download external images and style formats for S/MIME Email #I changed this setting to ON
```

I felt that I was getting somewhere.
(We were about 2 hours in at this point, and I knew that by-hook-or-by-crook I was getting this ticket printed.)

With the new settings configured, I opened the email again in Win10 Mail. I tried to print to the printer- it failed.

I tried to print to PDF again - _It worked!_

I tried to open the PDF - _It opened!_

Then, from the PDF reader program, I tried to print this PDF - _It printed!_

A very round-the-houses method!

**TL;DR** If you receive an email which contains a barcode that you can't print, and you are using win10 mail, try:

```
In Windows10 Mail:
Settings > Reading Pane > External Content Heading

Toggle the following settings:
[] Apply to all accounts (should be checked)
<>Automatically download external images and style formats except S/MIME Email (should be ON)
<>Automatically download external images and style formats for S/MIME Email (should be ON)

Print the Email as a PDF.
Open the PDF and print it from your PDF reader program
```
