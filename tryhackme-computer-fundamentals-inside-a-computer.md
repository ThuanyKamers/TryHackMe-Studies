# TryHackMe - Computer Fundamentals: Inside a Computer

## Case briefing
> A workstation was pulled from a breach scene. The suspect was gone before anyone arrived. All you have is the machine, in pieces, and a case file with notes from the scene.
>
> Some of the recovered components aren't from this machine. To rebuild it, you'll need to know what each part does, how it connects to the motherboard, and read the evidence closely enough to tell what belongs.

**Objective:** Assemble the machine, boot it, find the evidence file, and email it to the senior analyst to close the case.

## Task: Assembling the machine
The computer needs 7 parts. Here's everything laid out at the start. I can see the case, all the candidate parts, and the monitor waiting for the machine to be assembled.

![Lab overview with all components laid out next to the open case, monitor showing "Awaiting machine assembly..."](images/image-27.png)

I start by opening each part to check its description. The first one I pick up is a GPU, but its card says it's integrated into a laptop motherboard, so it's discarded since it isn't part of this PC.

![Laptop Graphics Processing Unit (GPU) item card, marked as integrated into a laptop motherboard, not used](images/image-28.png)

Next, the CPU. Its card confirms it processes every instruction and that the machine can't function without it, so I install it in the correct socket.

![Central Processing Unit (CPU) item card, showing its function and the CPU socket on the motherboard as its position](images/image-29.png)

![Close-up of the CPU being placed into its socket](images/image-30.png)

![Close-up of the CPU installed on the motherboard](images/image-31.png)

I see a laptop power charger next. Its card shows it's meant to plug into an external laptop, not this machine, so I discard it too.

![Laptop Power Charger (AC Adapter) item card, marked as external, connected to a wall outlet, not used](images/image-32.png)

I find a stick of RAM. I already know it belongs, so I install it in the DIMM slots.

![Random Access Memory (RAM) item card, showing its function and the DIMM slots on the motherboard as its position](images/image-33.png)

![Close-up of the RAM installed on the motherboard](images/image-34.png)

I see the HDD and put it in the drive bay.

![Hard Disk Drive (HDD) item card, showing its function and the drive bay inside the chassis as its position](images/image-35.png)

![Close-up of the HDD installed inside the chassis](images/image-36.png)

The (desktop) graphics card also goes in the PC.

![Desktop Graphics Processing Unit (GPU) item card, showing its function and the PCIe x16 slot as its position](images/image-37.png)

![Close-up of the GPU installed in the PCIe slot](images/image-38.png)

3 parts are still missing. The Input/Output panel is one of the missing parts of the PC.

![Input/Output Panel (I/O) item card, showing its function and the rear I/O slot as its position](images/image-39.png)

![Close-up of the I/O panel installed at the rear of the chassis](images/image-40.png)

The PSU is also needed.

![Power Supply Unit (PSU) item card, showing its function and the edge of the chassis as its position](images/image-41.png)

![Close-up of the PSU installed at the edge of the chassis](images/image-42.png)

The last part is the network adapter.

![Network Adapter item card, showing its function and the PCIe x1 slot as its position](images/image-43.png)

With all the parts connected, the power button lights up.

![Close-up of the power button lit up on the case](images/image-44.png)

I see the case file, which I hadn't opened until now:

![Case file - Parts & Function, showing CPU, RAM, PSU, Laptop Power Charger and Network Adapter](images/image-25.png)

![Case file - Parts & Function, showing Wi-Fi Dongle, I/O Panel, SSD, HDD, GPU and Laptop GPU](images/image-26.png)

Everything looks right. Only the hard disk drive doesn't show as checked for some reason, but it was installed.

## Task: Powering on with the wrong drive selected
I power the machine on, and this message appears on the screen:

![Boot error screen: WARNING - Mechanical drive detected. Wrong startup drive selected. The computer is using an older mechanical drive (the HDD) as its main storage](images/image-45.png)

I had missed an SSD among the parts.

![Solid State Drive (SSD) item card, labeled THM COLDBOOT 2TB, showing its function and the M.2 slot as its position](images/image-46.png)

![Close-up of the SSD installed in the M.2 slot](images/image-47.png)

With it installed, I power the machine on again.

![Boot screen showing Startup Check Complete, OS Located on SSD, OS Loading into RAM, and "SYSTEM READY. ACCESSING DESKTOP."](images/image-48.png)

Everything checks out. The computer boots into the desktop.

![Desktop showing an "Evidence" folder icon](images/image-49.png)

## Task: Evidence quiz
Opening the Evidence folder brings up a 4-question quiz about the components I just installed.

**Question:** What is the primary function of the Graphics Processing Unit (GPU)?

![Quiz question 1 of 4: primary function of the GPU, with 4 multiple-choice options](images/image-50.png)

**Answer:** `c` - Renders images and visual output to the screen.

**Question:** What does the Power Supply Unit (PSU) do?

![Quiz question 2 of 4: function of the PSU, with 4 multiple-choice options](images/image-51.png)

**Answer:** `b` - Converts mains electricity into the voltages each component needs.

**Question:** What happens if the Central Processing Unit (CPU) is missing?

![Quiz question 3 of 4: what happens if the CPU is missing, with 4 multiple-choice options](images/image-52.png)

This was an interesting question. I hadn't actually tried booting the computer with parts missing. I could have done that to see what happens with each component, but based on what the case file said about the CPU, nothing else makes sense.

**Answer:** `c` - Nothing executes. The machine cannot function at all.

**Question:** What is the role of the Motherboard (MOBO)?

![Quiz question 4 of 4: role of the motherboard, with 4 multiple-choice options](images/image-53.png)

**Answer:** `c` - It connects and routes communication between all components.

## Task: Recovering and sending the evidence
After the quiz, the evidence file opens showing the recovered flag.

![Notepad showing evidence_cb0047.txt - a Forensic Recovery Report confirming the suspect accessed the network via a compromised VPN, with the recovered flag](images/image-54.png)

**Answer:** `THM{c0ld_b00t_c0mpl3t3}`

I sent it to the senior analyst, explaining the situation.

![Email to the senior analyst reporting the completed cold boot forensic recovery, with the recovered flag](images/image-55.png)

Room complete.

## What I learned
- Each hardware component (CPU, RAM, GPU, PSU, HDD/SSD, I/O panel, network adapter) has one specific job and one correct place on the motherboard or chassis. Knowing both is what lets you tell a legitimate part from one that doesn't belong (like a laptop-only GPU or charger).
- The CPU is the one component without which nothing executes at all. It's what "processes every instruction," so the whole machine depends on it.
- Storage type matters for boot behavior: an older mechanical HDD can technically work as a boot drive, but the system flagged it as the "wrong startup drive" in favor of a faster SSD.
- The motherboard's role is to connect and route communication between every other component. It's the piece that ties the whole machine together, not just a mounting surface.
- Reading documentation/evidence closely (the case file, the parts list) before acting saves time and avoids wasting effort on components that don't belong on the build.
