---
layout: post
title:  "Java Multi-thread Notes"
date:   2018-09-05 11:41:00 +0800
featured-img: java_multithread
categories: java_multithread
---

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_11-49-01.png)

从上面的源代码中可以发现，Thread类实现了Runnable接口，它们之间具有多态关系。

```java
public class MyThread extends Thread{
    @Override
    public void run() {
        super.run();
        System.out.println("MyThread");
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
        System.out.println("运行结束！");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_12-01-51.png)

从以上运行结果来看，MyThread.java类中的run方法执行的时间比较晚，这也说明在使用多线程技术时，代码的运行结果与代码执行顺序或调用顺序是无关的。

线程是一个子任务，CPU以不确定的方式，或者说是以随机的时间来调用线程中的run方法，所以就会出现先打印“运行结束！”后输出“MyThread”这样的结果了。

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_13-22-56.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_13-25-37.png)

另外还需要注意一下，执行start()方法的顺序不代表线程启动的顺序。

```java
public class MyThread extends Thread{
    private int i;

    public MyThread(int i) {
        super();
        this.i = i;
    }

    @Override
    public void run() {
        System.out.println(i);
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        MyThread t11 = new MyThread(1);
        MyThread t12 = new MyThread(2);
        MyThread t13 = new MyThread(3);
        MyThread t14 = new MyThread(4);
        MyThread t15 = new MyThread(5);
        MyThread t16 = new MyThread(6);
        MyThread t17 = new MyThread(7);
        MyThread t18 = new MyThread(8);
        MyThread t19 = new MyThread(9);
        MyThread t110 = new MyThread(10);
        MyThread t111 = new MyThread(11);
        MyThread t112 = new MyThread(12);
        MyThread t113 = new MyThread(13);

        t11.start();
        t12.start();
        t13.start();
        t14.start();
        t15.start();
        t16.start();
        t17.start();
        t18.start();
        t19.start();
        t110.start();
        t111.start();
        t112.start();
        t113.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_14-05-23.png)

使用继承Thread类的方式来开发多线程应用程序在设计上是有局限性的，因为Java是单根继承，不支持多继承，所以为了改变这种限制，可以使用实现Runnable接口的方式来实现多线程技术。

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_14-11-58.png)

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Running");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_14-22-48.png)

```java
public class Run {
    public static void main(String[] args) {
        Runnable runnable=new MyRunnable();
        Thread thread=new Thread(runnable);
        thread.start();
        System.out.println("Running is over!");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_14-23-52.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_14-25-37.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_14-25-51.png)



### 留意i--与System.out.println()的异常

```java
public class MyThread extends Thread{
    private int i = 5;

    @Override
    public void run() {
        System.out.println("i=" + (i--) + " threadName="
                + Thread.currentThread().getName());
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        MyThread run = new MyThread();

        Thread t1 = new Thread(run);
        Thread t2 = new Thread(run);
        Thread t3 = new Thread(run);
        Thread t4 = new Thread(run);
        Thread t5 = new Thread(run);

        t1.start();
        t2.start();
        t3.start();
        t4.start();
        t5.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_15-37-21.png)

本实验的测试目的是：虽然println()方法在内部是同步的，但i--的操作却是在进入println()之前发生的，所以有发生非线程安全问题的概率。

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-05_15-39-59.png)



### 方法内的变量为线程安全

```java
public class HasSelfPrivateNum {
    public void addI(String username) {
        try {
            int num = 0;
            if (username.equals("a")) {
                num = 100;
                System.out.println("a set over!");
                Thread.sleep(2000);
            } else {
                num = 200;
                System.out.println("b set over!");
            }
            System.out.println(username + " num=" + num);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}

```

```java
public class Run {
    public static void main(String[] args) {

        HasSelfPrivateNum numRef = new HasSelfPrivateNum();

        ThreadA athread = new ThreadA(numRef);
        athread.start();

        ThreadB bthread = new ThreadB(numRef);
        bthread.start();

    }
}
```

```java
public class ThreadA extends Thread {
    private HasSelfPrivateNum numRef;

    public ThreadA(HasSelfPrivateNum numRef) {
        super();
        this.numRef = numRef;
    }

    @Override
    public void run() {
        super.run();
        numRef.addI("a");
    }
}
```

