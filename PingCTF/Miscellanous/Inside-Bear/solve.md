# Inside-Bear

## PingCTF 2023 has really good challenges.

&ensp; Solution

- Because I'm not good at other things, this is the first lesson I approach. After unzipping the downloaded file, we will receive a C file with a greeting and an ELF file. I used `foremost` to try to get the hidden part of that file and it really worked. In there, there continues to be a zip file, unzip it again and I see there is 1 ogg file, 1 WAV file and 1 GIF file. Prioritize the WAV file, use audacity to open it and switch to Spectrogram, I realize it is a string of characters.

 - <img src="spec-bear.png" width="500" height="200">

- Take them out and try with base64, we get the flag.

    -> flag: *ping{I_alway$_c0me_b@ck}*



