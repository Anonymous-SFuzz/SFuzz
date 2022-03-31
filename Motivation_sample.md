
Now, we unwind the function **find_dst_space()** that aims to get a globally set value as an index referencing variables in the current stack.

```
void devDiscoverHandle(int sockfd){
    int len, ret;
    struct sockaddr_in src_addr;
    int addrlen = sizeof(struct sockaddr_in);
    memset((uint8 *)&src_addr , 0, 0x10);
    memset(Global_addr , 0, 0x5C0);
    len = recvfrom(sockfd , Global_addr+0x1c, 0x5a4 , 0, (struct
    sockaddr *)&src_addr , (socklen_t *)&addrlen);
    if( len != ERROR )
        ret = protocol_handler((packet *)(Global_addr+0x1c));
    if (ret == ERROR)
        logOutput("devDiscoverHandle Error!");
}
int protocol_handler(packet *data){
    bytes[4] = {0xe1, 0x2b, 0x83, 0xc7};
    if(header_check(data))
        if(magic_check(data ->magic_bytes , bytes , 4))
            if(checksum(data))
                return msg_handler(data);
    return ERROR;
}
int msg_handler(packet *data){
    int ret = ERROR;
    if(data ->version == 0x01)
        ret=parse_advertisement(data->payload,data->payloadLen);
    return ret;
}
int parse_advertisement(uint8 *payload , int payloadLen) {
    char* dst;                                                                                           Unwind find_dst_space() here
    char* var_addr;                                                                                          |
    char buffer[64]; <---------------------------------------------------------------------------------------|          
    int index; <---------------------------------------------------------------------------------------------|
    var_addr = DAT_404d33a8;<--------------------------------------------------------------------------------|
    msg_element *element;                                                                                    |
    msg_element_header *element_header;                                                                      |
    element = parse_msg_element(payload, payloadLen);                                                        |
    element_header = element->header;                                                                        |
    if (element_header) {                                                                                    | 
        index = (int)*(var_addr+4)); //   <------------------------------------------------------------------|
        dst = buffer+index; <--------------------------------------------------------------------------------| 
        if (copy_msg_element((char *)element ->data , dst, <-------------------------------------------------|  
            element_header->len)) == 0) //Stack Overflow !!!
        return SUCCESS;
    }
    return ERROR;
}
```
