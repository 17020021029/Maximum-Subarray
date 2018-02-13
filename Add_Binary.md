# 题目描述

Given two binary strings, return their sum (also a binary string). 

求两个二进制字符串相加，结果返回字符串

## 示例

a = "11"
b = "1"
Return "100".
### 解题思路

·动态申请一个数组，用来存储相加后的结果：计算a和b的最大长度，应此长度加1再加1，申请数组长度
·注意需要string转int和int转string；
·动态数组申请最大长度为len+1，再加一是字符串最后的'\0'

#### 代码

```
char *addBinary(char *a, char *b) {
    int i,j, temp,temp1,lenA,lenB,len;
    char *str;
    lenA = strlen(a);
    lenB = strlen(b);
    len = lenA>lenB?lenA:lenB;   //比较a和b的长度
    str= (char *)malloc((len+2)*sizeof(char));
    //申请的空间要大于最大的字符串长度加1再+1，第一个1指字符串相加后可能进位，第二个1指字符串最后的'\0'结束字符
    memset(str,0,(len+2)*sizeof(char));   
    //把str所指区域前(len+2)*sizeof(char)个字节设置成字符0(memset函数:example linkhttps://baike.baidu.com/item/memset/4747579?fr=aladdin)
    j = len-1;temp  = 0;
    for(i=len;i >= 0 && lenA > 0 && lenB > 0; i--){   
    //从两字符串最后以为开始循环，每层第一行语句是记录保留在return里的结果，第二行语句相当于进位，再计算下一位的情况，直到其中其中一个加完
        *(str+i) = ((*(a+lenA-1)-'0') + (*(b+lenB-1)-'0') + temp)%2 + '0';   
        //*(a+len-1)-'0'，将字符型变成整型，最后+'0'又将整型变成字符型，下同
        temp = ((*(a+lenA-1)-'0') + (*(b+lenB-1)-'0') + temp)/2;
        lenA--;lenB--;
    }
    if(lenA == 0){//则对b字符串进行赋值给str
        for(; lenB > 0;i--){
            *(str+i) = ((*(b+lenB-1)-'0') +temp)%2 + '0';
            temp = ((*(b+lenB-1)-'0') +temp)/2;
            lenB--;
        }
    }else if(lenB == 0){//对a字符串进行赋值给str
        for(; lenA >0;i--){
            *(str+i) = ((*(a+lenA-1)-'0') + temp)%2+'0';
            temp = ((*(a+lenA-1)-'0') + temp)/2;
            lenA--;
        }
    }
    if(temp == 1) 
    {*(str+i) = temp+'0';   //*(str+i)是最高位,i此时等于0
    return str+i;}   //若temp进位为1，则赋值给str
    return str+i+1;  //str+i+1是从第二位开始的地址
}
```
