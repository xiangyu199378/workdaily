## 解释说明
 抽象结构体，是要对IC deriver驱动的抽象，真对某一类型。
 - 针对某一类型。例如：Brigde 抽象是一类。Panel的抽象是一类。
 - 这样的化，Brigde会越来越臃肿。所以在定好了，接口后。后面需要外部获取实现的功能。使用command命令来调用。实现内部接偶和。



```c
typedef struct icDriverAbstractStrap {
    const char *name;
    const char *desc;
    int (*available)(void);
    int (*create)(int devindex);
} icDriverAabstractStrap;



/* Available display drivers */
static icDriverAbstractStrap *displaydriverstrap[] = {
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
    return(1);
}

static void Bridge_DeleteDevice(ICDriver_Device *device)
{
    free(device);
}

static int Bridge_CreateDevice(int devindex)
{
    ICDriver_Brigde *device;

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
    /* register  */


    /* Set the function pointers */
    device->DriverInit = BRIDGE_DriverInit;
    device->free = BRIDGE_DeleteDevice;

    return 0;
}

icDriverAabstractStrap BRIGDE_driverstrap = {
    BRIDGE_DRIVER_NAME, "bridge icdriver",
    Bridge_Available, Bridge_CreateDevice
};


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
    struct ICDriver_Nature Nature;
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