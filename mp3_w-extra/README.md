# Xunil

The kernel we designed is named as `Xunil`, which is just a reversion of `Linux`. 

All four members in the team contribute to this kernel.

# Table of contents
1. [Project Folder Structure](#structure)
2. [Environment w/ configuration](#env)
3. [Build and Run](#build)
4. [Demo](#demo)
    1. [Boot Animation](#animation)
    2. [Desktop](#desk)
    3. [Terminal](#term)
    4. [Partial Functionality](#func)
5. [ACADEMIC INTEGRITY](#acad)





## Project Folder Structure<a name="structure"></a>

Here shows the file tree of our main project folder `student-distrib` in a level of 2.

Some files and folder are omitted.

```sh
student_distrib
├── data	# Place the necessary data file for boot animation, icon and desktop.
│   ├── boot_imgs.c
│   ├── ......
├── drivers	# Place the driver codes like keyboard/mouse driver, rtc/pit driver, PCIe scanner as well as the VGA driver (the so called VBE)
│   ├── filesystem.c
│   ├── filesystem.h
│   ├── i8259.c
│   ├── i8259.h
│   ├── keyboard.c
│   ├── keyboard.h
│   ├── mouse.c
│   ├── mouse.h
│   ├── pci.c
│   ├── pci.h
│   ├── pit.c
│   ├── pit.h
│   ├── rtc.c
│   ├── rtc.h
│   ├── statusbar.c
│   ├── statusbar.h
│   ├── terminal.c
│   ├── terminal.h
│   ├── vbe.c
│   └── vbe.h
├── kernel	# Place the kernel feature codes like paging and scheduler.
│   ├── asm_linkage.S
│   ├── asm_linkage.h
│   ├── idt.c
│   ├── idt.h
│   ├── paging.c
│   ├── paging.h
│   ├── paging_asm.S
│   ├── pcb.c
│   ├── pcb.h
│   ├── pcb_asm.S
│   ├── schedule.c
│   ├── schedule.h
│   ├── schedule_asm.S
│   ├── system_call.c
│   ├── system_call.h
│   └── system_call_asm.S
```



## Environment w/ configuration<a name="env"></a>

> For general environment preparation, please refer to the **mp0 document** provided to you at the very beginning of this course.

I build and test this kernel in a Ubuntu Virtural Machine with version `Ubuntu 20.04`.

A `QEMU emulator` provided by the course will be needed to execute our kernel. Please refer to the course materials for the version and other details.

To make our kernel work with Graphical User Interface (GUI), you need to add standard VGA PCIe feature `-vga std` in the QEMU configuration file. Mine is shown below:

```sh
#!/bin/sh
"/home/fuguanshujie/Desktop/ece391/qemu/bin/qemu-system-i386" -hda "/home/fuguanshujie/Desktop/ece391/ece391_share/work/mp3_group_27/student-distrib/mp3.img" -m 256 -gdb tcp:127.0.0.1:1234 -S -name mp3 -vga std
```

> You should do the same thing if you try to utilize **any PCIe devices** including NIC and sound card. 

## Build and Run<a name="build"></a>

- `make install` and `make` to compile our kernel 
  - it supposes to work smoothly, but actually you do not need to do this step since we have uploaded a compiled kernel img: `./student_distrib/mp3.img`.

- relate our kernel img to your QEMU emulator by modify the configuration file.
- open GDB by `gdb vmlinux` and type `target remote 10.0.2.2:1234` to connect to our kernel image. 
  - You can also modify the QEMU configuration file to disable GDB connection by deleting `-gdb tcp:127.0.0.1:1234` such that you only need to double click the QEMU icon to run our kernel. But I do not recommend to do so.

- Enter `c` or `continue` to execute our kernel.



## Demo<a name="demo"></a>

### Boot Animation<a name="animation"></a>

The boot animation is designed with a state machine and last for 10 seconds. The frame rate is based on pit frequency. 

The following `gif` is converted from recorded video `mov`, which is slower than it supposes to be. You can also see the mouse cursor implemented in our kernel. 

![](../readme_img/animation.gif)

### Desktop<a name="desk"></a>

The desktop contains a backgroud, a status bar with a terminal icon on the left-up corner and a timer on the right-up corner. I planned to design a real clock, but GUI took too much time from me.

![](../readme_img/desktop.png)



### Terminal<a name="term"></a>

Use the mouse cursor to click the terminal button (the mouse is a little bit hard to use, do not use your touch pad), you enter into the terminal interface. The terminal is converted from text mode into graphical mode. 

![](../readme_img/terminal.png)



### Partial Functionality<a name="func"></a>

All functionalities required by the MP3 are realized. I will just show something here.

- The `fish` command:

![](../readme_img/fish.png)

- Another terminal and `ls` command:

![](../readme_img/terminal2.png)

- A text editor `svim`. We implemented a simple version `vim` and hence we call it `svim`. It can be used to modify the file and save it, for instance, `frame0.txt`:

![](../readme_img/svim.png)

For other features or commands like `scheduler`, `cmd history`, `tab func` and etc., I will not show them here.

> **Write in the end:**
>
> This project took a great amount of time. We added detailed comments in our code and I also added reference to any other code or documents we refered to.
>
> The project actually did not meet my expectation and the extra credits we earned for this project was around 4 or 5 out of 10 according to a TA in this course. This can be a good reference if you are planning your work or try to estimate your points.
>
> Anyway, if you find any problem in our code (there should have many undiscovered bugs) or have any question towards our code, please feel free to contact. I will see if I can help. 
>
> That's basically it. 

## ACADEMIC INTEGRITY<a name="acad"></a>

Please review the University of Illinois Student Code before starting,
particularly all subsections of Article 1, Part 4 Academic Integrity and Procedure [here](http://studentcode.illinois.edu/article1_part4_1-401.html).

**§ 1‑402 Academic Integrity Infractions**

(a).	Cheating. No student shall use or attempt to use in any academic exercise materials, information, study aids, or electronic data that the student knows or should know is unauthorized. Instructors are strongly encouraged to make in advance a clear statement of their policies and procedures concerning the use of shared study aids, examination files, and related materials and forms of assistance. Such advance notification is especially important in the case of take-home examinations. During any examination, students should assume that external assistance (e.g., books, notes, calculators, and communications with others) is prohibited unless specifically authorized by the Instructor. A violation of this section includes but is not limited to:

(1)	Allowing others to conduct research or prepare any work for a student without prior authorization from the Instructor, including using the services of commercial term paper companies. 

(2)	Submitting substantial portions of the same academic work for credit more than once or by more than one student without authorization from the Instructors to whom the work is being submitted. 

(3) Working with another person without authorization to satisfy an individual assignment.

(b) Plagiarism. No student shall represent the words, work, or ideas of another as his or her own in any academic endeavor. A violation of this section includes but is not limited to:

(1)	Copying: Submitting the work of another as one’s own. 

(2)	Direct Quotation: Every direct quotation must be identified by quotation marks or by appropriate indentation and must be promptly cited. Proper citation style for many academic departments is outlined in such manuals as the MLA Handbook or K.L. Turabian’s A Manual for Writers of Term Papers, Theses and Dissertations. These and similar publications are available in the University bookstore or library. The actual source from which cited information was obtained should be acknowledged.

(3)	Paraphrase: Prompt acknowledgment is required when material from another source is paraphrased or summarized in whole or in part. This is true even if the student’s words differ substantially from those of the source. A citation acknowledging only a directly quoted statement does not suffice as an acknowledgment of any preceding or succeeding paraphrased material. 

(4)	Borrowed Facts or Information: Information obtained in one’s reading or research that is not common knowledge must be acknowledged. Examples of common knowledge might include the names of leaders of prominent nations, basic scientific laws, etc. Materials that contribute only to one’s general understanding of the subject may be acknowledged in a bibliography and need not be immediately cited. One citation is usually sufficient to acknowledge indebtedness when a number of connected sentences in the paper draw their special information from one source.

(c) Fabrication. No student shall falsify or invent any information or citation in an academic endeavor. A violation of this section includes but is not limited to:

(1)	Using invented information in any laboratory experiment or other academic endeavor without notice to and authorization from the Instructor or examiner. It would be improper, for example, to analyze one sample in an experiment and covertly invent data based on that single experiment for several more required analyses. 

(2)	Altering the answers given for an exam after the examination has been graded. 

(3)	Providing false or misleading information for the purpose of gaining an academic advantage.

(d)	Facilitating Infractions of Academic Integrity. No student shall help or attempt to help another to commit an infraction of academic integrity, where one knows or should know that through one’s acts or omissions such an infraction may be facilitated. A violation of this section includes but is not limited to:

(1)	Allowing another to copy from one’s work. 

(2)	Taking an exam by proxy for someone else. This is an infraction of academic integrity on the part of both the student enrolled in the course and the proxy or substitute. 

(3)	Removing an examination or quiz from a classroom, faculty office, or other facility without authorization.

(e)	Bribes, Favors, and Threats. No student shall bribe or attempt to bribe, promise favors to or make threats against any person with the intent to affect a record of a grade or evaluation of academic performance. This includes conspiracy with another person who then takes the action on behalf of the student.

(f)	Academic Interference. No student shall tamper with, alter, circumvent, or destroy any educational material or resource in a manner that deprives any other student of fair access or reasonable use of that material or resource. 

(1)	Educational resources include but are not limited to computer facilities, electronic data, required/reserved readings, reference works, or other library materials. 

(2)	Academic interference also includes acts in which the student committing the infraction personally benefits from the interference, regardless of the effect on other students.


LEGAL
-----
Permission to use, copy, modify, and distribute this software and its
documentation for any purpose, without fee, and without written agreement is
hereby granted, provided that the above copyright notice and the following
two paragraphs appear in all copies of this software.

IN NO EVENT SHALL THE AUTHOR OR THE UNIVERSITY OF ILLINOIS BE LIABLE TO
ANY PARTY FOR DIRECT, INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL
DAMAGES ARISING OUT  OF THE USE OF THIS SOFTWARE AND ITS DOCUMENTATION,
EVEN IF THE AUTHOR AND/OR THE UNIVERSITY OF ILLINOIS HAS BEEN ADVISED
OF THE POSSIBILITY OF SUCH DAMAGE.

THE AUTHOR AND THE UNIVERSITY OF ILLINOIS SPECIFICALLY DISCLAIM ANY
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.  THE SOFTWARE

PROVIDED HEREUNDER IS ON AN "AS IS" BASIS, AND NEITHER THE AUTHOR NOR
THE UNIVERSITY OF ILLINOIS HAS ANY OBLIGATION TO PROVIDE MAINTENANCE,
SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS."
