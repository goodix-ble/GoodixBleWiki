## Memory��������



### 1. Keil��ô���Ʊ���̼���С��

- �����޸Ĺ̼������������С����GR551x����_flash_scatter_config.h_�ļ��޸�`FLASH_SIZE`�Ĵ�С���Ӷ����ƹ̼��Ĵ�С�����̼����ڴ�ֵʱ��ARMCC�������ᱨ���´���
    ```
    .\Objects\ble_app_cts.axf: Error: L6220E: Load region LR_FLASH size (84604 bytes) exceeds limit (4096 bytes). Region contains 263 bytes of padding and 910 bytes of veneers (total 1173 bytes of linker generated content).
    Finished: 0 information, 0 warning and 1 error messages.
    ".\Objects\ble_app_cts.axf" - 1 Error(s), 0 Warning(s).
    ```



### 2. ��ô��һ�������̶���ָ����ַ��

- �ɲο�SDK��ô�ѱ���```BUILD_IN_APP_INFO```�̶���ָ���ĵ�ַ��
- �����ڹ̶���ַ��ʱ�򣬲�Ҫ��SDK�ڲ��ĵ�ַ�ռ��ͻ��SDK Flash��SRAM�����������ӦоƬ������ָ�ϣ���GR551x��Flash��SRAM���ֿɲο���[GR551x������ָ��](https://docs.goodix.com/zh/online/gr551x_develop_guide/V2.7)����Flash�洢ӳ�䡱�͡�RAM�洢ӳ�䡱�½ڡ�

