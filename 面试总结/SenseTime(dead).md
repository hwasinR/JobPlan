### 1. 哪些函数不能声明为虚函数  
构造函数与静态成员函数  

### 2. 静态成员函数如何访问成员变量  
方法一:声明类的所有数据成员都是静态的  
方法二:模仿成员函数传递this指针，增加静态成员函数的形参为对象的引用  
方法三:定义一个（一组）静态变量保存每个对象的this指针

### 3. 单例类的线程安全  
#### A. double check: 先检查是否已经有实例，有实例则不获取锁防止阻塞耗时，获取锁以后再检查一下（防止第一次检查m_instance_ptr时还是空指针，但等获取锁后别的线程已经new出来了，m_instance_ptr不再是空）
```
 static Ptr get_instance(){

        // "double checked lock"
        if(m_instance_ptr==nullptr){
            std::lock_guard<std::mutex> lk(m_mutex);
            if(m_instance_ptr == nullptr){
              m_instance_ptr = std::shared_ptr<Singleton>(new Singleton);
            }
        }
        return m_instance_ptr;
    }
```

#### B. Meyers' Singleton：
如果当变量在初始化的时候，并发同时进入声明语句，并发线程将会阻塞等待初始化结束。（c++11）
```
    static Singleton& get_instance(){
        static Singleton instance;
        return instance;
    }
```

#### C.饱汉模式
```
class Singelton{
private:
    Singelton(){
        m_count ++;
        printf("Singelton begin\n");
        Sleep(1000);                            // 加sleep为了放大效果
        printf("Singelton end\n");
    }
    static Singelton *single;
public:
    static Singelton *GetSingelton();
    static void print();
    static int m_count;
};
// 饿汉模式的关键：初始化即实例化
Singelton *Singelton::single = new Singelton;
int Singelton::m_count = 0;
```

### 右值


### 音频编解码

#### 右移
1.逻辑左移和算术左移，都是在右边补0，效果一样。左移1bit，相当于原数乘以2。
C/C++中，对于无符号数，可以认为是逻辑左移和逻辑右移。对于有符号数，可以认为是算术左移和算术右移。
逻辑右移：右移后，左边补0;
算术右移：右移后，左边补符号位，添加的位与原数的符号位相同，正数(0算作正数的一个)补0，负数补1。算术右移1bit，相当于原数 除以2。

#### 16PCM

将采样的每一帧信号，使用16比特量化，16比特排列为：

S(符号位) + 0...1..0(X位强度位) + 采样数据位(12-X) + 000(3位补零)

其中强度位可以使1,01,001,0001,00001,000001,0000001,0000000共计8种,信号的强度越大，其强度位越多,采样的数据越少（13折线法）

#### 16PCM转换为alaw
```
1. 保留符号位
2. 将符号位（8种可能）转换为3位比特位表示 0000000->000 0000001->001 000001 -> 010等等
3. 采样数据位取最高4位
4. 将符号位，强度位，采样数据位拼接在一起，若符号位为1,则异或01010101,否则异或11010101
```
#### alaw转换为16PCM
```
1. 利用01010101解异或8位alaw数据
2. 分别取出强度位A与4位数据位B
3. 将强度位A还原成原始强度位C，将C拼接在B前面，在B后面补一位1,根据缺少的位数补0，使原始强度位C + 4位数据位 +一个'1' + 0的位数 = 15位
4. 若符号位为0,则将符号位取反
```

### 算法
https://leetcode-cn.com/problems/single-number-iii/solution/zhi-chu-xian-yi-ci-de-shu-zi-iii-by-leet-4i8e/

1. 所有的数字异或得到异或值x,此x实际上是两个只出现1次数字a和b的异或值。
2. 选取此异或值x二进制表示的不为0的最低位Xi,数组所有数字按照Xi是否为0进行分组
3. 分别计算2个分组的异或值，即为2个只出现1次的数字。