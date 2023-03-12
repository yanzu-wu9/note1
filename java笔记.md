### 抽象类







### 接口









### 内部类

##### 内部类的分类

- 局部内部类
  - 能直接访问外部所有成员变量
  - 相当于类中的方法一样，外部不能直接，要先创建对象
- 匿名内部类
- 成员内部类
- 静态内部类





#### 异常

##### 异常分类

- Error
  - 不应该被捕获
  - 应该被声明
  - 无法恢复的错误
- Exception
  - 常见异常
    - ArrayIndexOutOfBoundsException
    - NullPointerException
    - ClassCastException
    - ArithmeticException
    - NumberFormatException

##### RuntimeException



![image-20230305143142314](E:\系统自带七文件\图片\Typora_pic\image-20230305143142314.png)

##### UncheckedException



![image-20230305143159174](E:\系统自带七文件\图片\Typora_pic\image-20230305143159174.png)



##### 处理异常的方法

1. throw
2. try...catch
3. throw  和 try...catch 一起使用





### IO流

##### 字节流

- InputStream(抽象类)
  - FileInputStream（低级流）
    - BufferedInputStream(缓冲流)
      - ObjectInputStream(系列化)
- OutputStream(抽象类)
  - FileOutputStream（低级流）
    - BufferedOutputStream(缓冲流)
      - ObjectInputStream(反系列化)
        - PrintStream（字节打印流）







##### 字符流

- Reader(抽象类)
  - FileReader（低级流）
    - BufferedReader(缓冲流)
      - InputStreamReader（转换流）
- Writer(抽象类)
  - FileWriter（低级流）
    - BufferedWriter(缓冲流)
      - OutputStreamWriter（转换流）
        - PrintWriter（字符打印流）



##### IO流资源管道关闭方式

- close();
- try...catch...finally（手动释放）
- try(定义流对象)...catch（自动释放）







### 线程

##### 线程创建方式

- Thread（继承Thread类，扩展性低）

  ```java
  class PrimeThread extends Thread {
           long minPrime;
           PrimeThread(long minPrime) {
               this.minPrime = minPrime;
           }
  
           public void run() {
               // compute primes larger than minPrime
                . . .
           }
       }
       
  PrimeThread p = new PrimeThread(143);
  p.start();
  ```

  

- Runnable（实现Runnable接口，可扩展性高，但没有返回值）

  ```java
  class PrimeRun implements Runnable {
           long minPrime;
           PrimeRun(long minPrime) {
               this.minPrime = minPrime;
           }
  
           public void run() {
               // compute primes larger than minPrime
                . . .
           }
       }
       
  PrimeRun p = new PrimeRun(143);
  new Thread(p).start();
  ```

  

- Callable（实现Callable接口，有返回值，可扩展性高）

  ```java
  Callable<String> callable = new Callable<>() {
              @Override
              public String call() throws Exception {
                  return null;
              }
          };
  FutureTask<String> stringFutureTask = new FutureTask<>(callable);
  new Thread(stringFutureTask).start();
      }
  ```

  



##### 线程安全问题

- 多个线程共享一个资源，无先后顺序（例如：银行取钱可以白嫖）





##### 解决方法

- 线程同步（核心思想：上锁==synchronized==）

  - 同步代码块

  ```java
  synchronized (锁对象) {
              for (int i = 0; i < 10; i++) {
                  System.out.println("hello~~");
              }
          }
  ```

  

  - 同步方法

  ```java
  synchronized（默认：this） void method3(){
          for (int i = 0; i < 10; i++) {
              System.out.println("hello~~");
          }
      }
  ```

  - 锁对象规范要求
    - 实例对象建议使用==this==
    - 静态方法建议使用==字节码（类名.Class）==



- Lock锁（更加灵活）

  - ReentrantLock

  ```java
  class X {
     private final ReentrantLock lock = new ReentrantLock();
     // ...
  
     public void m() {
       lock.lock();  // block until condition holds
       try {
         // ... method body
       } finally {
         lock.unlock()
       }
     }
   }
  ```

  



#### 线程池(ExecutorService)

##### 线程池创建的方法

- ExecutorService实现类：ThreadPoolExecutor等

  - ThreadPoolExecutor构造器的七个参数

    - corePoolSize：核心线程

    - maximumPoolSize：最大线程

    - keepAliveTime：临时线程存活时间

    - TimeUnit unit：时间单位

    - BlockingQueue<Runnable> workQueue：等待区工作队列

    - ThreadFactory threadFactory：生成线程工厂

    - RejectExecutionHandler hander：拒绝方式

      ```java
      ExecutorService pool = new ThreadPoolExecutor(3,5,6,
                      TimeUnit.SECONDS, 
                      new ArrayBlockingQueue<>(5),
                      Executors.defaultThreadFactory(),
                      new ThreadPoolExecutor.AbortPolicy());
      ```

  - Runnable

    ```java
     pool.execute(new Runnable() {
                @Override
                public void run() {
                    System.out.println("hello");
                }
            });
    ```

     

  - Callable

    ```java
    Future<String> submit = pool.submit(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    return null;
                }
            });
    ```







