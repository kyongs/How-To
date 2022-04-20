## SSD initialization




Fill SSD with sequential data twice. This will guarantee all available memory is filled wirh a data including factory provisioned area.


> #### **Note**
>
> Be aware of not writing partition in the _device ID_. <br>
> This command will initialze the whole SSD.


```
$ sudo dd if=/dev/zero of=/dev/**deviceID** bs=1024k
$ sudo dd if=/dev/zero of=/dev/**deviceID** bs=1024k
```



**dd** will end up with the message `No space left on device` or `out of space error`.



<br/><br/>

**Reference**

[SSD precondition](https://github.com/intel/fiovisualizer/blob/master/Workloads/Precondition/SSD%20precondition.txt) <br/>
[Device Initialization](https://github.com/meeeejin/til/blob/master/ssd/device-initialization.md)