```java
public class ThreadB extends Thread {
    private HasSelfPrivateNum numRef;

    public ThreadB(HasSelfPrivateNum numRef) {
        super();
        this.numRef = numRef;
    }

    @Override
    public void run() {
        super.run();
        numRef.addI("b");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-06_10-16-49.png)

可见，方法中的变量不存在非线程安全问题，永远都是线程安全的。这是方法内部的变量是私有的特性造成的。



### 实例变量非线程安全

```java
public class HasSelfPrivateNum {
    private int num = 0;
    public void addI(String username) {
        try {
            if (username.equals("a")) {
                num = 100;
                System.out.println("a set over!");
                Thread.sleep(2000);
            } else {
                num = 200;
                System.out.println("b set over!");
            }
            System.out.println(username + " num=" + num);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-06_10-35-06.png)

将前一个示例中的HasSelfPrivateNum类修改后的运行结果如上所示，说明当两个线程同时访问一个没有同步的方法，如果两个线程同时操作业务对象中的实例变量，则可能会出现“非线程安全”问题。此示例的知识点在前面已经介绍过，只要在public void addI(String username)方法前加关键字synchronized即可。

```java
public class HasSelfPrivateNum {
    private int num = 0;
    synchronized public void addI(String username) {
        try {
            if (username.equals("a")) {
                num = 100;
                System.out.println("a set over!");
                Thread.sleep(2000);
            } else {
                num = 200;
                System.out.println("b set over!");
            }
            System.out.println(username + " num=" + num);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-06_10-46-36.png)



### 多个对象多个锁

```java
public class Run {
    public static void main(String[] args) {

        HasSelfPrivateNum numRef1 = new HasSelfPrivateNum();
        HasSelfPrivateNum numRef2 = new HasSelfPrivateNum();

        ThreadA athread = new ThreadA(numRef1);
        athread.start();

        ThreadB bthread = new ThreadB(numRef2);
        bthread.start();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-07_11-47-04.png)

修改Run类中的代码，其余部分引用上例代码，运行结果如图所示。

上面示例是两个线程分别访问同一个类的两个不同实例的相同名称的同步方法，效果却是以异步的方式运行的。本示例由于创建了2个业务对象，在系统中产生出2个锁，所以运行结果是异步的，打印的效果就是先打印b，然后打印a。



### 脏读

```java
public class PublicVar {
    public String username = "A";
    public String password = "AA";

    synchronized public void setValue(String username, String password) {
        try {
            this.username = username;
            Thread.sleep(5000);
            this.password = password;

            System.out.println("setValue method thread name="
                    + Thread.currentThread().getName() + " username="
                    + username + " password=" + password);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void getValue() {
        System.out.println("getValue method thread name="
                + Thread.currentThread().getName() + " username=" + username
                + " password=" + password);
    }
}
```

```java
public class ThreadA extends Thread{
    private PublicVar publicVar;

    public ThreadA(PublicVar publicVar) {
        super();
        this.publicVar = publicVar;
    }

    @Override
    public void run() {
        super.run();
        publicVar.setValue("B", "BB");
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        try {
            PublicVar publicVarRef = new PublicVar();
            ThreadA thread = new ThreadA(publicVarRef);
            thread.start();

            Thread.sleep(200);// the output result is affected by this value

            publicVarRef.getValue();
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-07_13-49-52.png)

出现脏读是因为`public void getValue()`方法并不是同步的，所以可以在任意时候进行调用。解决办法当然就是加上同步synchronized关键字。

```java
synchronized public void getValue() {
        System.out.println("getValue method thread name="
                + Thread.currentThread().getName() + " username=" + username
                + " password=" + password);
    }
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-07_13-57-09.png)



### synchronized锁重入

```java
public class MyThread extends Thread{
    @Override
    public void run() {
        Service service = new Service();
        service.service1();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();
    }
}
```

```java
public class Service {
    synchronized public void service1() {
        System.out.println("service1");
        service2();
    }

    synchronized public void service2() {
        System.out.println("service2");
        service3();
    }

    synchronized public void service3() {
        System.out.println("service3");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_20-40-47.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_20-49-13.png)

可重入锁也支持在父子类继承的环境中。

```java
public class Main {
    public int i = 10;

    synchronized public void operateIMainMethod() {
        try {
            i--;
            System.out.println("main print i=" + i);
            Thread.sleep(100);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

```java
public class Sub extends Main{
    synchronized public void operateISubMethod() {
        try {
            while (i > 0) {
                i--;
                System.out.println("sub print i=" + i);
                Thread.sleep(100);
                this.operateIMainMethod();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class MyThread extends Thread{
    @Override
    public void run() {
        Sub sub = new Sub();
        sub.operateISubMethod();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_20-57-56.png)



### 出现异常，锁自动释放

当一个线程执行的代码出现异常时，其所持有的锁会自动释放。



### 用同步代码块解决同步方法的弊端（一半异步，一半同步）

```java
public class MyThread1 extends Thread{
    private Task task;

    public MyThread1(Task task) {
        super();
        this.task = task;
    }

    @Override
    public void run() {
        super.run();
        task.doLongTimeTask();
    }
}
```

```java
public class MyThread2 extends Thread{
    private Task task;

    public MyThread2(Task task) {
        super();
        this.task = task;
    }

    @Override
    public void run() {
        super.run();
        task.doLongTimeTask();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        Task task = new Task();

        MyThread1 thread1 = new MyThread1(task);
        thread1.start();

        MyThread2 thread2 = new MyThread2(task);
        thread2.start();
    }
}
```

```java
public class Task {
    public void doLongTimeTask(){
        for (int i = 0; i < 100; i++){
            System.out.println("nosynchronized threadName=" + Thread.currentThread().getName() + "i=" + (i + 1));
        }
        System.out.println("");
        synchronized (this){
            for (int i = 0; i < 100; i++){
                System.out.println("synchronized threadName=" + Thread.currentThread().getName() + " i=" + (i + 1));
            }
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-07_17-54-47.png)![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-07_17-59-55.png)



### 静态同步synchronized方法与synchronized（class）代码块

```java
public class Run {
    public static void main(String[] args) {

        Service service = new Service();
        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();

        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();

        ThreadC c = new ThreadC(service);
        c.setName("C");
        c.start();
    }
}
```

```java
public class Service {
    synchronized public static void printA() {
        try {
            System.out.println("线程名称为：" + Thread.currentThread().getName()
                    + "在" + System.currentTimeMillis() + "进入printA");
            Thread.sleep(3000);
            System.out.println("线程名称为：" + Thread.currentThread().getName()
                    + "在" + System.currentTimeMillis() + "离开printA");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    synchronized public static void printB() {
        System.out.println("线程名称为：" + Thread.currentThread().getName() + "在"
                + System.currentTimeMillis() + "进入printB");
        System.out.println("线程名称为：" + Thread.currentThread().getName() + "在"
                + System.currentTimeMillis() + "离开printB");
    }

    synchronized public void printC(){
        System.out.println("线程名称为：" + Thread.currentThread().getName() + "在"
                + System.currentTimeMillis() + "进入printC");
        System.out.println("线程名称为：" + Thread.currentThread().getName() + "在"
                + System.currentTimeMillis() + "离开printC");
    }
}
```

```java
public class ThreadA extends Thread {

    private Service service;

    public ThreadA(Service service) {
        this.service = service;
    }

    @Override
    public void run() {
        service.printA();
    }
}
```

```java
public class ThreadB extends Thread {

    private Service service;

    public ThreadB(Service service) {
        this.service = service;
    }

    @Override
    public void run() {
        service.printB();
    }
}
```

```java
public class ThreadC extends Thread {

    private Service service;

    public ThreadC(Service service) {
        this.service = service;
    }

    @Override
    public void run() {
        service.printC();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-08_21-39-54.png)

线程a，b是同步的，但c与他们是异步的，原因是持有不同的锁，一个是对象锁，另外一个是Class锁，而Class锁可以对类的所有对象实例 起作用。修改Run.java类可以证明：

```java
public class Run {
    public static void main(String[] args) {
        Service service1 = new Service();
        Service service2 = new Service();

        ThreadA a = new ThreadA(service1);
        a.setName("A");
        a.start();

        ThreadB b = new ThreadB(service2);
        b.setName("B");
        b.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-08_21-52-36.png)

同步`synchronized(class)`代码块的作用其实和`synchronized static`方法的作用一样。修改类Service.java，Run.java仍使用刚刚修改过的。

```java
public class Service {
    public static void printA() {
        synchronized (Service.class) {
            try {
                System.out.println("线程名称为：" + Thread.currentThread().getName()
                        + "在" + System.currentTimeMillis() + "进入printA");
                Thread.sleep(3000);
                System.out.println("线程名称为：" + Thread.currentThread().getName()
                        + "在" + System.currentTimeMillis() + "离开printA");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

    }

    public static void printB() {
        synchronized (Service.class) {
            System.out.println("线程名称为：" + Thread.currentThread().getName()
                    + "在" + System.currentTimeMillis() + "进入printB");
            System.out.println("线程名称为：" + Thread.currentThread().getName()
                    + "在" + System.currentTimeMillis() + "离开printB");
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-08_22-02-05.png)



### 数据类型String的常量池特性

```java
public class Service {
    public static void print(String stringParam) {
        try {
            synchronized (stringParam) {
                while (true) {
                    System.out.println(Thread.currentThread().getName());
                    Thread.sleep(1000);
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private Service service;
    public ThreadA(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.print("AA");
    }
}
```

```java
public class ThreadB extends Thread{
    private Service service;
    public ThreadB(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.print("AA");
    }
}
```

```java
public class Run {
    public static void main(String[] args) {

        Service service = new Service();

        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();

        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_21-13-47.png)

出现这样的情况就是因为String的两个值都是AA，两个线程持有相同的锁，所以造成线程B不能执行。这就是Stirng常量池所带来的问题。因此在大多数的情况下，同步synchronized代码块都并不适用String作为锁对象，而改用其他，比如`new Object`实例化一个`Object`对象，但它并不放入缓存中。



### 多线程死锁

```java
public class DealThread implements Runnable {

    public String username;
    public Object lock1 = new Object();
    public Object lock2 = new Object();

    public void setFlag(String username) {
        this.username = username;
    }

    @Override
    public void run() {
        if (username.equals("a")) {
            synchronized (lock1) {
                try {
                    System.out.println("username = " + username);
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                synchronized (lock2) {
                    System.out.println("Executed in the order of lock1->lock2");
                }
            }
        }
        if (username.equals("b")) {
            synchronized (lock2) {
                try {
                    System.out.println("username = " + username);
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                synchronized (lock1) {
                    System.out.println("Executed in the order of lock2->lock1");
                }
            }
        }
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        try {
            DealThread t1 = new DealThread();
            t1.setFlag("a");

            Thread thread1 = new Thread(t1);
            thread1.start();

            Thread.sleep(100);

            t1.setFlag("b");
            Thread thread2 = new Thread(t1);
            thread2.start();
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_21-40-08.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_21-43-33.png)



### 内置类与静态内置类

```java
public class PublicClass {
    private String username;
    private String password;

    class PrivateClass {
        private String age;
        private String address;

        public String getAge() {
            return age;
        }

        public void setAge(String age) {
            this.age = age;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        public void printPublicProperty() {
            System.out.println(username + " " + password);
        }
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

}
```

```java
public class Run {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();
        publicClass.setUsername("usernameValue");
        publicClass.setPassword("passwordValue");

        System.out.println(publicClass.getUsername() + " "
                + publicClass.getPassword());

        PublicClass.PrivateClass privateClass = publicClass.new PrivateClass();
        privateClass.setAge("ageValue");
        privateClass.setAddress("addressValue");

        System.out.println(privateClass.getAge() + " "
                + privateClass.getAddress());
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-09_22-15-35.png)

如果`PublicClass.java`类和`Run.java`类不在同一个包中，则需要将`PrivateClass`内置声明成`public`公开的。

想要实例化内置类必须使用如下代码：

```java
PublicClass.PrivateClass privateClass = publicClass.new PrivateClass();
```

内置类还有一种叫做静态内置类。

```java
public class PublicClass {
    static private String username;
    static private String password;

    static class PrivateClass {
        private String age;
        private String address;

        public String getAge() {
            return age;
        }

        public void setAge(String age) {
            this.age = age;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        public void printPublicProperty() {
            System.out.println(username + " " + password);
        }
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

```java
public class Run {
    public static void main(String[] args) {

        PublicClass publicClass = new PublicClass();
        publicClass.setUsername("usernameValue");
        publicClass.setPassword("passwordValue");

        System.out.println(publicClass.getUsername() + " "
                + publicClass.getPassword());

        PublicClass.PrivateClass privateClass = new PublicClass.PrivateClass();
        privateClass.setAge("ageValue");
        privateClass.setAddress("addressValue");

        System.out.println(privateClass.getAge() + " "
                + privateClass.getAddress());
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_21-31-19.png)



只要对象不变，即使对象的属性被改变，运行的结果还是同步。

```java
public class Userinfo {
    private String username;
    private String password;

    public Userinfo() {
    }

    public Userinfo(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

```java
public class Service {
    public void serviceMethodA(Userinfo userinfo) {
        synchronized (userinfo) {
            try {
                System.out.println(Thread.currentThread().getName());
                userinfo.setUsername("abcabcabc");
                Thread.sleep(3000);
                System.out.println("end! time=" + System.currentTimeMillis());
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private Service service;
    private Userinfo userinfo;

    public ThreadA(Service service,
                   Userinfo userinfo) {
        super();
        this.service = service;
        this.userinfo = userinfo;
    }

    @Override
    public void run() {
        service.serviceMethodA(userinfo);
    }
}
```

```java
public class ThreadB extends Thread {
    private Service service;
    private Userinfo userinfo;

    public ThreadB(Service service,
                   Userinfo userinfo) {
        super();
        this.service = service;
        this.userinfo = userinfo;
    }

    @Override
    public void run() {
        service.serviceMethodA(userinfo);
    }
}
```

```java
public class Run {
    public static void main(String[] args) {

        try {
            Service service = new Service();
            Userinfo userinfo = new Userinfo();

            ThreadA a = new ThreadA(service, userinfo);
            a.setName("a");
            a.start();
            Thread.sleep(50);
            ThreadB b = new ThreadB(service, userinfo);
            b.setName("b");
            b.start();

        } catch (InterruptedException e) {
            e.printStackTrace();
        }

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_12-09-52.png)



### 解决异常死循环

```java
public class RunThread extends Thread {
    private boolean isRunning = true;
    public boolean isRunning(){
        return isRunning;
    }

    public void setRunning(boolean running) {
        isRunning = running;
    }

    @Override
    public void run() {
        System.out.println("in the run");
        while (isRunning == true){
        }
        System.out.println("the thread is stopped!");
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        try {
            RunThread thread = new RunThread();
            thread.start();
            Thread.sleep(1000);
            thread.setRunning(false);
            System.out.println("已经赋值为false");
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_16-52-50.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_18-02-10.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_18-01-48.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_18-20-13.png)

```java
public class RunThread extends Thread {
    volatile private boolean isRunning = true;
    public boolean isRunning(){
        return isRunning;
    }

    public void setRunning(boolean running) {
        isRunning = running;
    }

    @Override
    public void run() {
        System.out.println("in the run");
        while (isRunning == true){
        }
        System.out.println("the thread is stopped!");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_16-56-19.png)



### volatile非原子的特性

```java
public class MyThread extends Thread {
    volatile public static int count;
    private static void addCount(){
        for (int i = 0; i < 100; i++){
            count++;
        }
        System.out.println("count=" + count);
    }

    @Override
    public void run() {
        addCount();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        MyThread[] myThreads = new MyThread[100];
        for (int i = 0; i < 100; i++){
            myThreads[i] = new MyThread();
        }
        for (int i = 0; i < 100; i++){
            myThreads[i].start();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_18-38-19.png)

关键字`volatile`虽然增加了实例变量在多个线程之间的可见性，但它却不具备同步性，那么也就不具备原子性。

更改自定义线程类MyThread.java文件代码如下：

```java
public class MyThread extends Thread {
    volatile public static int count;
    synchronized private static void addCount(){
        for (int i = 0; i < 100; i++){
            count++;
        }
        System.out.println("count=" + count);
    }

    @Override
    public void run() {
        addCount();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_18-52-49.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_19-25-11.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-22-33.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-24-02.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_18-57-45.png)

用图来演示一下使用关键字`volatile`时出现非线程安全的原因。

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-01-25.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-02-25.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-06-28.png)



### 使用原子类进行i++操作

```java
import java.util.concurrent.atomic.AtomicInteger;

public class AddCountThread extends Thread {
    private AtomicInteger count = new AtomicInteger(0);

    @Override
    public void run() {
        for (int i = 0; i < 10000; i++){
            System.out.println(count.incrementAndGet());
        }
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        AddCountThread countService = new AddCountThread();

        Thread t1 = new Thread(countService);
        t1.start();

        Thread t2 = new Thread(countService);
        t2.start();

        Thread t3 = new Thread(countService);
        t3.start();

        Thread t4 = new Thread(countService);
        t4.start();

        Thread t5 = new Thread(countService);
        t5.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-19-16.png)



### 原子类也并不完全安全

```java
import java.util.concurrent.atomic.AtomicLong;

public class MyService {
    public static AtomicLong aiRef = new AtomicLong();
    public void addNum(){
        System.out.println(Thread.currentThread().getName() + "the value after adding 100:"
                + aiRef.addAndGet(100));
        aiRef.addAndGet(1);
    }
}
```

```java
public class MyThread extends Thread{
    private MyService mySerivce;

    public MyThread(MyService mySerivce) {
        super();
        this.mySerivce = mySerivce;
    }

    @Override
    public void run() {
        mySerivce.addNum();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        try {
            MyService service = new MyService();

            MyThread[] array = new MyThread[5];
            for (int i = 0; i < array.length; i++) {
                array[i] = new MyThread(service);
            }
            for (int i = 0; i < array.length; i++) {
                array[i].start();
            }
            Thread.sleep(1000);
            System.out.println(service.aiRef.get());
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-35-25.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-41-06.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-41-32.png)

更改`MyService.java`文件代码如下：

```java
import java.util.concurrent.atomic.AtomicLong;

public class MyService {
    public static AtomicLong aiRef = new AtomicLong();
    synchronized public void addNum(){
        System.out.println(Thread.currentThread().getName() + "the value after adding 100:"
                + aiRef.addAndGet(100));
        aiRef.addAndGet(1);
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_20-45-25.png)

 

### synchronized代码块有volatile同步的功能

```java
public class Service {
    private boolean isContinueRun = true;

    public void runMethod() {
        String anyString = new String();
        while (isContinueRun == true) {
        }
        System.out.println("停下来了！");
    }

    public void stopMethod() {
        isContinueRun = false;
    }
}
```

```java
public class ThreadA extends Thread {
    private Service service;

    public ThreadA(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.runMethod();
    }
}
```

```java
public class ThreadB extends Thread{
    private Service service;

    public ThreadB(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.stopMethod();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        try {
            Service service = new Service();

            ThreadA a = new ThreadA(service);
            a.start();

            Thread.sleep(1000);

            ThreadB b = new ThreadB(service);
            b.start();

            System.out.println("已经发起停止的命令了！");
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_21-05-50.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_21-10-24.png)

```java
public class Service {
    private boolean isContinueRun = true;

    public void runMethod() {
        String anyString = new String();
        while (isContinueRun == true) {
            synchronized (anyString) {
            }
        }
        System.out.println("停下来了！");
    }

    public void stopMethod() {
        isContinueRun = false;
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-10_21-11-56.png)



### 方法wait()锁释放与notify()锁不释放

当方法wait()被执行后，锁被自动释放，但执行完notify()方法，锁却不自动释放。

```java
public class Service {
    public void testMethod(Object lock) {
        try {
            synchronized (lock) {
                System.out.println("begin wait()");
                lock.wait();
                System.out.println("  end wait()");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private Object lock;

    public ThreadA(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        Service service = new Service();
        service.testMethod(lock);
    }
}
```

```java
public class ThreadB extends Thread {
    private Object lock;

    public ThreadB(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        Service service = new Service();
        service.testMethod(lock);
    }
}
```

```java
public class Test {
    public static void main(String[] args) {

        Object lock = new Object();

        ThreadA a = new ThreadA(lock);
        a.start();

        ThreadB b = new ThreadB(lock);
        b.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-11_21-40-12.png)

如果将`wait()`改成`sleep()`方法，就成了同步的效果。

```java
public class Service {
    public void testMethod(Object lock) {
        try {
            synchronized (lock) {
                System.out.println("begin wait()");
                Thread.sleep(4000);
                System.out.println("  end wait()");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-11_21-44-33.png)



方法notify()被执行后，不释放锁。

```java
public class NotifyThread extends Thread {
    private Object lock;
    public NotifyThread(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        Service service = new Service();
        service.synNotifyMethod(lock);
    }
}
```

```java
public class Service {
    public void testMethod(Object lock) {
        try {
            synchronized (lock) {
                System.out.println("begin wait() ThreadName="
                        + Thread.currentThread().getName());
                lock.wait();
                System.out.println("  end wait() ThreadName="
                        + Thread.currentThread().getName());
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void synNotifyMethod(Object lock) {
        try {
            synchronized (lock) {
                System.out.println("begin notify() ThreadName="
                        + Thread.currentThread().getName() + " time="
                        + System.currentTimeMillis());
                lock.notify();
                Thread.sleep(5000);
                System.out.println("  end notify() ThreadName="
                        + Thread.currentThread().getName() + " time="
                        + System.currentTimeMillis());
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class SynNotifyMethodThread extends Thread {
    private Object lock;

    public SynNotifyMethodThread(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        Service service = new Service();
        service.synNotifyMethod(lock);
    }

}
```

```java
public class ThreadA extends Thread {
    private Object lock;

    public ThreadA(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        Service service = new Service();
        service.testMethod(lock);
    }
}
```

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {

        Object lock = new Object();

        ThreadA a = new ThreadA(lock);
        a.start();

        NotifyThread notifyThread = new NotifyThread(lock);
        notifyThread.start();

        SynNotifyMethodThread c = new SynNotifyMethodThread(lock);
        c.start();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-11_22-10-26.png)

此实验说明，必须执行完notify()方法所在的同步`synchronized`代码块后才释放锁。



### 当interrupt方法遇到wait方法

```java
public class Service {
    public void testMethod(Object lock) {
        try {
            synchronized (lock) {
                System.out.println("begin wait()");
                lock.wait();
                System.out.println("  end wait()");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
            System.out.println("出现异常了，因为呈wait状态的线程被interrupt了！");
        }
    }
}
```

```java
public class ThreadA extends Thread{
    private Object lock;

    public ThreadA(Object lock) {
        super();
        this.lock = lock;
    }

    @Override
    public void run() {
        Service service = new Service();
        service.testMethod(lock);
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        try {
            Object lock = new Object();

            ThreadA a = new ThreadA(lock);
            a.start();

            Thread.sleep(5000);

            a.interrupt();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_10-24-35.png)



### 多生产者与多消费者：操作值-假死

```java
public class ValueObject {
    public static String value = "";
}
```

```java
public class P {
    private String lock;

    public P(String lock) {
        super();
        this.lock = lock;
    }

    public void setValue() {
        try {
            synchronized (lock) {
                while (!ValueObject.value.equals("")) {
                    System.out.println("生产者 "
                            + Thread.currentThread().getName() + " WAITING了★");
                    lock.wait();
                }
                System.out.println("生产者 " + Thread.currentThread().getName()
                        + " RUNNABLE了");
                String value = System.currentTimeMillis() + "_"
                        + System.nanoTime();
                ValueObject.value = value;
                lock.notify();
            }

        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class C {
    private String lock;

    public C(String lock) {
        super();
        this.lock = lock;
    }

    public void getValue() {
        try {
            synchronized (lock) {
                while (ValueObject.value.equals("")) {
                    System.out.println("消费者 "
                            + Thread.currentThread().getName() + " WAITING了☆");
                    lock.wait();
                }
                System.out.println("消费者 " + Thread.currentThread().getName()
                        + " RUNNABLE了");
                ValueObject.value = "";
                lock.notify();
            }

        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadP extends Thread {
    private P p;

    public ThreadP(P p) {
        super();
        this.p = p;
    }

    @Override
    public void run() {
        while (true) {
            p.setValue();
        }
    }
}
```

```java
public class ThreadC extends Thread {
    private C r;

    public ThreadC(C r) {
        super();
        this.r = r;
    }

    @Override
    public void run() {
        while (true) {
            r.getValue();
        }
    }
}
```

```java
public class Run {
    public static void main(String[] args) throws InterruptedException {

        String lock = new String("");
        P p = new P(lock);
        C r = new C(lock);

        ThreadP[] pThread = new ThreadP[2];
        ThreadC[] rThread = new ThreadC[2];

        for (int i = 0; i < 2; i++) {
            pThread[i] = new ThreadP(p);
            pThread[i].setName("生产者" + (i + 1));

            rThread[i] = new ThreadC(r);
            rThread[i].setName("消费者" + (i + 1));

            pThread[i].start();
            rThread[i].start();
        }

        Thread.sleep(5000);
        Thread[] threadArray = new Thread[Thread.currentThread()
                .getThreadGroup().activeCount()];
        Thread.currentThread().getThreadGroup().enumerate(threadArray);

        for (int i = 0; i < threadArray.length; i++) {
            System.out.println(threadArray[i].getName() + " "
                    + threadArray[i].getState());
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_10-36-42.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_10-40-57.png)



### 通过管道进行线程间通信：字节流

```java
import java.io.IOException;
import java.io.PipedInputStream;

public class ReadData {
    public void readMethod(PipedInputStream input) {
        try {
            System.out.println("read  :");
            byte[] byteArray = new byte[20];
            int readLength = input.read(byteArray);
            while (readLength != -1) {
                String newData = new String(byteArray, 0, readLength);
                System.out.print(newData);
                readLength = input.read(byteArray);
            }
            System.out.println();
            input.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
import java.io.IOException;
import java.io.PipedOutputStream;

public class WriteData {
    public void writeMethod(PipedOutputStream out) {
        try {
            System.out.println("write :");
            for (int i = 0; i < 300; i++) {
                String outData = "" + (i + 1);
                out.write(outData.getBytes());
                System.out.print(outData);
            }
            System.out.println();
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
import java.io.PipedInputStream;

public class ThreadRead extends Thread {
    private ReadData read;
    private PipedInputStream input;

    public ThreadRead(ReadData read, PipedInputStream input) {
        super();
        this.read = read;
        this.input = input;
    }

    @Override
    public void run() {
        read.readMethod(input);
    }
}
```

```java
import java.io.PipedOutputStream;

public class ThreadWrite extends Thread {
    private WriteData write;
    private PipedOutputStream out;

    public ThreadWrite(WriteData write, PipedOutputStream out) {
        super();
        this.write = write;
        this.out = out;
    }

    @Override
    public void run() {
        write.writeMethod(out);
    }
}
```

```java
import java.io.IOException;
import java.io.PipedInputStream;
import java.io.PipedOutputStream;

public class Run {
    public static void main(String[] args) {

        try {
            WriteData writeData = new WriteData();
            ReadData readData = new ReadData();

            PipedInputStream inputStream = new PipedInputStream();
            PipedOutputStream outputStream = new PipedOutputStream();

            // inputStream.connect(outputStream);
            outputStream.connect(inputStream);

            ThreadRead threadRead = new ThreadRead(readData, inputStream);
            threadRead.start();

            Thread.sleep(2000);

            ThreadWrite threadWrite = new ThreadWrite(writeData, outputStream);
            threadWrite.start();

        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_10-57-19.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_11-00-28.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_11-00-40.png)



### 通过管道进行线程间通信：字符流

```java
public class ReadData {
    public void readMethod(PipedReader input) {
        try {
            System.out.println("read  :");
            char[] byteArray = new char[20];
            int readLength = input.read(byteArray);
            while (readLength != -1) {
                String newData = new String(byteArray, 0, readLength);
                System.out.print(newData);
                readLength = input.read(byteArray);
            }
            System.out.println();
            input.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class WriteData {
    public void writeMethod(PipedWriter out) {
        try {
            System.out.println("write :");
            for (int i = 0; i < 300; i++) {
                String outData = "" + (i + 1);
                out.write(outData);
                System.out.print(outData);
            }
            System.out.println();
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadRead extends Thread{
    private ReadData read;
    private PipedReader input;

    public ThreadRead(ReadData read, PipedReader input) {
        super();
        this.read = read;
        this.input = input;
    }

    @Override
    public void run() {
        read.readMethod(input);
    }
}
```

```java
public class ThreadWrite extends Thread{
    private WriteData write;
    private PipedWriter out;

    public ThreadWrite(WriteData write, PipedWriter out) {
        super();
        this.write = write;
        this.out = out;
    }

    @Override
    public void run() {
        write.writeMethod(out);
    }
}
```

```java
public class Run {
    public static void main(String[] args) {

        try {
            WriteData writeData = new WriteData();
            ReadData readData = new ReadData();

            PipedReader inputStream = new PipedReader();
            PipedWriter outputStream = new PipedWriter();

            // inputStream.connect(outputStream);
            outputStream.connect(inputStream);

            ThreadRead threadRead = new ThreadRead(readData, inputStream);
            threadRead.start();

            Thread.sleep(2000);

            ThreadWrite threadWrite = new ThreadWrite(writeData, outputStream);
            threadWrite.start();

        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_11-25-59.png)



方法`join`的作用是使所属的线程z进行无限期的阻塞，等待线程x销毁后再继续执行z后面的代码。

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_18-30-23.png)

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        try {
            int secondValue = (int) (Math.random() * 10000);
            System.out.println(secondValue);
            Thread.sleep(secondValue);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        try {
            MyThread threadTest = new MyThread();
            threadTest.start();
            threadTest.join();

            System.out.println("我想当threadTest对象执行完毕后我再执行，我做到了");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_18-35-14.png)



### 方法join(long)与sleep(long)的区别

```java
public final synchronized void join(long millis)
    throws InterruptedException {
        long base = System.currentTimeMillis();
        long now = 0;

        if (millis < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (millis == 0) {
            while (isAlive()) {
                wait(0);
            }
        } else {
            while (isAlive()) {
                long delay = millis - now;
                if (delay <= 0) {
                    break;
                }
                wait(delay);
                now = System.currentTimeMillis() - base;
            }
        }
    }
```

从join(long)的源代码中可以了解到，当执行wait(long)方法后，当前线程的锁被释放，那么其他线程就可以调用此线程中的同步方法了。而`Thread.sleep(long)`方法却不释放锁。



### ReentrantLock实现同步

```java
public class MyService {
    private Lock lock = new ReentrantLock();

    public void testMethod() {
        lock.lock();
        for (int i = 0; i < 5; i++) {
            System.out.println("ThreadName=" + Thread.currentThread().getName()
                    + (" " + (i + 1)));
        }
        lock.unlock();
    }
}
```

```java
public class MyThread extends Thread {
    private MyService service;

    public MyThread(MyService service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.testMethod();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {

        MyService service = new MyService();

        MyThread a1 = new MyThread(service);
        MyThread a2 = new MyThread(service);
        MyThread a3 = new MyThread(service);
        MyThread a4 = new MyThread(service);
        MyThread a5 = new MyThread(service);

        a1.start();
        a2.start();
        a3.start();
        a4.start();
        a5.start();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_20-30-46.png)

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_20-32-27.png)



### 使用Condition实现等待/通知

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class MyService {
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void await() {
        try {
            condition.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private MyService service;

    public ThreadA(MyService service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.await();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {

        MyService service = new MyService();

        ThreadA a = new ThreadA(service);
        a.start();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_21-17-36.png)

报错的异常欣喜是监视器出错，解决的方法是必须在`condition.await()`方法调用之前调用`lock.lock()`代码获得同步监视器。

如果想单独唤醒部分线程该怎么处理呢？这时就有必要使用多个Condition对象了，也就是Condition对象可以唤醒部分指定线程，有助于提升程序运行的效率。可以先对线程进行分组，然后再唤醒指定组中的线程。

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class MyService {
    private Lock lock = new ReentrantLock();
    public Condition conditionA = lock.newCondition();
    public Condition conditionB = lock.newCondition();

    public void awaitA() {
        try {
            lock.lock();
            System.out.println("begin awaitA时间为" + System.currentTimeMillis()
                    + " ThreadName=" + Thread.currentThread().getName());
            conditionA.await();
            System.out.println("  end awaitA时间为" + System.currentTimeMillis()
                    + " ThreadName=" + Thread.currentThread().getName());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void awaitB() {
        try {
            lock.lock();
            System.out.println("begin awaitB时间为" + System.currentTimeMillis()
                    + " ThreadName=" + Thread.currentThread().getName());
            conditionB.await();
            System.out.println("  end awaitB时间为" + System.currentTimeMillis()
                    + " ThreadName=" + Thread.currentThread().getName());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void signalAll_A() {
        try {
            lock.lock();
            System.out.println("  signalAll_A时间为" + System.currentTimeMillis()
                    + " ThreadName=" + Thread.currentThread().getName());
            conditionA.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public void signalAll_B() {
        try {
            lock.lock();
            System.out.println("  signalAll_B时间为" + System.currentTimeMillis()
                    + " ThreadName=" + Thread.currentThread().getName());
            conditionB.signalAll();
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public class ThreadA extends Thread{
    private MyService service;

    public ThreadA(MyService service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.awaitA();
    }
}
```

```java
public class ThreadB extends Thread {
    private MyService service;

    public ThreadB(MyService service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.awaitB();
    }
}
```

```java
public class Run {
    public static void main(String[] args) throws InterruptedException {

        MyService service = new MyService();

        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();

        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();

        Thread.sleep(3000);

        service.signalAll_A();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-12_21-30-21.png)



### 公平锁与非公平锁

```java
import java.util.concurrent.locks.ReentrantLock;

public class Service {
    private ReentrantLock lock;

    public Service(boolean isFair) {
        super();
        lock = new ReentrantLock(isFair);
    }

    public void serviceMethod() {
        try {
            lock.lock();
            System.out.println("ThreadName=" + Thread.currentThread().getName()
                    + "获得锁定");
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public class Run {
    public static void main(String[] args) throws InterruptedException {
        final Service service = new Service(true);

        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("★线程" + Thread.currentThread().getName()
                        + "运行了");
                service.serviceMethod();
            }
        };

        Thread[] threadArray = new Thread[10];
        for (int i = 0; i < 10; i++) {
            threadArray[i] = new Thread(runnable);
        }
        for (int i = 0; i < 10; i++) {
            threadArray[i].start();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_09-10-24.png)

```java
public class Run {
    public static void main(String[] args) throws InterruptedException {
        final Service service = new Service(false);

        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("★线程" + Thread.currentThread().getName()
                        + "运行了");
                service.serviceMethod();
            }
        };

        Thread[] threadArray = new Thread[10];
        for (int i = 0; i < 10; i++) {
            threadArray[i] = new Thread(runnable);
        }
        for (int i = 0; i < 10; i++) {
            threadArray[i].start();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_09-16-16.png)



```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class MyService {
    public ReentrantLock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void waitMethod(){
        try {
            lock.lock();
            System.out
                    .println("lock begin " + Thread.currentThread().getName());
            for (int i = 0; i < Integer.MAX_VALUE / 10; i++) {
                String newString = new String();
                Math.random();
            }
            System.out
                    .println("lock   end " + Thread.currentThread().getName());
        } finally {
            if (lock.isHeldByCurrentThread()) {
                lock.unlock();
            }
        }
    }
}
```

```java
public class Run {
    public static void main(String[] args) throws InterruptedException {
        final MyService service = new MyService();
        Runnable runnableRef = new Runnable() {
            @Override
            public void run() {
                service.waitMethod();
            }
        };

        Thread threadA = new Thread(runnableRef);
        threadA.setName("A");
        threadA.start();
        Thread.sleep(500);
        Thread threadB = new Thread(runnableRef);
        threadB.setName("B");
        threadB.start();
        threadB.interrupt();// 打标记
        System.out.println("main end!");
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_11-14-22.png)

将类`MyService.java`中原有代码`lock.lock()`变成`lock.lockInterruptibly()`。

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class MyService {
    public ReentrantLock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void waitMethod(){
        try {
            lock.lockInterruptibly();
            System.out
                    .println("lock begin " + Thread.currentThread().getName());
            for (int i = 0; i < Integer.MAX_VALUE / 10; i++) {
                String newString = new String();
                Math.random();
            }
            System.out
                    .println("lock   end " + Thread.currentThread().getName());
        } catch (InterruptedException e) {
            System.out.println("线程"+Thread.currentThread().getName()+"进入catch~!");
            e.printStackTrace();
        } finally {
            if (lock.isHeldByCurrentThread()) {
                lock.unlock();
            }
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_11-02-30.png)



### 使用Condition实现顺序执行

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class Run {
    volatile private static int nextPrintWho = 1;
    private static ReentrantLock lock = new ReentrantLock();
    final private static Condition conditionA = lock.newCondition();
    final private static Condition conditionB = lock.newCondition();
    final private static Condition conditionC = lock.newCondition();

    public static void main(String[] args) {

        Thread threadA = new Thread() {
            public void run() {
                try {
                    lock.lock();
                    while (nextPrintWho != 1) {
                        conditionA.await();
                    }
                    for (int i = 0; i < 3; i++) {
                        System.out.println("ThreadA " + (i + 1));
                    }
                    nextPrintWho = 2;
                    conditionB.signalAll();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            }
        };

        Thread threadB = new Thread() {
            public void run() {
                try {
                    lock.lock();
                    while (nextPrintWho != 2) {
                        conditionB.await();
                    }
                    for (int i = 0; i < 3; i++) {
                        System.out.println("ThreadB " + (i + 1));
                    }
                    nextPrintWho = 3;
                    conditionC.signalAll();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            }
        };

        Thread threadC = new Thread() {
            public void run() {
                try {
                    lock.lock();
                    while (nextPrintWho != 3) {
                        conditionC.await();
                    }
                    for (int i = 0; i < 3; i++) {
                        System.out.println("ThreadC " + (i + 1));
                    }
                    nextPrintWho = 1;
                    conditionA.signalAll();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            }
        };
        Thread[] aArray = new Thread[5];
        Thread[] bArray = new Thread[5];
        Thread[] cArray = new Thread[5];

        for (int i = 0; i < 5; i++) {
            aArray[i] = new Thread(threadA);
            bArray[i] = new Thread(threadB);
            cArray[i] = new Thread(threadC);

            aArray[i].start();
            bArray[i].start();
            cArray[i].start();
        }
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_11-48-12.png)

### 类ReentrantReadWriteLock的使用：读读共享

```java
public class Service {
    private ReentrantReadWriteLock lock = new ReentrantReadWriteLock();

    public void read() {
        try {
            try {
                lock.readLock().lock();
                System.out.println("获得读锁" + Thread.currentThread().getName()
                        + " " + System.currentTimeMillis());
                Thread.sleep(10000);
            } finally {
                lock.readLock().unlock();
            }
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private Service service;

    public ThreadA(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.read();
    }
}
```

```java
public class ThreadB extends Thread{
    private Service service;

    public ThreadB(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.read();
    }
}
```

```java
public class Run {
    public static void main(String[] args) {
        Service service = new Service();
        ThreadA a = new ThreadA(service);
        a.setName("A");
        ThreadB b = new ThreadB(service);
        b.setName("B");
        a.start();
        b.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_12-00-04.png)

### 写写互斥

更改代码如下:

```java
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Service {
    private ReentrantReadWriteLock lock = new ReentrantReadWriteLock();

    public void write() {
        try {
            try {
                lock.writeLock().lock();
                System.out.println("获得写锁" + Thread.currentThread().getName()
                        + " " + System.currentTimeMillis());
                Thread.sleep(10000);
            } finally {
                lock.writeLock().unlock();
            }
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private Service service;

    public ThreadA(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.write();
    }
}
```

```java
public class ThreadB extends Thread{
    private Service service;

    public ThreadB(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.write();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_13-40-10.png)



### 读写互斥

```java
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Service {
    private ReentrantReadWriteLock lock = new ReentrantReadWriteLock();

    public void read() {
        try {
            try {
                lock.readLock().lock();
                System.out.println("获得读锁" + Thread.currentThread().getName()
                        + " " + System.currentTimeMillis());
                Thread.sleep(10000);
            } finally {
                lock.readLock().unlock();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void write() {
        try {
            try {
                lock.writeLock().lock();
                System.out.println("获得写锁" + Thread.currentThread().getName()
                        + " " + System.currentTimeMillis());
                Thread.sleep(10000);
            } finally {
                lock.writeLock().unlock();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread {
    private Service service;

    public ThreadA(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.read();
    }
}
```

```java
public class ThreadB extends Thread{
    private Service service;

    public ThreadB(Service service) {
        super();
        this.service = service;
    }

    @Override
    public void run() {
        service.write();
    }
}
```

```java
public class Run {
    public static void main(String[] args) throws InterruptedException {

        Service service = new Service();

        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();

        Thread.sleep(1000);

        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();

    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_13-43-11.png)

### 写读互斥

更改类`Run.java`的代码:

```
public class Run {
    public static void main(String[] args) throws InterruptedException {

        Service service = new Service();

        ThreadB b = new ThreadB(service);
        b.setName("B");
        b.start();

        Thread.sleep(1000);

        ThreadA a = new ThreadA(service);
        a.setName("A");
        a.start();
    }
}
```

![](/assets/img/posts/java_multithread_notes/Snipaste_2018-09-13_13-48-01.png)

