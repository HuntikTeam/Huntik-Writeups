
### Challenge : Hidden - Trace

- Download and unzip it, "file" to check, it is a Linux rev 1.0 ext4 filesystem data file. I spent a lot of timeon this challenge, I tried
everything from "mount", "binwalk" .... but it didn't seem to work. I suddenly thought of "foremost".
I used "foremost" to extract files in the original file and found that they were divided into 2 sections :

- Checking the first jpg entry is not very positive, all are normal images, in the second entry we get a flag.
