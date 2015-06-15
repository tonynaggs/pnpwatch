Plug and Play Watch
===================

Plug and Play notification watcher for MS Windows (NT, 2000, XP, Vista, 7, 8, ...)

This is a small Windows utility, that reports and decodes Plug and Play events as they
happen, e.g.: CD-ROMs being inserted or removed; PCMCIA cards or USB devices being
inserted or removed; connection or removal from a laptop dock; or changes in the
configuration of the Ethernet, modem etc... .

Version 2.0 has been updated & tidied in 2014 for use on newer versions of MS Windows.
This program is fairly simple and written in C, using the Windows SDK interface.
Mostly this is historic as the first version was written & distributed this way, but
also I feel that MFC (Microsoft Foundation Classes for C++) would obscure the basic
use of the Plug and Play functions. I have tried to make the code readable & maintainable,
my preferred coding style is somewhat close to MISRA C guidelines.

Older versions suitable for Windows 95/98 were previously distributed, and can be
found in the pnp-watch-win95 directory.
1997-11-05 Version 1.1   pnpwat11.zip
1998-11-22 Version 1.2   pnpwat12.zip


Programming Windows with Plug and Play
======================================

Plug and Play events are notified to the top level window of an application 
through the WM_DEVICECHANGE message. In Window 95 and NT <= 4.0 all event
notifications are sent to all applications. From Windows 98 and 2000 a
reduced set of events are sent to all applications (e.g. DBT_DEVNODES_CHANGED,
arrival & removal notifications for disks), the program must request
additional events through RegisterDeviceNotification().

WM_DEVICECHANGE Notifications
-----------------------------

The wParam of the Windows message indicates what the event notification is.

In general if the hex value of the notification begins with 0x80?? the 
notification will have DBT_DEV_... structure describing the associated device.
Hex values starting with 0x40?? would have a string describing the device,
but this only happens in Windows 95/98/ME kernel & device drivers.


* DBT_CONFIGCHANGECANCELED = 0x0019
  lParam = 0;
  Some program cancelled the most recently announced DBT_QUERYCHANGECONFIG.

* DBT_CONFIGCHANGED = 0x0018
  lParam = 0
  The most recently announced DBT_QUERYCHANGECONFIG succeeded.

* DBT_CUSTOMEVENT = ..

* DBT_DEVICEARRIVAL = 0x8000
  lParam points to a DEV_BROADCAST_* structure.
  A new device or interface has been connected or configured.
  For Windows XP and later all programs are notified of disk
  arrival and removal, but the program must request notifications
  for other devices through the RegisterDeviceNotification() api.

* DBT_DEVICEQUERYREMOVE = 0x8001
  lParam points to a DEV_BROADCAST_* structure.
  Check if it is safe for device to be removed.

* DBT_DEVICEQUERYREMOVEFAILED = 0x8002
  lParam points to a DEV_BROADCAST_* structure.
  Some program objected to DBT_DEVICEQUERYREMOVE.

* DBT_DEVICEREMOVEPENDING = 0x8003
  lParam points to a DEV_BROADCAST_* structure.
  Device removal will happen shortly, last chance to flush file
  writes & close.

* DBT_DEVICEREMOVECOMPLETE = 0x8004
  lParam points to a DEV_BROADCAST_* structure.
  Device has been removed.
  For Windows XP and later all programs are notified of disk
  arrival and removal, but the program must request notifications
  for other devices through the RegisterDeviceNotification() api.

* DBT_DEVICETYPESPECIFIC = 0x8005
  lParam may be 0 or point to a DEV_BROADCAST_* structure.
 "Type Specific" event. (More details welcome.)

* DBT_QUERYCHANGECONFIG = 0x0017
  lParam = 0
  Query if it is okay to change configuration, eg between docked & undocked.

* DBT_DEVNODES_CHANGED = 0x0007
  lParam = 0
  One or more Devnodes have changed, an application may re-scan (parts of)
  the current configuration in the registry.

* DBT_MONITORCHANGE = 0x001B (Windows 95, 98, ME & NT)
  Windows 2000, and later variants broadcast WM_DISPLAYCHANGE instead.
  lParam has new screen resolution, low word is X and high word is Y.
  Monitor connection or resolution setting has changed.

* DBT_USERDEFINED = 0xFFFF
  lParam may be 0 or point to a DEV_BROADCAST_* structure.
  "User Defined" event.

RegisterDeviceNotification
--------------------------
To get more than the global 
   HDEVNOTIFY WINAPI RegisterDeviceNotification(
      HANDLE hRecipient,
      LPVOID NotificationFilter,
      DWORD Flags
   );

For an appication hRecipient is a window handle, normally the top
level window of the application which will already get the default
notification broadcasts.

For a Windows service hRecipient is a handle to the service, the
service must have called RegisterServiceCtrlHandlerEx (rather than
RegisterServiceCtrlHandler) in its ServiceMain function.



To be continued ...