- Executors工具类 
  - ![image-20230306123055166](E:\系统自带七文件\图片\Typora_pic\image-20230306123055166.png)





#### 定时器

##### 定时器创建方法

- Timer（单线程）

  ```java
  Timer timer = new Timer();
          timer.schedule(new TimerTask() {
              @Override
              public void run() {
                  System.out.println("hello~~");
              }
          },2000,3000);
  ```

  

- ScheduledExecutorService（线程池）

  ```java
  pool.scheduleAtFixedRate(new TimerTask() {
              @Override
              public void run() {
                  System.out.println(Thread.currentThread().getName()+"hello~~");
              }
          },2,3,TimeUnit.SECONDS);
  ```

  

##### 线程并发与并行

##### 并发

- CPU分时轮询的执行线程（一对多）



##### 并行

- 同一个时刻同时在运行（多对多）



#### 线程生命周期

![image-20230306151501615](E:\系统自带七文件\图片\Typora_pic\image-20230306151501615.png)





### 反射

#### 反射获取类对象的方式

- ```java
  Class c1 = Class.forName("类的全限名");
  ```

- ```java
  Class c2 = 类名.Class;
  ```

- ```java
  Class c3 = 对象.getClass();
  ```



#### 反射获取Constructor（构造器）的方式

- ```java
  Constructor constructor = c.getDeclaredConstructor();//获取所有无参构造器
  ```

- ```java
  Constructor constructor = c.getDeclaredConstructor(Class<?>... parameterTypes);//获取有参构造器
  //例如：
  	Constructor constructor = c.getDeclaredConstructor(String.class, Integer.class);
  ```

  

##### 反射Constructor的作用

- 创建对象（public）

  ```java
  Object p = (Object) constructor.newInstance();
  ```



- 破坏封装性，能强制打开私有权限（private）

  ```java
  constructor.setAccessible(true);
  Object p = (Object) constructor.newInstance();
  ```

  

#### 反射Field（成员属性）

##### 获取Field

```java
Field[] fields = c.getDeclaredFields();//取所有属性
Field field = c.getDeclaredField(String name);//取指定的成员变量名
```



##### Field的作用

- 赋值（值找对象）

  ```java
  People p = new People();
  field.set(p, "李四");
  ```



- 取值（值找对象）

  ```java
  People p = new People();
  String name = (String) field.get(p);
  ```

  





#### 反射获取Method（方法）

##### 获取Method的方式

- ```java
  Method method = getDeclaredMethod(String name, Class<?>... parameterTypes);//指定方法名与方法中的参数获取
  ```

- ```java
  Method method = getDeclaredMethods();//获取所有方法
  ```

  

##### Method的作用

==（以前是对象找行为，现在是行为找对象）==

- 执行方法

  ```java
  Object invoke =	invoke(Object obj, Object... args)；
  Dog dog = new Dog();
  method.invoke(dog,"张三");
  ```



#### 反射的作用

- 突破泛型的约束==（Class文件进入运行的时候，泛型自动擦除）==
  - 绕过编译阶段为集合添加数据
- 突破封装的约束

- 运行时得到一个类的全部成分然后操作
- 可以做高级框架



### 注解

#### 注解的作用

- 对java中的类，方法，成员变量做标记，然后进行特殊处理



#### 自定义注解

##### 格式

```java
public @interface Book {
    String name() default "《张三传》";
    int price();
}

@Book(name = "《李四传》",price = 39)
class A{
    ...
}
```



#### 元注解

##### 概念

==标记注解的注解==



#### 常见元注解

- @Target：约束注解的使用范围

  - Type：类

  - Method：方法

  - Field：成员变量

    ```java
    @Target({ElementType.TYPE,ElementType.METHOD,ElementType.FIELD})
    ```

    

- @Retention：申明注解的生命周期

  - RetentionPolicy.RUNTIME：作用于源码阶段，字节码文件，运行阶段（开发常用）
  - RetentionPolicy.SOURCE：只作用在源码阶段，生成的字节码文件中不存在
  - RetentionPolicy.CLASS（默认）：作用在源码阶段，字节码文件，运行阶段不存在

#### 注解解析

##### 解析注解的技巧

![image-20230307092801103](E:\系统自带七文件\图片\Typora_pic\image-20230307092801103.png)



