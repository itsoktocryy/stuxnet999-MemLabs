# Memlabs by Stuxbet99
## Challenge description:
### My sister's computer crashed. We were very fortunate to recover this memory dump. Your job is get all her important files from the system. From what we remember, we suddenly saw a black window pop up with some thing being executed. When the crash happened, she was trying to draw something. Thats all we remember from the time of crash.

#### Note: This challenge is composed of 3 flags.

## PREPA:
#### We extract a .raw dump file to analyze with volatility.

#### First of all we need to check which volatility profile we're going to use:
![1](https://user-images.githubusercontent.com/73375576/225740646-a0124c20-7938-4cd9-9f6a-070efbc1d35d.png)
#### I used Win7SP1x64.

## FLAG1:
#### $ python2 /volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles > consoles.log
![2](https://user-images.githubusercontent.com/73375576/225742118-e85c0f4e-35d9-45fa-9ac6-efcebaed080e.png)
#### (Using this plugin will not only tell us the commands typed in cmd.exe but it will show us the entire input output buffer).
#### • The scan reveals a base64 encoded string. Decode it and grab first flag.
#### $ echo "ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=" | base64 -d

## FLAG2:
#### • We need to retrieve as much as info as possible from this broken machine. To do so I listed all processes again with:
#### $ python2 /volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 pslist > pslist.log
#### • Inspecting the scan result I saw an executable of microsoft paint that might have data we need to retrieve. Dumped the process with:
#### $ python2 /volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 memdump --pid 2424 --dump-dir /dumps/MsPaint
#### • Since this dump is from MsPaint we could try to retrive data by looking at it as raw image data. To do so:
#### $ touch 2424.data && mv 2424.dmp 2424.data
#### • By inspecting in gimp the raw image data for a long time and scrolling around with offset I saw something that looked like handwritten letters. Got a clear image of the string by adjusting the position values in gimp.
![3](https://user-images.githubusercontent.com/73375576/225748844-e87520e6-790b-4c19-a499-74b8cdb8bd83.png)
#### • That looks like a mirrored flag !!! I got lazy to mirror it on gimp so I used a real one xD.
![4](https://user-images.githubusercontent.com/73375576/225749120-520ee7c0-81bf-48b6-8116-fc3a9a30c18e.png)

## FLAG3:
#### $ cat pslist.log
#### • Listed again all processes and saw a compressed file named "Important.rar", that looks important xD.
#### • To dump a process we need his address.
![5](https://user-images.githubusercontent.com/73375576/225750575-28bdef65-6dd1-4f51-a1cb-ff6a65bacef4.png)
#### $ python2 /volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 dumofiles -Q 0x000000003fa3ebc0 --dump-dir /dumps/Important_rar
#### $ grep -i flag -r *
#### • By grepping, found a png called flag3.png might worth to check idk. Let's try to rebuid the .rar file from the .dat file we retrieved.
![7](https://user-images.githubusercontent.com/73375576/225753312-719ee75c-9875-4dd7-9c2a-9b501bf2e498.png)
#### • But when we try to extract it, it asks us for a password...
![8](https://user-images.githubusercontent.com/73375576/225753423-ab94d3ad-a764-47a9-a98d-b17792fc8632.png)
#### • Feeling stuck I oppened the .rar file with Notepad and found a lead.
![9](https://user-images.githubusercontent.com/73375576/225753657-123a8805-eb9d-4050-bb58-ae5536e0bbdb.png)
#### • Using the hashdump plugin I recovered all hashes found in memory.
![10](https://user-images.githubusercontent.com/73375576/225753932-478af5cd-daba-4821-bc7d-318d20ec868e.png)
#### • And now we can unlock and extract the compressed file. And there was indeed a flag3.png inside!
![11](https://user-images.githubusercontent.com/73375576/225754185-2807ec21-bf20-4437-bf03-655208becd93.png)
