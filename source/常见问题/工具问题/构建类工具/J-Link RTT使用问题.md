## J-Link RTTʹ������


### 1. �����ʾ�������п���RTT����

- ȷ���ļ�${SDK}\external\segger_rtt\SEGGER_RTT.c���빤�̱��롣

- �����������ļ�_custom_config.h_�ĺ�APP_LOG_PORTֵ�޸�Ϊ1��

  ```c
  // <o> APP log port type
  // <0=> UART
  // <1=> RTT
  // <2=> ITM
  #ifndef APP_LOG_PORT
  #define APP_LOG_PORT            1
  #endif
  ```

- ȷ��λ���ļ�${SDK}\platform\boards\board_SK.c�ĺ���bsp_log_init��main�����ʼ��ʱ�򱻵��ã���ȷ��ʾ�����̵�board_init���������ã��˺��������bsp_log_init������ΪSDK��RTT���ܵĳ�ʼ�����ں���bsp_log_init�н��С�



### 2. ��ȡ_SEGGER_RTT�����ַ�ļ��� 

- ��ʹ��J-Link RTTʱ������д_SEGGER_RTT�ĵ�ַ��һ������д��Χ���߽����Զ�������һ������д�̶��ĵ�ַ�����Ľ��ܵڶ��ַ�����

  - �Ȳο����Ŀ���RTT���ܣ�Ȼ����빤�̡�

  - ������ɺ󣬴�.map�ļ�����_SEGGER_RTT�ı�����ַ���ο����£�

    ```c
    _SEGGER_RTT      0x2000c220   Data     120  segger_rtt.o(.bss)
    ```

  - �޸�_SEGGER_RTT.c_�б����Ķ��壬��map�ļ��ı�����ַʹ��attribute at���ԣ�ǿ�ƹ̶���������������_SEGGER_RTT������ַ�Ͳ����湤�̹��ܺͱ��������Ӷ������仯�ˡ�

    ```c
    SEGGER_RTT_CB _SEGGER_RTT __attribute__ ((at(0x2000c220))); 
    ```



### 3. J-Link RTT��ӡ��־ʱ������һ�����־���ٴ�ӡ

- ����Ƿ�����Sleepģʽ�£�RTT���ڷ�AONģ�飬��Sleepģʽ֧�ֲ��Ѻã�����оƬ��Activeģʽʹ��RTT��

- �����Activeģʽ�´���RTT��־ֹͣ��ӡ�����Խ�RTT����ģʽ�޸�ΪSEGGER_RTT_MODE_BLOCK_IF_FIFO_FULL����ģʽ�����£�����������������Ե���RTT��������С��

  ```
  #define SEGGER_RTT_MODE_DEFAULT                   SEGGER_RTT_MODE_BLOCK_IF_FIFO_FULL 
  ```

  