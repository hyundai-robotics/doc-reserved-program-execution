# ${cont_model} Robot Controller Functional Manual - Program Scheduled Execution

{% hint style="warning" %}
The information provided in this product manual is the property of HD Hyundai Robotics.

Without HD Hyundai Robotics' written consent, no part of this manual may be reproduced, redistributed, provided to a third party, or used for other purposes.



This manual is subject to change without prior notice.



**Copyright ⓒ 2023 by HD Hyundai Robotics**
{% endhint %}
# 1. Overview

The Program Scheduled Execution function allows you to schedule programs that will be executed by external input signals and run the scheduled programs in order.

Hyundai robot controllers support two methods of Program Scheduled Execution. Please understand the characteristics of each method and use them according to your desired operating environment.
# 1.1 External Selection Method

When different workpieces enter consecutively along a conveyor, you can select the program for each workpiece via an external program selection method and register it in the Program Scheduled Execution Register so the scheduled programs are executed in order.

![](../_assets/image1.png)

The figure above shows that while program A is running, programs B and C have been registered in the Program Scheduled Execution Register in order, and program D has not yet been registered.
# 1.2 Internal Setting Method

If a program has been assigned for each work target, pressing the button on the workbench registers the corresponding program number in the Program Scheduled Execution Register, and programs are executed in that order when started.


![](../_assets/image2.png)

If different workpieces are placed on three workstations as shown above, pressing the buttons on workstations 1, 2, and 3 registers programs 1, 2, and 3 respectively in the Program Scheduled Execution Register.

When the operator prepares workpiece 1 and presses the workstation input button and then the start button on the operation panel, program 1 begins execution. While program 1 is running, if the operator prepares workpiece 2 and presses the workstation input button, program 2 waits in the Program Scheduled Register. After program 1 finishes, program 2 that was waiting in the Program Scheduled Register executes. If the input button for workstation 2 is pressed after program 1 has already completed, the start button must be pressed again.
# 2. System Settings
# 2.1 Program Scheduled Execution Preferences
The environment for the Program Scheduled Execution feature is configured under [**System > Control Parameters > Program Scheduled Execution**].

![](../_assets/image3.png)

## Applied register count  
- Disabled  
The Program Scheduled Execution feature is not used.

- 20  
Prepare 20 registers for the Program Scheduled Execution. If you try to schedule more than 20 programs, an E1047 error will occur.

- 1  
Prepare 1 register for Program Scheduled Execution. Only one program can be scheduled. Therefore, it cannot be used in production lines where more than one different workpiece enters consecutively and pre-scheduling is required.  

  

## Program input method  
- External Selection  
Registers the program assigned as the “Program Select Bit” in [**System > Control Parameters > I/O Signal Settings > Input Signal Assignment**] into the Program Scheduled Execution Register.

    ![](../_assets/image4.png)

    - Assign the program Strobe input signal in the figure above.
    - Assign a Binary/Discrete (OFF->Binary) input signal to select whether to receive externally input program selection signals as Binary or Discrete, and use according to the receiving method.
    - The reception method reads the Program Select Bit when the program Strobe signal changes from Low to High and generates the desired program number according to Binary/Discrete(OFF->Binary). The Program Select Bit input signal must be activated at least 200ms before the program Strobe input signal.
    - When using the External Selection mode, set “Use Program Strobe Signal” to “Enabled” under [**System > User Preferences**].

- Internal Setting  
The user preassigns input signals and their corresponding program numbers, and when the input signal turns ON, the specified program number is registered in the register.
    - Provides up to 7 different work devices.
    - The “Handle duplicate program input” menu is only used when in Internal Setting mode.
    - The workstation's input signals can also be used as external start and stop buttons (see the ["Input Signals"](#input-signals) section).
  

## Handling duplicate program inputs  
When the Program input method is set to “Internal Setting” and a program number identical to one already in the Program Scheduled Execution Register is input, this setting determines how to handle the duplicate registration.

- Delete  
Deletes the registered program number in the register. It searches for the identical program number in the register and deletes it. A notice “Reserved program (No:xxx) will be deleted” is displayed on the screen.

- Prohibit  
Does not register if an identical program is already reserved in the register. A notice “Program (No:xxx) reservation is prohibited” is displayed on the screen.

- Allow  
Registers if the register is empty.


## Input signals  
- Configure the signal for each port of the input connectors mounted on the I/O board.

- When external start and external stop signals are not assigned in Input Signal Assignment, pressing the workstation input button can be used as external start or external stop commands.
    -   External start : execute the external start simultaneously with registering a program number.
    -   External stop : executes external stop when scheduling the currently running program.

  

## Output signals  
- Configure the signal for each port of the output connectors mounted on the I/O board.
- When a lamp is connected to the output signal, the lamp changes as follows from the time the program number is scheduled in the register until it is executed.
    - When the program number is not scheduled → Lamp off
    - When the program number is scheduled → Lamp blinking
    - When the scheduled program number is running → Lamp on
    - When the scheduled program number has completed → Lamp off

  

## Program  
Set the operation program for the corresponding workstation.
If a program number is not assigned, a notice “Reserved program number is not assigned” is displayed on the screen.
# 2.2 Program Scheduled Execution Register

The Program Scheduled Execution Register allows you to confirm, change, insert, or delete scheduled programs. This can be used when the Applied register count in Program Scheduled Execution settings is set to '20' or '1'. Select [**Program Schedule**] from the panel selection window.

![](../_assets/image5.png)

- Edit  
To change the scheduled program at the current position, click the "Edit" button and enter the desired program number.

- Insert  
Click the "Insert" button and enter the desired reserved program number to reserve a new program after the current position.

- Delete  
Position on the reserved program number you want to delete and click the "Delete" button to remove the program number from the register.

  

[**Note**]
- Cannot be executed in Remote Mode.
# 2.3 External Check of Program Schedule Status

- External Selection  
Outputs the “Program Echo Bit” signal synchronized with the “Program ACK” signal for externally input reserved program numbers. Assign the signal under [**System > Control Parameters > I/O Signal Settings > Output Signal Assignment**].
 ![](../_assets/image6.png)

    - Outputs the selected program number to the signal assigned to the “Program Echo Bit”
    - Outputs the signal assigned to “Program ACK” for 200ms

  

- Internal Setting  
In [**System > Control Parameters > Program Scheduled Execution**], a blinking signal is output to the output signal assigned to the reserved program.
    - For the method of checking output signals, refer to the description of “Output signals” in section 2.1.
# 3. Playback
- Execute from step 0  
When the start button is pressed, the robot controller executes the first program in the Program Scheduled Execution Register. After executing until the program END, it executes the next program registered in the Program Scheduled Execution Register.

- Execute from a mid step  
When the start button is pressed, execution begins from the selected statement of the selected program. After completing the execution of this program, it executes the programs registered in the Program Scheduled Execution Register.

- Execute when no scheduled program exists  
If no programs are registered in the register, when pressing the start button, it will wait until a program is registered in the Program Scheduled Execution Register as shown in the figure below. When a program is registered, it executes it immediately.
 ![](../_assets/image7.png)
# 4. Errors / Warnings
- Errors  
    |Category|Details|
    |-|-|
    |Message|E1047 The register exceeded 20|
    |Cause|Attempting to reserve more than the configured number of 20 under Settings/Control Parameters/Program Scheduled Execution.|
    |Action|Check the reservation status in Service/Monitoring/Program Scheduled Execution.|  

  

- Warnings
    |Category|Details|
    |-|-|
    |Message|W28302 Program reservation registration is available only in Remote Mode.|
    |Cause|The program reservation registration mode is not Remote Mode.|
    |Action|Check the operation mode.|  
