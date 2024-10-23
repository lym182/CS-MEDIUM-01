# CS-MEDIUM-01
## 学习心得与路径
- 心得：这半个月被狠狠洗礼，还是之前高估自己了，以为能很快解决，国庆特种兵旅游七天归来后才开始看题（甚至还有闲心去练舞我的天。。）仔细一看要学的超级超级多，而开始熬夜补，最忙的一周平均每天两点半睡七点起，学到崩溃，，，没想到停了其他的事，一堆意外事件接踵而至，一次次耗费我大量的时间，一点开群发现交题截至时间提前（天塌了...）反反复复写，结果怎么都不对，最后两天发现两个大的题目题目全看错，又紧急改，，，改到现在才终于能交了。这半个月真是太让人难崩了，有种磨练赶ddl能力的感觉，同时也让我体会到在时间紧的情况下该怎么以平静的心态去处理种种突发情况，也是体会到了高三都没有的待遇--周末去图书馆自习，有一种奇妙的充实感，真好，又学会了好多东西。
- 路径：B站，问朋友
## 问题回答
1.因为 int和long long类型无法处理大数，我们只能将大数视为一串字符来计算。
2.用数组对上一位进位或者借位。
3.读取'('  ')'和'-'，将其转为正数运算再考虑'-'的添加。

- 运行结果(见文件)
- 代码（所有功能）
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define MAX 100

void Addition(char num1[], char num2[]);
void Subtraction(char num1[], char num2[]);
void Multiplication(char num1[],char num2[]);
void Division(char num1[], char num2[]);
int  SubStract(int p1[], int len1, int p2[], int len2);
void Subtraction_add1(char num1[], char num2[]);
void Subtraction_add2(char num1[], char num2[]);
void Addition_sub(char num1[], char num2[]);

