# 信号与槽

1. 信号，类似于普通函数，只需声明，无需实现

2. 槽函数，qt5中的任意成员函数，静态函数，全局函数，lambda表达式

3. 可以使用qt 内部定义好的，也可以自定义

4. 没有返回值，但是有参数

5. 信号和槽的参数列表顺序是一致的

6. 信号的参数数目可以大于槽函数的，反过来不行

   ```c++
   void mysignal();
   emit myslot();
   
   void mysgnal();
   void myslot();
   ```

   lamada函数，匿名槽函数

    ```c++
   void fun()
   {
       
   }
   [=]()mutable 
   {
       
   }
   
   // = or &
   值传递或者引用传递
   this 
       lamade表达式所在类的所有成员
       
    ```

   编辑框相关函数：

   ```c++
   text();											//获取编辑框内容
   setText(const QString &)						//设置编辑框内容
   ui->lineEdit->setTextMargins(15,0,0,0);			//设置显示间距
   ui->lineEdit->setEchoMode(QLineEdit::Password);	//设置显示方式
   
   //设置显示提示
   # include<Qcompleter>
   # include<QStringList>
   QString list;
   list<<"hello"<<"holy"<<"high";
   Qcompleter *com =new Qcompleter(list,this);
   com->setCaseSensitivity(Qt::CaseInsensitive);	//不区分大小写 	
   ui->lineEdit->setCompleter(com);
   ```

   文本编辑框相关内容：

   1. > text edit 		//可以显示文字以外的其他东西
      >
      > plain text edit 	//只能显示文字
      >
      >

   ```c++
   
   ```

   label控件的使用

   > 1. 显示文字
   >
   > ```c++
   > ui->labelText->setText("jianghuiwen");
   > ```
   >
   > 2. 显示图片
   >
   > ```c++
   > ui->labelImage->setPixmap(QPixmap("资源文件的路径"));
   > ui->labelImage->setScaledContents(true);		//让图片自动适应label的大小
   > 
   > ```
   >
   > 3. 显示gif动画
   >
   > ```c++
   > #include<QMovie>
   > ......
   > QMovie *mymovie = new QMovie("gif文件的路径");
   > ui->labelImage->setMovie(mymovie);
   > mymovie->start();
   > ui->labeLgif->setScaledContents()
   > ```
   >
   > 4. 显示网址(可以显示HTML格式的字符串)
   >
   > ```c++
   > QLabel *label = new QLabel(this);
   > ui->labelurl ->setText("jianghuiwen");
   > ui->labelurl ->setText("
   >                 <h1><a href=\"https://baidu.com\">百度一下</a></h1>;
   >                 ")
   > label->setOpenExternalLinks(true);  
   > ```

Qt布局管理器

1. 两种组建定位机制

   > 1. 绝对定位
   > 2. 布局定位
   >
   > (1) 水平布局
   >
   > (2) 垂直布局
   >
   > (3) 网格布局


# 第二次学习：

1. ```c++
   #include <QObject>
    ////////// newspaper.h //////////
   class Newspaper : public QObject
   {
       Q_OBJECT
   public:
       Newspaper(const QString & name) :
           m_name(name)
       {
       }
    
       void send()	//发送信号的函数
       {
           emit newPaper(m_name);
       }
    
   signals:
       void newPaper(const QString &name);	//信号函数
    
   private:
       QString m_name;
   };
    
   ////////// reader.h //////////
   #include <QObject>
   #include <QDebug>
    
   class Reader : public QObject
   {
       Q_OBJECT
   public:
       Reader() {}
    
       void receiveNewspaper(const QString & name)
       {
           qDebug() << "Receives Newspaper: " << name;
       }
   };
    
   ////////// main.cpp //////////
   #include <QCoreApplication>
    
   #include "newspaper.h"
   #include "reader.h"
    
   int main(int argc, char *argv[])
   {
       QCoreApplication app(argc, argv);
    
       Newspaper newspaper("Newspaper A");
       Reader reader;
       QObject::connect(&newspaper, &Newspaper::newPaper,
                        &reader,    &Reader::receiveNewspaper);
       newspaper.send();
    
       return app.exec();
   }
   ```

知识点:

> 1. 任何成员函数、static 函数、全局函数和 Lambda 表达式都可以作为槽函数
> 2. 一个信号可以和多个槽连接,但是触发的顺序是不确定的
> 3. 多个信号可以连接到一个槽,只要任意发出一个信号,这个槽就可以被调用
> 4. 一个信号可以连接到另一个信号,用法跟信号与槽的方式一样
> 5. 槽可以被取消连接,当一个对象被delete之后,Qt自动取消所有连接到该对象上的所有槽
> 6. lambda表达式
>
> [](){}mutuable或exception->返回值{函数体}
>
> `mutuable`:按值传递函数对象参数时，加上mutable修饰符后，可以修改按值传递进来的拷贝（注意是能修改拷贝，而不是值本身）。
>
> `exception`:exception声明用于指定函数抛出的异常，如抛出整数类型的异常，可以使用throw(int)
>
> 7. 只有继承了`QObject`类的类，才具有信号槽的能力。所以，为了使用信号槽，必须继承`QObject`。凡是`QObject`类（不管是直接子类还是间接子类），都应该在第一行代码写上`Q_OBJECT`。

