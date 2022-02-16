
 

<div align=center>
<img src="../images/024434c2a29e4ab7badf82caea2a276eaf4088a46ae71b6e9d12810d630d431c.png"
     alt="这里放图片显示不出的时候出现的文字"
     style="zoom:90%"/>
<center><p>图1入口框架</p></center>
</div>

<div align=center>
<img src="../images/09b06fd4baf5dd77f675d3ed09ba170c64181cd25373e6121ff052d97e5c2774.png"
     alt="这里放图片显示不出的时候出现的文字"
     style="zoom:90%"/>
<center><p>图2 抽象功能</p></center>
</div>

## 解释说明
 抽象结构体，是要对IC deriver驱动的抽象，真对某一类型。
 - 针对某一类型。例如：Brigde 抽象是一类。Panel的抽象是一类。
 - 这样的化，Brigde会越来越臃肿。所以在定好了，接口后。后面需要外部获取实现的功能。使用command命令来调用。实现内部接偶和。



```c
typedef struct icDriverAabstractStrap {
    const char *name;
    const char *desc;
    int (*available)(void);
    int (*create)(int devindex);
} icDriverAabstractStrap;



/* Available display drivers */
static icDriverAabstractStrap *displaydriverstrap[] = {
#ifdef _DRIVER_REDRIVER
    &REDRIVER_driverstrap,
#endif
#ifdef _DRIVER_DEMUX
    &DEMUX_driverstrap,
#endif
#ifdef _DRIVER_BRIGDE
    &BRIGDE_driverstrap,
#endif
#ifdef _DRIVER_PANEL
    &PANEL_driverstrap,
#endif
    NULL
};
```
```c



/* Bridge driver bootstrap functions */

static int Bridge_Available(void)
{

// Obtain environments and check whether they are  supported   
    return(1);
}

static void Bridge_DeleteDevice(ICDriver_Device *device)
{
// Freeing abstract objects occupies resources     
    free(device);
}

static int Bridge_CreateDevice(int devindex)
{
    ICDriver_Brigde *device;
 // step 1: Allocates memory for an object.
    /* Initialize all variables that we clean on shutdown */
    device = (ICDriver_Brigde *)malloc(sizeof(ICDriver_Brigde));
    if (device) 
    {
        memset(device, 0, (sizeof *device));
    }
    else
    {
      return -1;
    } 
// step 2: Initialze the object instance and set the specific IC driver
     device->Nature = &BRIGDE_NATURE;
      
//     osRegisterBusDevices()
// step 3: Initialze the object instance and set the specific IC driver      
    /* Set the function pointers */
    device->Init = Bridge_DriverInit;
    device->Quit = Bridge_deDriverInit;
    device->Power_On = Bridge_Power_On;
    device->Power_Off = Bridge_Power_Off;
    device->Reset = Bridge_Reset;
    device->Sleep = Bridge_Sleep;
    device->Wakeup = Bridge_Wakeup;
    device->free = Bridge_DeleteDevice;
    return 0;
}

icDriverAabstractStrap BRIGDE_driverstrap = {
    BRIDGE_DRIVER_NAME, "bridge icdriver",
    Bridge_Available, Bridge_CreateDevice
};


static int BRIDGE_DriverInit(_THIS)
{
   osRegisterBusDevices(this-> Nature)；
}

static int BRIDGE_deDriverInit(_THIS)
{
   osUnRegisterBusDevices(this-> Nature)；

}


```
```c
#ifndef _ICDRIVER_DRIDGE_H
#define _ICDRIVER_DRIDGE_H

#define _THIS    ICDriver_Brigde *_this


struct ICDriver_Nature
{
    /* * * */
    /* The name of this IC driver */
    const char *name;
   /* The Address of this IC driver  in ther rtems*/
    const char *osNodeAddress;
   /* The buslds is used for slecet which  bus */
     int *buslds;
   /* The hwaddress is ic driver address in the bus */  
     int *hwaddress;
     /* The pointer of  instantiate  ic driver control  */
    const rtems_libi2c_drv_t *icDeriverProtocolDesc; 
}   

typedef struct _ICDriver_Abstract{
    /* * * */
    struct ICDriver_Nature *Nature;

    /* * * */    
    /* Initialize the object that icDriver abstract struct  */
    int (*Init)(_THIS,...));
    int (*Quit)(_THIS,...));
    /* Reverse the effects DriverInit() -- called if DriverInit() fails or the application is shutting down 
    */
     /* * * */
    int (*Power_On)(void *arg,...);
    int (*Power_Off)(void *arg,...);
    int (*Reset)(void *arg,...);
    int (*Sleep)(void *arg,...);
    int (*Wakeup)(void *arg,...);
    /* * * */
    int (*custom_cmd)(void *arg,...); 
   
     /* The function used to dispose of this structure */
    void (*free)(_THIS);
}ICDriver_Display;

   ICDriver_Display *_Driver_DeMux = NULL;
   ICDriver_Display *_Driver_Redriver = NULL;
   ICDriver_Display *_Driver_Bridge = NULL;
   ICDriver_Display *_Driver_Panel = NULL;
  
/* * * */ 
/* Initialize the object that icDriver abstract struct  */
rtems_status_code (*Initialization)(rtems_device_major_number major ,rtems_device_major_number maior ,void *arg);   
rtems_status_code (*Open)(rtems_device_major_number major ,rtems_device_major_number maior ,void *arg);
rtems_status_code (*Close)(rtems_device_major_number major ,rtems_device_major_number maior ,void *arg);
rtems_status_code (*Read)(rtems_device_major_number major ,rtems_device_major_number maior ,void *arg);
rtems_status_code (*Write)(rtems_device_major_number major ,rtems_device_major_number maior ,void *arg);
rtems_status_code (*Control)(rtems_device_major_number major ,rtems_device_major_number maior ,void *arg);

#endif // _ICDRIVER_DRIDGE_H

```

## 伪代码流程说明









