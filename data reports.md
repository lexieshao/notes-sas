
# 06 Program 制定简单报告--------------------
 file ''print; put;  
   @n指定移动到第n列/  
    +n，指定移动n列  
    /跳动到下一行 #n，跳动到第n行
    @hold住当前行*/
* 报告每个同学的销售情况
data _null_ 告诉sas不写数据集名，使得程序更快，file创建输出文件  
title；去掉所有自动标题  
put _page 在每个学生报告下面插上页码;
    'DATA _NULL_;
    INFILE 'C:\lizzieshao\lsascode\Candy.dat';
    INPUT Name $ 1-11 Class @15 DateReturned MMDDYY10. 
         CandyType $ Quantity;
   Profit = Quantity * 1.25;
   FILE 'C:\lizzieshao\lsascode\Student.txt' PRINT;
   TITLE;
   PUT @5 'Candy sales report for ' Name 'from classroom ' Class
     // @5 'Congratulations!  You sold ' Quantity 'boxes of candy'
     / @5 'and earned ' Profit DOLLAR6.2 ' for our field trip.';
   PUT _PAGE_;
    RUN;'

/*--------------------12 Program,简单输出 report------------------
包含print、means、tabulate、sort的所有功能*/

DATA natparks;
   INFILE 'C:\lizzieshao\lsascode\Parks.dat';
   INPUT Name $ 1-21 Type $ Region $ Museums Camping;
RUN;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   TITLE 'Report with Character and Numeric Variables';
RUN;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Museums Camping;
   TITLE 'Report with Only Numeric Variables';
RUN;

* define为单个变量指定一些选项 define var/option ‘列名’
usage选项：across 为变量的每一个值都创建一列，
analysis：为变量创建统计量，数值变量是默认的，默认为sum
display：为数据集中的每一个观测值创建一行，字符串变量该选项是默认的
group：为每个变量的值都穿啊关键一行
order：为每个观测值都创建一行，并且排列是依据指定变量顺序
PROC REPORT with ORDER variable, MISSING option, and column header;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE MISSING;
   COLUMN Region Name Museums Camping;
   DEFINE Region / ORDER;
   DEFINE Camping / ANALYSIS 'Camp/Grounds';
   TITLE 'National Parks and Monuments Arranged by Region';
RUN;

/*----------------13  Program ，report创建简易报告-----------------
group：简易行
across：简易列*/
DATA natparks;
   INFILE 'C:\lizzieshao\lsascode\Parks.dat';
   INPUT Name $ 1-21 Type $ Region $ Museums Camping;
RUN;

* Region and Type as GROUP variables;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Region Type Museums Camping;
   DEFINE Region / GROUP;
   DEFINE Type / GROUP;
   TITLE 'Summary Report with Two Group Variables';
RUN;

* Region as GROUP and Type as ACROSS with sums
增加了逗号，变量交叉，（）tell which var to sum;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Region Type,(Museums Camping);
   DEFINE Region / GROUP;
   DEFINE Type / ACROSS;
   TITLE 'Summary Report with a Group and an Across Variable';
RUN;

* PROC REPORT with breaks，为报告增加停顿
break location var/options，为指定变量的每一个值产生停顿，这个变量必须是group或order变量，并且在define定义过
rbreak location.option，无需指定，只产生一个丁顿，开始或结尾
location:before、after
option：插入哪种停顿，OL 停顿的地方加入横线
page 开始一个新的页面 skip 插入一个空行 summarize 插入数值变量之和 ul 在停顿下面划线;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Name Region Museums Camping;
   DEFINE Region / ORDER;
   BREAK AFTER Region / SUMMARIZE OL SKIP;
   RBREAK AFTER / SUMMARIZE OL SKIP;
   TITLE 'Detail Report with Summary Breaks';
RUN;

*增加统计量，
max min mean median n nmiss p90 pctn pctsum std sum
变量和统计量之间加入逗号，n不需要，多个变量或多个统计量加括号
Statistics in COLUMN statement with two group variables;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Region Type N (Museums Camping),MEAN;
   DEFINE Region / GROUP;
   DEFINE Type / GROUP;
   TITLE 'Statistics with Two Group Variables';
RUN;

*Statistics in COLUMN statement with group and across variables;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Region N Type,(Museums Camping),MEAN;
   DEFINE Region / GROUP;
   DEFINE Type / ACROSS;
   TITLE 'Statistics with a Group and Across Variable';
RUN;
* 加入计算变量to report with compute block
define new-var/computed
compute new-var/option
  programming statements
  endcomp
COMPUTE new variables that are numeric and character;
PROC REPORT DATA = natparks NOWINDOWS HEADLINE;
   COLUMN Name Region Museums Camping Facilities Note;
   DEFINE Museums / ANALYSIS SUM NOPRINT;
   DEFINE Camping / ANALYSIS SUM NOPRINT;
   DEFINE Facilities / COMPUTED 'Camping/and/Museums';
   DEFINE Note / COMPUTED;
   COMPUTE Facilities;
      Facilities = Museums.SUM + Camping.SUM;
   ENDCOMP;
   COMPUTE Note / CHAR LENGTH = 10;
      IF Camping.SUM = 0 THEN Note = 'No Camping';
   ENDCOMP;
   TITLE 'Report with Two Computed Variables'; 
RUN;