int main (void){
    char num1[MAX];
    char num2[MAX];
    char op ;
    puts("请输入想计算的内容：");
    scanf("%s %c %s",num1,&op,num2);
    switch (op)
    {
    case '+':
        Addition(num1, num2);
        break;
    
    case '-':
        Subtraction(num1, num2);
        break;

    case '*':
        Multiplication(num1, num2);
        break;

    case '/':
        Division(num1, num2);
        break;

    default:
        break;
    }
    system("pause");
    return 0;
}
void Addition_sub(char num1[], char num2[]) {
    int sum[MAX] = {0};
    int i, j, len, len3, len4;
    int n2[MAX] = {0};
    int blag = 0;
    //判断符号
    if(num1[0] == '(' && num2[0] != '('){
        blag = 1;
    }
    
    int len1 = strlen (num1); // 计算数组num1的长度，即大数的位数 
    int len2 = strlen (num2); // 计算数组num2的长度，即大数的位数     
    
    //将num1字符数组的数字字符转换为整型数字，且逆向保存在整型数组sum中，即低位在前，高位在后
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            sum[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
         
     // 转换第二个数 
     for (i = len2-1, j = 0; i >= 0; i--)
        {if(num2[i] >= '0' && num2[i] <= '9'){
            n2[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
       
    len = len3>len4 ? len3 : len4; // 获取较大的位数
     
     // 将两个大数相加 
    for (i = 0; i <= len; i++)
    {
        sum[i] += n2[i];  // 两个数从低位开始相加 
        if (sum[i] > 9)   // 判断是否有进位 
        {   // 进位 
            sum[i] -= 10;
            sum[i+1]++;
        }
    }
    if(sum[len] != 0) { // 判断最高位是否有进位 
        len++;
    }
    printf("%s - %s = ", num1, num2);

    if(blag == 1){
        printf("-");
    }
     
    for (i = len-1; i >= 0; i--)
        printf("%d", sum[i]);
     
    printf("\n");
     
}
void Subtraction_add1(char num1[], char num2[])
{
    int i, j, len, blag;
    int sum[MAX] = {0};
    int temp[MAX] = {0};
    int n2[MAX] = {0};
    int len1 = strlen(num1); // 计算数组num1的长度，即大数的位数
    int len2 = strlen(num2); // 计算数组num2的长度，即大数的位数
    int len3, len4;
    // 将num1字符数组的数字字符转换为整型数字，且逆向保存在整型数组sum中，即低位在前，高位在后
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            sum[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
     // 转换第二个数 
    for (i = len2-1, j = 0; i >= 0; i--)
        {if(num2[i] >= '0' && num2[i] <= '9'){
            n2[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
     
    // 在进行减法之前要进行一些预处理 
    blag = 0; // 为0表示结果是正整数，为1表示结果是负整数 
    if(len3 < len4) // 如果被减数位数小于减数
    {
        blag = 1; // 标记结果为负数
        // 交换两个数，便于计算 
        for (int i = 0; i < len3; i++)
        {
            temp[i] = sum[i];
        }
        for (int i = 0; i < len4; i++)
        {
            sum[i] = n2[i];
        }
        for (int i = 0; i < len4; i++)
        {
            n2[i] = temp[i];
        }
        len = len3;
        len3 = len4;
        len4 = len;
    }
    else if(len3 ==len4) // 如果被减数的位数等于减数的位数
    {  
        // 判断哪个数大 
        for(i = 0; i < len3; i++)
        {
            if(sum[i] == n2[i]) {
                continue;
            }
            if(sum[i] > n2[i])
            {
                blag = 0; // 标记结果为正数 
                break;
            } 
            else
            {
                blag = 1; // 标记结果为负数 
                // 交换两个数，便于计算 
                for (int i = 0; i < len3; i++){
                temp[i] = sum[i];
                }
        for (int i = 0; i < len4; i++)
        {
            sum[i] = n2[i];
        }
        for (int i = 0; i < len4; i++)
        {
            n2[i] = temp[i];
        }
                break;
            } 
        } 
    }
    len = len3 > len4 ? len3 : len4; // 获取较大的位数
     
    // 将两个大数相减 
    for (i = 0; i < len; i++)
    {
        sum[i] = sum[i] - n2[i]; // 两个数从低位开始相减 
        if (sum[i] < 0)   // 判断是否有借位 
        {    // 借位 
            sum[i] += 10;
            sum[i+1]--;
        }
    }
    // 计算结果长度 
    for (i = len-1; i>=0 && sum[i] == 0; i--){
    len = i;
    }
     
    printf("%s + %s  = ", num1, num2);
    if(blag==1)
    {
        printf("-");
    } 
     
    for (i = len - 1; i >= 0; i--) {
    printf("%d", sum[i]);
    }
     
}
void Subtraction_add2(char num1[], char num2[])
{
    int i, j, len, blag;
    int sum[MAX] = {0};
    int temp[MAX] = {0};
    int n2[MAX] = {0};
    int len1 = strlen(num1); // 计算数组num1的长度，即大数的位数
    int len2 = strlen(num2); // 计算数组num2的长度，即大数的位数
    int len3, len4;
    //将num1字符数组的数字字符转换为整型数字，且逆向保存在整型数组sum中，即低位在前，高位在后
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            sum[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
    // 转换第二个数 
    for (i = len2-1, j = 0; i >= 0; i--)
        {if(num2[i] >= '0' && num2[i] <= '9'){
            n2[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
     
    // 在进行减法之前要进行一些预处理 
    blag = 0; // 为0表示结果是正整数，为1表示结果是负整数 
    if(len3 < len4) // 如果被减数位数小于减数
    {
        blag = 1; // 标记结果为负数
        // 交换两个数，便于计算
        for (int i = 0; i < len3; i++)
        {
            temp[i] = sum[i];
        }
        for (int i = 0; i < len4; i++)
        {
            sum[i] = n2[i];
        }
        for (int i = 0; i < len4; i++)
        {
            n2[i] = temp[i];
        }
        len = len3;
        len3 = len4;
        len4 = len;
    }
    else if(len3 ==len4) // 如果被减数的位数等于减数的位数
    {  
        // 判断哪个数大 
        for(i = 0; i < len3; i++)
        {
            if(sum[i] == n2[i]) {
                continue;
            }
            if(sum[i] > n2[i])
            {
                blag = 1; // 标记结果为负数  
                break;
            } 
            else
            {
                blag = 0; // 标记结果为正数 
                // 交换两个数，便于计算 
                for (int i = 0; i < len3; i++){
                    temp[i] = sum[i];
                }
        for (int i = 0; i < len4; i++)
        {
            sum[i] = n2[i];
        }
        for (int i = 0; i < len4; i++)
        {
            n2[i] = temp[i];
        }
                break;
            } 
        } 
    }
    len = len3 > len4 ? len3 : len4; // 获取较大的位数
     
    // 将两个大数相减
    for (i = 0; i < len; i++)
    {
        sum[i] = sum[i] - n2[i]; // 两个数从低位开始相减 
        if (sum[i] < 0)   // 判断是否有借位  
        {    // 借位 
            sum[i] += 10;
            sum[i+1]--;
        }
    }
    // 计算结果长度
    for (i = len-1; i>=0 && sum[i] == 0; i--){
    len = i;
    }
     
    printf("%s + %s  = ", num1, num2);
    if(blag==1)
    {
        printf("-");
    } 
     
    for (i = len - 1; i >= 0; i--) {
    printf("%d", sum[i]);
    }
     
}
void Addition(char num1[], char num2[]) {
    int sum[MAX] = {0};
    int i, j, len, len3, len4;
    int n2[MAX] = {0};
    int blag = 0;
    //判断符号
    if(num1[0] == '(' && num2[0] == '('){
        blag = 1;
    }
    if(num1[0] !='(' && num2[0] == '('){
        Subtraction_add1(num1, num2);
        return;
    }
    if(num2[0] !='(' && num1[0] == '('){
        Subtraction_add2(num1, num2);
        return;
    }
    
    
    int len1 = strlen (num1); // 计算数组num1的长度，即大数的位数 
    int len2 = strlen (num2); // 计算数组num2的长度，即大数的位数     
    
    //将num1字符数组的数字字符转换为整型数字，且逆向保存在整型数组sum中，即低位在前，高位在后
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            sum[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
         
    // 转换第二个数  
    for (i = len2-1, j = 0; i >= 0; i--)
        {if(num2[i] >= '0' && num2[i] <= '9'){
            n2[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
       
    len = len3>len4 ? len3 : len4; // 获取较大的位数
     
    // 将两个大数相加
    for (i = 0; i <= len; i++)
    {
        sum[i] += n2[i];  // 两个数从低位开始相加 
        if (sum[i] > 9)   // 判断是否有进位  
        {   // 进位
            sum[i] -= 10;
            sum[i+1]++;
        }
    }
    if(sum[len] != 0) { // 判断最高位是否有进位 
        len++;
    }
    printf("%s + %s = ", num1, num2);

    if(blag == 1){
        printf("-");
    }
     
    for (i = len-1; i >= 0; i--)
        printf("%d", sum[i]);
     
    printf("\n");
     
}
void Subtraction(char num1[], char num2[])
{
    int i, j, len, blag;
    int sum[MAX] = {0};
    int temp[MAX] = {0};
    int n2[MAX] = {0};
    int len1 = strlen(num1); // 计算数组num1的长度，即大数的位数 
    int len2 = strlen(num2); // 计算数组num2的长度，即大数的位数
    int len3, len4;
     
    
    if(num1[0] !='(' && num2[0] == '('){
        Addition_sub(num1, num2);
        return;
    }
    if(num2[0] !='(' && num1[0] == '('){
        Addition_sub(num1, num2);
        return;
    }
     
    //将num1字符数组的数字字符转换为整型数字，且逆向保存在整型数组sum中，即低位在前，高位在后
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            sum[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
    // 转换第二个数 
    for (i = len2-1, j = 0; i >= 0; i--)
        {
            if(num2[i] >= '0' && num2[i] <= '9'){
            n2[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
     
    // 在进行减法之前要进行一些预处理 
    blag = 0; // 为0表示结果是正整数，为1表示结果是负整数 
    if(len3 < len4) // 如果被减数位数小于减数
    {
        blag = 1; // 标记结果为负数
        // 交换两个数，便于计算
        for (int i = 0; i < len3; i++)
        {
            temp[i] = sum[i];
        }
        for (int i = 0; i < len4; i++)
        {
            sum[i] = n2[i];
        }
        for (int i = 0; i < len4; i++)
        {
            n2[i] = temp[i];
        }
        len = len3;
        len3 = len4;
        len4 = len;
    }
    else if(len3 ==len4) // 如果被减数的位数等于减数的位数
    {  
        //判断哪个数大  
        for(i = 0; i < len3; i++)
        {
            if(sum[i] == n2[i]) {
                continue;
            }
            if(sum[i] > n2[i])
            {
                blag = 0; // 标记结果为正数 
                break;
            } 
            else
            {
                blag = 1; // 标记结果为负数
                // 交换两个数，便于计算 
                for (int i = 0; i < len3; i++){
                    temp[i] = sum[i];
                }
        for (int i = 0; i < len4; i++)
        {
            sum[i] = n2[i];
        }
        for (int i = 0; i < len4; i++)
        {
            n2[i] = temp[i];
        }
                break;
            } 
        } 
    }
    len = len3 > len4 ? len3 : len4; // 获取较大的位数
     
     // 将两个大数相减
    for (i = 0; i <= len; i++)
    {
        sum[i] = sum[i] - n2[i]; // 两个数从低位开始相减 
        if (sum[i] < 0)   // 判断是否有借位 
        {    // 借位 
            sum[i] += 10;
            sum[i+1]--;
        }
    }
    // 计算结果长度
    for (i = len-1; i>=0 && sum[i] == 0; i--){
    len = i;
    }
     
    printf("%s  - %s  = ", num1, num2);
    if(blag==1)
    {
        printf("-");
    } 
     
    for (i = len - 1; i >= 0; i--) {
    printf("%d", sum[i]);
    }
     
}

void Multiplication(char num1[],char num2[])
{
    int i, j, len, len1, len2, len3, len4;
    int a[MAX+10] = {0};
    int b[MAX+10] = {0};
    int c[MAX*2+10] = {0};
     
    int blag = 0;
    if (num1[0] != '(' && num2[0] == '(')
    {
        blag = 1;
    }
    if (num2[0] != '(' && num1[0] == '(')
    {
        blag = 1;
    }
     
     
    len1 = strlen(num1);
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            a[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
     
    len2 = strlen(num2);
    for (i = len2-1, j = 0; i >= 0; i--)
        {if(num2[i] >= '0' && num2[i] <= '9'){
            b[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
     
    for(i = 0; i < len4; i++)//用第二个数乘以第一个数,每次一位 
    {
        for(j = 0; j < len3; j++)
        {
            c[i+j] += b[i] * a[j]; //先乘起来,后面统一进位
        }
    }
     
    for(i=0; i<MAX*2; i++) //循环统一处理进位问题
    {
        if(c[i]>=10)
        {
            c[i+1]+=c[i]/10;
            c[i]%=10;
        }
    }
 
    for(i = MAX*2; c[i]==0 && i>=0; i--); //跳过高位的0
    len = i+1; // 记录结果的长度 
     
    printf("%s * %s  = ", num1, num2);
    if(blag == 1){
        printf("-");
    }
    for(i = len-1; i>=0; i--) {
    printf("%d", c[i]); 
    }
}

void Division(char num1[], char num2[])
{
    int i, j;
    int len1, len2, len3, len4, len=0;     //大数位数
    int dValue;                //两大数相差位数
    int nTemp;                 //Subtract函数返回值
    int num_a[MAX] = {0};      //被除数
    int num_b[MAX] = {0};      //除数
    int num_c[MAX] = {0};      //商
 
    int blag = 0;
    if(num1[0] != '(' && num2[0] == '('){
        blag = 1;
    }
    if(num2[0] != '(' && num1[0] == '('){
        blag = 1;
    }
    
    len1 = strlen(num1);       //获得大数的位数
    len2 = strlen(num2);
     
     //将数字字符转换成整型数并倒存在数组中
    for (i = len1-1, j = 0; i >= 0; i--) {
        if(num1[i] >= '0' && num1[i] <= '9'){
            num_a[j] = num1[i] - '0';
            len3 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
    for (i = len2-1, j = 0; i >= 0; i--) {
        if(num2[i] >= '0' && num2[i] <= '9'){
            num_b[j] = num2[i] - '0';
            len4 = j + 1;
            j++;
        }
        else{
            continue;
        }
    }
 
    if( len3 < len4 )          //如果被除数小于除数，直接返回-1，表示结果为0
    {
        blag = 3;
    }
    dValue = len3 - len4;      //相差位数
    for (i = len3-1; i >= 0; i--)    //让除数和被除数位数相等(前补0)
    {
        if (i >= dValue){
            num_b[i] = num_b[i-dValue];
        }
        else {                       
            num_b[i] = 0;
        }
    }
    len4 = len3;
    for(j = 0; j <= dValue; j++ )    //重复调用，记录减成功的次数，即为商（差N位，不同位减N+1次）
    {
        while((nTemp = SubStract(num_a, len3, num_b+j, len4-j)) >= 0)//第三处传入的是b[j]的地址
        {
            len3 = nTemp;            //结果长度
            num_c[dValue-j]++;       //每成功减一次，将商的相应位加1
        }
    }
    // 计算商的位数，并将商放在num_c数组中
    for(i = dValue; num_c[i] == 0 && i >= 0; i-- ){
       continue;
    }                                 
    if(i >= 0){
        len = i + 1;                 // 倒着验到不为0的数，获取商的位数 
    }                             
    printf("%s / %s = ", num1, num2);
    if(blag == 3){
        printf("0");
        return;
    }
    if( len>=0 )
    {
        if(blag == 1){
            printf("-");
        }
        for(i = len-1; i >= 0; i-- )
            printf("%d", num_c[i]);
    }
    else
    {
        printf("0");
    }
} 

int SubStract(int p1[], int len1, int p2[], int len2)
{
    int i;
    //用返回值判断是否可以相减并将减后的值存入p1(-1表示不够减，0表示刚好够，够减则返回差的位数)
    //判断p1和p2的大小
    if(len1 < len2)
    {
        return -1;
    }
    if(len1 == len2 )
    {                        
        for(i = len1-1; i >= 0; i--)
        {
            if(p1[i] > p2[i])   // 满足条件，可做减法
                break;
            else if(p1[i] < p2[i]) // 不够减返回-1
                return -1;
        }
    }
    for(i = 0; i <= len1-1; i++)  // 从低位开始做减法
    {
        p1[i] -= p2[i];         // 相减 
        if(p1[i] < 0)           // 若是否需要借位
        {   
            p1[i] += 10;
            p1[i+1]--;
        }
    }
    for(i = len1-1; i >= 0; i--)  // 查找结果的最高位
    {
        if( p1[i] )             //最高位第一个不为0
            return (i+1);       //得到位数并返回
    } 
    return 0;                   //两数相等的时候返回0
}
```
