---
title: Tennis night with Hammar and Carl
date: 2020-11-24
---

![Photo of Hammar and Carl](2020-11-24 photo-of-hammar-and-carl.jpg)

Me, Hammar and Carl went for a late night tennis session @ [Good to Great Academy](https://goodtogreat.se/en/) in Danderyd. In contrast to the murky clubs we usually play at (which we like) this club was super fancy and even offered free recordings through a company called [PlayReplay](https://playreplay.io/).

> PlayReplay doesn't have an About page on their [website](https://playreplay.io/) what I can tell, but it seems like it's a swedish company.

There was a touchscreen on the court where you could create an account with PlayReplay by simply entering your email and password. That's it. Then it sent the recording to your email after the session finished. Pretty cool. I edited out some highlights that you can see below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/HhuS33CzD4A" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To edit the video I looked through the video and made notes of the segments I'd like to keep. I then used a [python script](https://stackoverflow.com/a/58059148/794103) together with `ffmpeg` to split the video into multiple segments, and then concatenate it into a single file. Below is the script in it's entirety.

```python
#!/usr/bin/env python3

import subprocess

def get_duration(input_video):
    cmd = ["ffprobe", "-i", input_video, "-show_entries", "format=duration",
           "-v", "quiet", "-sexagesimal", "-of", "csv=p=0"]
    return subprocess.check_output(cmd).decode("utf-8").strip()


if __name__ == "__main__":
    name = "input.mp4"

    times = []
    times.append(["00:18:33", "00:18:41"]) # Carl, Hammar, dubbel nätstuds
    times.append(["00:21:47", "00:22:00"]) # Victor slår en bra poäng mot Hammar
    times.append(["00:22:40", "00:23:00"]) # Victor slår en bra poäng mot Carl
    times.append(["00:23:23", "00:23:36"]) # Victor slår en bra poäng mot Hammar
    times.append(["00:23:38", "00:23:48"]) # Hammar slår Victor med en bra poäng (direkt efter)
    times.append(["00:24:30", "00:24:39"]) # Carl slår Victor med en bra forehand
    times.append(["00:25:09", "00:25:21"]) # Bra bollduell mellan Victor och Hammar
    times.append(["00:25:26", "00:25:44"]) # Bra boll mellan Hammar och Carl
    times.append(["00:26:00", "00:26:01"]) # Carlos visar upp sina fotbolls-skills
    times.append(["00:28:57", "00:29:02"]) # Hammar BACKHAND
    times.append(["00:29:18", "00:29:25"]) # Hammar BACKHAND
    times.append(["00:29:37", "00:29:54"]) # Carl slår Hammar på nät
    times.append(["00:32:29", "00:32:47"]) # Bra boll mellan Hammar och Victor
    times.append(["00:33:58", "00:34:08"]) # Äcklig stoppboll av Carlos
    times.append(["00:35:04", "00:35:12"]) # Vattenpaus
    times.append(["00:37:04", "00:37:19"]) # Carlos yxkast-lopp över Hammar
    times.append(["00:38:05", "00:38:15"]) # Victor lobbar Hammar
    times.append(["00:38:58", "00:39:06"]) # Hammar BACKHAND
    times.append(["00:39:42", "00:39:49"]) # Hammar FOREHAND
    times.append(["00:40:37", "00:40:41"]) # Hammar är HELT KLAR
    times.append(["00:41:06", "00:41:15"]) # Hammar BACKHAND
    times.append(["00:41:36", "00:41:46"]) # Hammar BACKHAND (backhand sitter bra idag)
    times.append(["00:42:24", "00:42:32"]) # Carlos drar strängarna
    times.append(["00:43:17", "00:43:28"]) # Victor vinner över Hammar med smash
    times.append(["00:48:04", "00:48:14"]) # Carlos kastar racket
    times.append(["00:48:45", "00:48:56"]) # Victor vinner över Hammar
    times.append(["00:50:06", "00:50:17"]) # Carlos vinner över Hammar
    times.append(["00:50:55", "00:51:04"]) # Carlos slår igenom Hammar
    times.append(["00:53:01", "00:53:23"]) # Två bra bollar mellan Victor och Hammar
    times.append(["00:54:53", "00:54:56"]) # Fist bumps

    open('concatenate.txt', 'w').close()
    for idx, time in enumerate(times):
        output_filename = f"output{idx}.mp4"
        cmd = ["ffmpeg", "-i", name, "-ss", time[0], "-to", time[1], "-c:v", "copy", "-c:a", "copy", output_filename]
        subprocess.check_output(cmd)

        with open("concatenate.txt", "a") as myfile:
            myfile.write(f"file {output_filename}\n")

    cmd = ["ffmpeg", "-f", "concat", "-i", "concatenate.txt", "-c", "copy", "output.mp4"]
    output = subprocess.check_output(cmd).decode("utf-8").strip()
```
