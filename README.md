# ProtocalParser


## Purpose
You can use this lib to unpack/pack communicate frame from/to data stream, only two function for you
'NOTE:' All code is written by c, and FSM(Finite State Machine) technology, all function is non-block, it's important for non-os embedded environment


 Frame Struct is list below
 
     Byte  :   2       :   1   :   1   :     N     :   1   :
     Name  : Header    :  Type :  Len  :  Payload  :  Xor  :
     Value : 0xAB 0x55 :
 

MIT license, please feel be free to use this code, if occur bugs, let's me know

Q   Q:819280802<br>
Email:819280802@qq.com

## APIs
    extern uint8_t* FrameUnpack(uint8_t Data); 
    extern uint8_t* FramePack(uint8_t* buf, uint8_t type, uint8_t* pdata, uint8_t len); 
    
## Example

    #include "FrameParser.h"
    #include <stdio.h>
    
    static uint8_t buf[] =
    {
        0x00, 0x01, 0xAB,
        0xAB,
    
        0xAB, 0x55,
        0x01,
        0x03,
        0x00, 0x01, 0x02,
        0xFF,
    
        0xAB, 0x55,
        0x01,
        0x03,
        0x00, 0x01, 0x02,
        0xFF,
    };
    
    int main()
    {
        uint8_t *result = NULL;
        uint8_t packet_cnt = 0;
    
        printf("System Start\r\n");
    
        printf("\r\nTest 1: Unpack Packet\r\n");
        for(uint8_t i = 0; i < sizeof(buf); i++)
        {
            result = FrameUnpack(buf[i]);
            if(NULL != result)
            {
                printf("Find Packet\r\n");
                packet_cnt++;
            }
        }
    
        if(!packet_cnt)
        {
            printf("Can not find packet\r\n");
        }
        else
        {
            printf("Find %d Packets\r\n", packet_cnt);
        }
    
        printf("\r\nTest 2: Pack Packet with content\r\n");
        // Regenerate Again
        memset(buf, 0, sizeof(buf));
        result = FramePack(buf, 0x03, "012", 3);
        packet_cnt = 0;
    
        for(uint8_t i = 0; i < sizeof(buf); i++)
        {
            result = FrameUnpack(buf[i]);
            if(NULL != result)
            {
                printf("Find Packet\r\n");
                packet_cnt++;
            }
        }
    
        if(!packet_cnt)
        {
            printf("Can not find packet\r\n");
        }
        else
        {
            printf("Find %d Packets\r\n", packet_cnt);
        }
    
    
        printf("\r\nTest 3: Pack Packet without content\r\n");
        // Regenerate Again
        memset(buf, 0, sizeof(buf));
        result = FramePack(buf, 0x03, NULL, 0);
        packet_cnt = 0;
    
        for(uint8_t i = 0; i < sizeof(buf); i++)
        {
            result = FrameUnpack(buf[i]);
            if(NULL != result)
            {
                printf("Find Packet\r\n");
                packet_cnt++;
            }
        }
    
        if(!packet_cnt)
        {
            printf("Can not find packet\r\n");
        }
        else
        {
            printf("Find %d Packets\r\n", packet_cnt);
        }
    
        while(1);
        return 0;
    }
