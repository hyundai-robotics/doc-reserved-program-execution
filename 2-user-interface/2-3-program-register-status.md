# 2.3 External Check of Program Schedule Status

- External Selection  
Outputs the "Program Echo Bit" signal synchronized with the "Program ACK" signal for externally input reserved program numbers. Assign the signal under `System > Control Parameters > I/O Signal Settings > Output Signal Assignment`.
 ![](../_assets/image6.png)

    - Outputs the selected program number to the signal assigned to the "Program Echo Bit"
    - Outputs the signal assigned to "Program ACK" for 200ms

  

- Internal Setting  
In `System > Control Parameters > Program Scheduled Execution`, a blinking signal is output to the output signal assigned to the reserved program.
    - For the method of checking output signals, refer to the description of "Output signals" in section 2.1.
