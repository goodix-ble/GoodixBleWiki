## APP LOGģ�鳣������



### 1. UART��LOG��߲������ܵ����٣�

-   GR5xxϵ��SoC��UART������ʾ�Ϊ4M�������ǵ��������Լ�����תUSBоƬ���������ǳ����Ƽ�ʹ��4M����LOG�����⣬GR5xx Starter Kit���ص�J-Link OBоƬ���֧�ֵĲ�����ֻ��460800����������Բ������и��ߵ�Ҫ����Ҫ�û�������ܸ��õĴ���תUSB�塣



### 2. ʹ�ô��ڴ�LOG�ڵ����Ͽ����������ô�죿

���Ը������²�����һ�����Ų飺

1. ��鴮�ڹ��ߵ����ã�����**������**��**��żУ��**��**����λ**��**ֹͣλ**��**����**�����ã�������г�ʼ��UARTʱ�����ñ���һ�¡�
2. ���_custom_config.h_�����á�`APP_LOG_ENABLE`��Ӧ��Ϊ`1`��`APP_LOG_PORT`��Ӧ��Ϊ`0`��
3. ���APP LOGģ����UART�Ƿ���ȷ��ʼ�����ڵ���`app_log_init()`����ʱ���ڶ�������`trans_func`Ӧ����ʹ�ö�ӦUART��������ĺ���������д���ɲο�SDK��ʵ��`platform\boards\board_SK.c`��[GR5xx APP LogӦ��˵��](https://docs.goodix.com/zh/online/app_log_bl/V3.2)����ģ���ʼ������ȡ��½ڡ�
4. ���UART��ʼ��ʱPin Mux�Ƿ���ȷ���á������Pin Muxֵ��ο���ʹ��SoC��Datasheet��
5. �����LOGʱʹ����DMA��ʽ����Ҫ���UART DMA�������Ƿ���ȷ���Լ�ʹ�õ�UART�Ƿ�֧��DMA������GR551x SoC��UART1�ǲ�֧��DMA�ģ���
6. �����������5���Բ��ܽ�����⣬����ʹ���߼������ǻ�ʾ������UART TX/RX����ץȡ���Σ����ж�GR5xx SoC�Ƿ��������ȷ��UART�źš�����У�����Ҫ��һ���Ų鴮��תUSB�򴮿ڹ��ߵ����⡣���û�У����Ƽ�����SDK�е�Example���̽��жԱȲ��ԣ���ȷ����Ӳ�����⻹��������⣬Ȼ����Ծ���������з����Ų顣



### 3. ��ô��SEGGER RTT��LOG��

SDK�ṩ������ģ�幤�̾�֧��ͨ��SEGGER RTT��LOG���ڹ��̶�Ӧ��_custom_config.h_���ҵ�`APP_LOG_PORT`�꣬��ֵ�޸�Ϊ`1`��

```c
// <o> APP log port type
// <0=> UART
// <1=> RTT
// <2=> ITM
#ifndef APP_LOG_PORT
#define APP_LOG_PORT            1
#endif
```

Ȼ�����±������ؼ��ɡ���J-Link RTT Viewer�������豸ʱ��**Connection to J-Link**һ��һ��ѡ��**USB**��**Specify Target Device**һ����д**Cortex-M4**��**Target Interface & Speed**ѡ��**SWD**��**4000kHz**��**RTT Control Block**����������д��������һ�����ڱ��������`.map`��Ѱ��`_SEGGER_RTT`���ŵĵ�ַ��Ȼ����RTT Viewer��ʹ��**Address**ѡ����д���ڶ��ַ�ʽ����д��ַ��Χ��RTT Viewer�Զ�����������ͨ����д������SRAM��Χ���ɡ���SoC�ĵ�ַ��Χ��ο��±�

SoC | Search Range
-- | ---
GR5515 | 0x30000000 0x3003FFFF
GR5513 | 0x30000000 0x3001FFFF
GR5525 | 0x20000000 0x2003FFFF
GR5526 | 0x20000000 0x2007FFFF
GR533x | 0x20000000 9x20017FFF

������Լ��Ĺ��̣���ο�SDK��ʵ�ֽ�����ֲ����ο���[GR5xx APP LogӦ��˵��](https://docs.goodix.com/zh/online/app_log_bl/V3.2)����ģ���ʼ������ȡ��½ڡ�



### 4. ΪʲôRTT��ӡ��־һ��ʱ���Ͳ��ٴ�ӡ�ˣ�ϵͳҲ�����ˣ�

1.  ���ٴ�ӡ��־��ԭ����RTT��ʽ��ӡ��־����J-Link���ӣ���GR5xx SoC����˯��ģʽ֮��J-Link���ӻ�Ͽ���J-Link RTT Viewer���߱��Զ��������ܣ���ϵͳ�ٴλ���ʱ��J-Link�����Ѿ��Ͽ��ˣ����Ա���Ϊ���ٴ�ӡ����LOG��
2.  ϵͳ������ԭ������п�����RTT��ӡģʽΪ����ģʽ��������FIFO����֮��RTT�ͻ������ȴ���λ����FIFO���ա���ϵ�1����������ԭ��˯�߻���֮��J-Link�����Ѿ��Ͽ�����λ��û���ټ�����ȡFIFO����FIFO����֮���ٴ�LOG�������ȴ�����Ϊ�ͻ����Ϊ������

����취��2�֣�

1.  �رյ͹���ģʽ����ϵͳ��ʼ��ʱ����`pwr_mgmt_mode_set(PMR_MGMT_ACTIVE_MODE);`����SoC������Active״̬���Ա���J-Link���Ӷϵ���ͬʱ��ܡ���LOG���롰ϵͳ�����������⣬ȱ�����޷���Ե͹��Ĳ��ֽ�����Ч���ԡ�
2.  ��RTT���ģʽ����Ϊ������ģʽ���ڳ�ʼ��RTTʱʹ��`SEGGER_RTT_ConfigUpBuffer()`�������޸����ģʽ��������ģʽ�����֣�`SEGGER_RTT_MODE_NO_BLOCK_SKIP`��`SEGGER_RTT_MODE_NO_BLOCK_TRIM`��������FIFOʣ��ռ䲻���Է��������Ĵ���ӡ��־���������ΪSKIP���������ôδ�ӡ���������ΪTRIM����ֻ��FIFO�����������Ĳ���ȫ�����������ַ�����Ȼ��Ӱ��͹��ģ�ֻ�ܱ���J-Link���ӶϿ��󲻻����������������ܱ��ⶪLOG�������



### 5.  J-Link RTT Viewer����֮�󿴲���LOG��ô�죿

���Ը������²�����һ�����Ų飺

1. ���_custom_config.h_�����á�`APP_LOG_ENABLE`��Ӧ��Ϊ`1`��`APP_LOG_PORT`��Ӧ��Ϊ`1`��

2. ���APP LOGģ����RTT�Ƿ���ȷ��ʼ�����ڵ���`app_log_init()`����ʱ���ڶ�������`trans_func`Ӧ����ʹ��`SEGGER_RTT_Write()`��������ĺ�������������`SEGGER_RTT_Write()`������Ϊ��β�һ����������д���ɲο�SDK��ʵ��`platform\boards\board_SK.c`��[GR5xx APP LogӦ��˵��](https://docs.goodix.com/zh/online/app_log_bl/V3.2)����ģ���ʼ������ȡ��½ڡ�

3. ���RTT�еĸ������á����ȼ��`_SEGGER_RTT`�����Ƿ���ȷ���á�ͨ������²��Ƽ���`_SEGGER_RTT`�������õ��̶��ĵ�ַ��������û�ѡ���˽���������ŵ��̶���ַ������Ҫȷ������̶���ַ������SRAM��ַ��Χ���ڡ�

4. ���J-Link RTT Viewer����ʱ`RTT Control Block`�ĵ�ַ�����Ƿ���ȷ��GR5xx SoC�ݲ�֧��**Auto Detection**ģʽ����Ҫʹ��**Address**ģʽ�ֶ����룬����ʹ��**Search Range**ģʽ�������ַ��Χ��RTT Viewer�Զ�ȥ��������������÷�����ο�����ô��SEGGER RTT��LOG�����½ڵĻش�

5. �����������4����Ȼ�޷���λ��������⣬����Ҫͨ���������Ը�����ȷ��ÿһ����LOG��ÿһ�������Ƿ���ȷִ�У�����뷵��ֵ�Ƿ����Ԥ�ڵȵȡ�



### 6. �洢��LOG��ô��������

-   ʹ��Android�˵�GRToolbox App���������������ο���[GR5xx APP LogӦ��˵��](https://docs.goodix.com/zh/online/app_log_bl/V3.2)������ȡ��־���½ڡ�



### 7. ʹ��GRToolbox�����洢LOGʱ��LOG��������ô�죿

-   ���п���������������ʱ�洢LOG��RingBuffer������µġ�Ϊ�˾����ܽ��Ͳ�дFlash��Ӧ��ʱ���Ӱ�죬��LOGʱ��������������Ĵ洢���������ǽ�LOG���ݷ��뻺��RingBuffer�У�����`app_log_store_schedule()`����������д��Flash�������`app_log_store_schedule()`ִ��֮ǰ������˳���RingBuffer��С��LOG���ͻ����LOG������������LOG���������Դ����������ֽ���취��
    1. ���󻺴�RingBuffer����������޸�`components\libraries\app_log\app_log_store.h`��`APP_LOG_STORE_LINE_SIZE`��`APP_LOG_STORE_CACHE_NUM`��ֵ�������󻺴�RingBuffer����ͬʱҲ������RAMռ�á�
    2. ���ӵ���`app_log_store_schedule()`��Ƶ�Ρ�������ʹ��RTOS�Ļ����£�����������������`app_log_store_schedule()`����������ȼ������ø�����õ�����εĵ��ȣ�������Ƶ�εز���Flash���ܻ��ĳЩʱ�����е�Ӧ�����Ӱ�졣



### 8. ʹ��GRToolbox������־ʱ������ֻ���������־���޷���ȡ�������־��ô�죿

-   LOG�洢ʹ���˻��θ��ǻ��ƣ����洢�ռ䲻��ʱ�������µ�LOGȥ�������ϵ�LOG��������������޷������ؽ����ֻ��ͨ������洢���ͼ��ٲ���Ҫ��LOG�������������⡣

