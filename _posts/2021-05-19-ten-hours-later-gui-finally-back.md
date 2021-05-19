---
tags: [ CUDA, Ubuntu  ]
title:  Ten hours later, GUI finally back
---

 
I got lucky that GUI has been recovered after 10h troubleshooting.


# What happend
These days I am learning a book called *Deep Learning with Python*. The author recommended that Ubuntu is preferred for running the codes than windows. And my mate also told me that the CUDA, which is designed by NVIDIA for boost tensor calculation,  was only support linux when it comes in early ears.

I have experience with Ubuntu. It's the OS that I dive in 2014.  And in my imagine the deploy process will be a piece of cake. 

So the prerequisites R all set. The target machine is Z420 with 1070Ti Graphic Card. It has two SSD. The first has installed win10. I quickly installed Ubuntu on second disk with dual boot auto configured by grub. Anaconda, PyCharm, Chrome, SublimeText  are performed seamlessly. I feel not bad and all the things seem like going better than I expected.

But after CUDA is installed, the OS seems lost GUI after reboot. I just tought maybe the doc is not officially as I followed the instructions on Tensorflow, that is the 3rd party, instead of nvidia.




# How I rescue
The first try is reinstalled by nvidia official site. But it did not work. I serached many webpages. The first try is mannualy start GUI. And it also failed. The error log info says some module does not exist. The nvidia drivers seems like installed properly and no error info found. I searced the error log. There R so many solutions, like reinstall nvidia, or even reinstall the OS. 

I thought maybe just the desktop is broken. I searched the reinstall method. And below is the method:

```
sudo apt-get install --reinstall ubuntu-desktop
```

But it also got error log and it seems like some dependency is conflict.

Maybe it's not proper way? So I stop and begin search other ways.

Other ways seems all not worked. I have to try reinstall the desktop. With some dependency conflicts solved. Desktop is installed. And the GUI instant recovered without reboot.

What a relief!





# Conclusion
It's a tough way I think. And the fastest way to dive in a new area is ask someone who has been there. I didn't seek aid from mate cause it's weekend and maybe he is having a good time and I do not want to interrupt him for this little stupid problem.

And also, patient is very important. There R some times that I do not want to continue cause it's frustrating and nobody know what happend and what is going on. 10h invest seems like a bad trade. Even it seems like some kind of experience , but I think time should not be consumed like this stupid things.

Follow things will be more tough. Found ways, improve efficient, and crack it like a beast.
