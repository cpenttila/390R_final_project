# 390R_final_project

Our original project was to look into the firmware on a fitbit to understand more about the data that is stored and if an unauthorized person could access the GPS data. After our original plan hit a roadblock, we transitioned to a different project of examining FFmpeg. FFmpeg is an open source software for handling and manipulating multimedia data. Our main goal of this new project is to look at the demuxer and muxer portions of the software to see if there are any vulnerabilities. 

FitBit Progress

Our first attempt to look at the firmware from our fitbit was to use the charging cable that came with the device and see if we could get any data when plugged into a computer. The original charging cable was not able to transmit data between the device and the computer because the charging cable only had two prongs that we are assuming are used as ground and power. This method was not useful so we looked into the documentation online to see if any of the firmware was on the website. The devices that we were working on were from the online retailer aliexpress. Since these fitbits were offbrand and not the original fitbit, we did not find any information online for the firmware or other documentation online. after these attempts were exhausted we resorted to opening up our watch and finding the flash chip. 

When we opened our device, we did not find any sort of debugging ports on the board so we looked at all of the chips on the board. When taking a closer look at the different hardware elements, we saw a timing crystal plus other hardware for plugging in power and other sensors. We also found a chip that had different numbers on it. After some extensive online research, we came to the conclusion that this chip was the flash chip. Our research on the numbers written on the flash chip gave us only that the chip had bluetooth capabilities. We carefully took the chip off the board and then went to find a socket programmer so that we could examine the contents of the chip. Finding the correct sized socket programmer was difficult and ultimately became the reason that we switched projects half way through the semester.

FFmpeg Progress

FFmpeg is an open source program used by many big companies. as it is used widley across the internet, it has also been fuzzed by a lot of people. When we began to look at this software program, we needed to first install the software to our machine and then use a type of program analysis on the program. Installing FFmpeg turned out to be a little more difficult than originally thought. We were having problems compiling the program but we were able to find a compilation guide on the FFmpeg website that allowed us to succesfully compile the program so that we could use it and analyze it. 

Our first step was to look at the source code and do some static analysis. So we loaded the file onto Ghidra and we went through some parts of the code to try and understand exactly how it worked, our first goal was to find the main function using libc_start_main and understand the function flow from main. 

Here is a picture of ghidra in the main function.

[ghidra](./Images/ghidra.png)

However, it was hard to find anything exploitable considering that it has over 260,000+ lines of executable code. So we decided to understand the structure of the files for ffmpeg to narrow the areas of focus. 

Our research and analysis lead us to understanding the following about the main libraries of ffmpeg:

Libavutil - core tools
Libavformat - media formats
Libavcodec - codecs (encoding/decoding library)
Libavdevice - special devices (muxing/demuxing library)
Libavfilter, libswresample, libpostproc, libswscale - post-processing

Another important factor to look at is the coverage in the code. We found resources that displayed the coverage by function and by file(these links are in the FFmpeg sources file). Considering that the overall coverage was 

After we compiled the program we needed to begin our analysis. Since this program has been fuzzed by many people already, we started by looking into some of the work already done for program analysis. Some bigger companies that have worked with FFmpeg include AFL++ and Google's OSS-FUZZ project. Like some projects before, we decided to use AFL++ to fuzz FFmpeg. We did not originally have a seed corpus that worked for our project but after researching more about the previous fuzzing projects on FFmpeg, we were able to find some examples that helped us set up an environment as a proof od concept. 

Our next steps after proving that we could fuzz FFmpeg with AFL++ is to create a harness for FFmpeg so that we can look at just certain parts of the code. Once we started examining the code for FFpeg, we realized that there was too much to look at as a whole so we needed to find one or two portions of the code to focus on. For us, we wanted to look closely at the demuxing and muxing parts of the code. To fuzz these specific portions, we needed to create a harness for FFmpeg that will work with AFL++. while we were working on the harness, we also were getting a better understanding on how FFmpeg works. 

Looking at the code and other online resources, we got more knowledge on how FFmpeg interacts with the multimedia data that is being handled. < here is where i think we should talk about the different data structures >. 

After looking at the program as a whole, we took a deeper look at the demuxing and muxing portions of the code to get a deeper understanding of what these parts of the software are doing. First we looked at the demuxing part.
