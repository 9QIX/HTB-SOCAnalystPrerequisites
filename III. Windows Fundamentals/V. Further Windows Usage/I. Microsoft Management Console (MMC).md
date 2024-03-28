# Microsoft Management Console (MMC)

The MMC can be used to group snap-ins, or administrative tools, to manage hardware, software, and network components within a Windows host. It has been around since Windows Server 2000 and runs on all Windows versions. We can also use MMC to create custom tools and distribute them to users. MMC works with the concept of snap-ins, allowing administrators to create a customized console with only the administrative tools needed to manage several services. These snap-ins can be added to manage both local and remote systems.

We can open MMC by just typing `mmc` in the Start menu. When we open MMC for the first time, it will be blank.

![Blank MMC](/Images/image-27.png)

From here, we can browse to `File` --> `Add or Remove Snap-ins`, and begin customizing our administrative console.

![Add or Remove Snap-ins](/Images/image-28.png)

As we begin adding snap-ins, we will be asked if we want to add the snap-in to manage just the local computer or if it will be used to manage another computer on the network.

![Manage Snap-ins](/Images/image-29.png)

Once we have finished adding snap-ins, they will appear on the left-hand side of MMC. From here, we can save the set of snap-ins as a `.msc` file, so they will all be loaded the next time we open MMC. By default, they are saved in the Windows Administrative Tools directory under the Start menu. Next time we open MMC, we can choose to load any of the views that we have created.

![Snap-ins in MMC](/Images/image-30.png)
