## Android NDK 开发


### 基础介绍

#### 静态注册C/C++函数

- **函数命名规则**

    Java_包名_类名_函数名
    ```
    JNIEXPORT void JNICALL
    Java_com_top_demo_helloCpp(JNIEnv* env, jobject obj)
    {
    
    }
    ```
    
    - 上面的JNIEXPORT和JNICALL是JNI的宏，用来标识该函数可以被JNI调用。
    - JNIEnv结构体指向了JNI的函数表，这些函数可以完成和Java的交互。
    - jobject是当前与之链接的native方法隶属的类对象，即调用这个JNI方法的对象。
    - 上面这两个参数由Java虚拟机调用的时候传入。

#### 动态注册C/C++函数

- 由于是将函数映射表注册到JVM中，所以函数的调用速度更快。
- 不用使用静态注册那套繁琐的命名规则。

- **注册**

```c
//编写需要使用的函数
static jstring nativeJNITest(JNIEnv *env, jobject) 
{
    std::string test = "你好，c++";
    return env->NewStringUTF(test.c_str());
}

static jint nativeComputeDamage(JNIEnv *env, jobject thiz, jint attack, jint agility)
{
    return (jint)(attack + agility * 1.2)
}

// 提供一个函数映射表，注册给JVM，这样JVM就可以通过函数映射表来调用相应的函数
// 这样的效率比静态注册的效率高
/**
 * @param1 Java中的native方法名。可以自定义。
 * @param2 函数签名，描述函数的返回值和参数
 * @param3 函数指针，指向被调用的c++函数。名车需要和函数名一样。
 */
static JNINativeMethod nativeMethod[] = {
    {"JNITest", "()Ljava/lang/String;", (void *) nativeJNITest},
    {"computeDamage", "(II)I", (void *)nativeComputeDamage}
};


//该函数在执行System.loadLibrary()后会被调用，用于向JVM注册函数表。
//返回值是使用的JNI版本。
JNIEXPORT jint JNICALL JNI_OnLoad(JavaVM *jvm, void *reserved)
{
    JNIEnv *env;
    //需要通过JVM动态的获取JNIEnv来提供Java介质
    if (jvm->GetEnv((void **) &env, JNI_VERSION_1_6) != JNI_OK) {
        return JNI_ERR;
    }
    
    //需要调用这些函数的类
    //一定要注意名称的正确性：包名 + 类名
    jclass clz = env->FindClass("com/example/xingxinyu/cppdemo/JNIHelper");
    if(clz == NULL){
        LOGE("类名不对");
    } else {
        LOGE("类加载成功");
    }
    
    jint method_size = sizeof(nativeMethod) / sizeof(nativeMethod[0]);
   /**
     * 注册函数表
     * @param1 需要关联到那个【Java】类，Kotlin类不行
     * @param2 方法数组
     * @param3 方法数
     */
    env->RegisterNatives(clz, nativeMethod, method_size);
    //返回使用的JNI版本                               
    return JNI_VERSION_1_6;
}

```


- **解注册**
向JVM中注册函数映射表后，因该在JVM释放改JNI组件时把其释放，不然就是隐患。

```c
//该函数在执行System.loadLibrary()后会被调用，用于向JVM注册函数表。
//返回值是使用的JNI版本。
JNIEXPORT void JNICALL JNI_OnUnload(JavaVM *jvm, void *reserved)
{
    JNIEnv *env;
    //需要通过JVM动态的获取JNIEnv来提供Java介质
    if (jvm->GetEnv((void **) &env, JNI_VERSION_1_6) != JNI_OK) {
        return;
    }
    jclass clz = env->FindClass("com/example/xingxinyu/cppdemo/JNIHelper");
    // 解注册函数表
    env->UnregisterNatives(clz);
}
```


#### JNI描述符

前面在动态注册时，需要生成函数映射表，其中需要一个是函数签名，它是由JNI描述符来描述的，写错了函数就会找不到。

Java|JNI
---|---
byte|B
char|C
short|S
int|I
long|J
float|F
double|D
boolean|Z

- 引用类型描述符
- 数组描述符
- 方法描述符

#### JNI数据类型

普遍规律：在普通数据类型前+j,比如: jobject对应Java的obejct

- 基本数据类型

Java Type|Native Type |Description
-------|-------|------
boolean|jboolean|unsigned 8 bits
byte|jbyte|signed 8 bits
char|jchar|unsigned 16 bits
short|jshort|signed 16 bits
int|jint|signed 32 bits
long|jlong|signed 64 bit
float|jfloat|32 bit
double|jdouble|64 bit


- 引用类型
```
jobject                             (all objects)
    |
    |--jclass                       (java.lang.Class instances)
    |--jstring
    |--jarray                       (arrays)
    |     |
    |     |--jobjectArray           (Object[])
    |     |--jboolArray             (boolean[])
    |     |--jbyteArray             (byte[])
    |     |--jcharArray             (char[])
    |     |--jshortArray            (short[])
    |     |--jintArray              (int[])
    |     |--jlongArray             (long[])
    |     |--jfloatArray            (float[])
    |     |--jdoubleArray           (double[])
    |--jthrowable                   (java.lang.Throwable objects)
```


------

### Java调用C


### C调用Java


### 三方工具

