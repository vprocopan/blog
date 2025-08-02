---
title: Run Control Panel Tools from the Command Line
---

# Run Control Panel Tools from the Command Line

This article describes how to launch Windows Control Panel tools by typing commands at a Command Prompt or in the Run dialog (Win + R).

> **Note:** To run these commands from a Command Prompt, you must be in the Windows system folder. Not all tools may be available on every Windows installation.

## Control Panel Tools

| Tool                                    | Command                                           |
|-----------------------------------------|---------------------------------------------------|
| Accessibility Options                   | `control access.cpl`                              |
| Add New Hardware                        | `control sysdm.cpl add new hardware`              |
| Add/Remove Programs                     | `control appwiz.cpl`                              |
| Date/Time Properties                    | `control timedate.cpl`                            |
| Display Properties                      | `control desk.cpl`                                |
| FindFast                                | `control findfast.cpl`                            |
| Fonts Folder                            | `control fonts`                                   |
| Internet Properties                     | `control inetcpl.cpl`                             |
| Joystick Properties                     | `control joy.cpl`                                 |
| Keyboard Properties                     | `control main.cpl keyboard`                       |
| Microsoft Exchange (Windows Messaging)  | `control mlcfg32.cpl`                             |
| Microsoft Mail Post Office              | `control wgpocpl.cpl`                             |
| Modem Properties                        | `control modem.cpl`                               |
| Mouse Properties                        | `control main.cpl`                                |
| Multimedia Properties                   | `control mmsys.cpl`                               |
| Network Properties                      | `control netcpl.cpl` <br>*(NT 4.0: `ncpa.cpl`)*    |
| Password Properties                     | `control password.cpl`                            |
| PC Card (PCMCIA)                        | `control main.cpl pc card`                        |
| Power Management (Windows 95)           | `control main.cpl power`                          |
| Power Management (Windows 98)           | `control powercfg.cpl`                            |
| Printers Folder                         | `control printers`                                |
| Regional Settings                       | `control intl.cpl`                                |
| Scanners and Cameras                    | `control sticpl.cpl`                              |
| Sound Properties                        | `control mmsys.cpl sounds`                        |
| System Properties                       | `control sysdm.cpl`                               |

## Usage Examples

To launch the Add/Remove Programs tool using `rundll32`:
```cmd
rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl
```

To open the Users tool:
```cmd
control ncpa.cpl users
```

On Windows 95/98/Me:
```cmd
control inetcpl.cpl users
```

This article describes how to run Control Panel tools in Windows by typing a command at a command prompt or in the Open box.

More Information
To run a Control Panel tool in Windows, type the appropriate command in the Open box or at a command prompt.


NOTE: If you want to run a command from a command prompt, you must do so from the Windows folder. Also, note that your computer may not have all of the tools listed in this article, as your Windows installation may not include all of these components.

Control panel tool Command

Accessibility Options control access.cpl  
Add New Hardware control sysdm.cpl add new hardware  
Add/Remove Programs control appwiz.cpl  
Date/Time Properties control timedate.cpl  
Display Properties control desk.cpl  
FindFast control findfast.cpl  
Fonts Folder control fonts  
Internet Properties control inetcpl.cpl  
Joystick Properties control joy.cpl  
Keyboard Properties control main.cpl keyboard  
Microsoft Exchange control mlcfg32.cpl  
(or Windows Messaging)  
Microsoft Mail Post Office control wgpocpl.cpl  
Modem Properties control modem.cpl  
Mouse Properties control main.cpl  
Multimedia Properties control mmsys.cpl  
Network Properties control netcpl.cpl  
NOTE: In Windows NT 4.0, Network  
properties is Ncpa.cpl, not Netcpl.cpl  
Password Properties control password.cpl  
PC Card control main.cpl pc card (PCMCIA)  
Power Management (Windows 95) control main.cpl power  
Power Management (Windows 98) control powercfg.cpl  
Printers Folder control printers  
Regional Settings control intl.cpl  
Scanners and Cameras control sticpl.cpl  
Sound Properties control mmsys.cpl sounds  
System Properties control sysdm.cpl


Windows substitutes the name of the tool you want to run for %1%. For example:

"rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl".
To run the Users tool in Control Panel, type control Ncpa.cpl users, and then press ENTER.

To run the Users tool for Windows 95/98/Me, type "control inetcpl.cpl users" (without the quotation marks) and then press ENTER.
