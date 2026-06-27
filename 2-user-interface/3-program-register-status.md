# 2.3 程序时程状态的外部检查

- 外部选择  
输出与“程序确认信号”同步的“程序回声位”信号，用于外部输入的保留程序编号。将信号分配至`System > Control Parameters > I/O Signal Settings > Output Signal Assignment`下。

 ![](../_assets/image6.png)

    - 将所选程序编号输出到分配给“程序回声位”的信号
    - 将分配给“程序确认信号”的信号输出200毫秒

  

- 内部设置  
在`System > Control Parameters > Program Scheduled Execution`中，向分配给保留程序的输出信号输出闪烁信号。
    - 有关输出信号检查的方法，请参见第2.1节“输出信号”的描述。