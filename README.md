# bandit
Writeups of Bandit Levels

In order to complete level 12-13, you will need to do a few things which are mostly file inspection and extraction. As the problem notes, you'll need to know the flags for tar, gzip, bzip2, xxd, among others.

First, connect to the level using ssh bandit.labs.overthewire.org -p 2220 -l bandit12 and password: JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

From there, you'll find a file called "data.txt" that we're told has been super compressed.

In order to create a file we can manipulate and track we're going to create a new temporary directory using mkdir /tmp/yourname and then we're going to copy the file using cp data.txt /tmp/yourname. You can use mv to rename the file as well.

Next, thanks to the helpful hint on Roppers, we're going to use xxd -r data.txt to see what's going on. In there, we can see that this decodes the hexdump to binary and contains something called data2.bin. 

We're now going to direct this to a new file that we'll call "datarevert" using xxd -r data.txt > datarevert. Now, using file datarevert we see that it is a gzip file, which we can rename as datarevert.gz using mv datarevert datarever.gz and unzip with gzip -d datarevert. 

After this, the product is (surprise) datarevert (again) which we'll check with file datarevert, which results in a bzip2 file. On we go - using bzip2 -d datarevert the product this time is datarevert.out. Time to inspect again - file datarevert.out is another gzip file. We'll rename with mv datarevert.out datarevert.gz to datarevert2.tar, and extract it using the command tar -xvf datarevert.tar. This extracts data5.bin - and now it's back to that old chestnut, file data5.bin, which reveals it's a another tar archive, so it's back to mv data5.bin data5.tar to data5.tar, extract it using tar -xvf data5.tar, we extract data6.bin, from there tar -xvf data6.bin which extracts data8.bin, we inspect with file data8.bin to discover it's yet again another gzip file, we rename to data8.gz with mv data8.bin data8.gz, extract using gzip -d data8.gz and *finally* are left with data8, an ASCII text file, which we can print with cat data8 to reveal that the password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw.
