# Tenda Router AC Series Vulnerability

This vulnerability lies in the `/goform/setWAN` page which influences the lastest version of Tenda Router AC11. ([AC11_V02.03.01.104_CN](https://www.tenda.com.cn/download/detail-3163.html))

## Vulnerability description

![3](../7/3.png)

There is a stack buffer overflow vulnerability in the `PPPoE` module.


![1](1.png)

the program reads user input `wanPPPoEPwd` into variable `v17` and uses `nvram_set` function to set the nvram variable `wan0_pppoe_passwd`, without porper length check. 

![2](2.png)

the prograrm will then use `nvram_get` function to put that input into variable `v66` and copy to the fixed destination, which will cause a stack overflow.

So by POSTing the page `/goform/setWAN` with proper `wanPPPoEPwd`, the attacker can easily perform a **Deny of Service Attack** or **Remote Code Execution** with carefully crafted overflow data.

## POC

![poc](poc.png)

## Timeline

- 2022.01.09 report to CVE & CNVD