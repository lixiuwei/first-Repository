first-Repository
================
#include<stdio.h>

#include<malloc.h>

#define Size 3//停车场的容量

#define Price 0.1//每分钟的价钱

#define Null 0


typedef struct time//定义时间

{

int hour;//小时

int min;//分钟

}Time;//时间结点



typedef struct 

{

int car_number;//车牌号

int number;

Time arrivetime,leavetime;//到达时间，离开时间

int fee;//所需费用

}car_info;//车辆的信息结点



typedef struct

{

car_info *north;//车场北

car_info *south;//车场南

int number;//停车数量

int car_number;

}car_park;//停车场



typedef struct

{

car_info *west;//车场西

car_info *east;//车场东

int number;//停车数量


}car_park_back;//倒车道



typedef struct car//链队列的定义

{

car_info data;//数据域

struct car *next; //指针域 

}carnode;

typedef struct node

{

carnode *head;//头结点

carnode *rear;//尾结点

int number;//停车数量

}car_park_temp;//便道



void init_car_park(car_park *cp)//停车场初始化

{

cp->north=(car_info *)malloc(Size * sizeof(car_info));//申请空间

if(!cp->north) printf("error\n");//申请空间失败

cp->south=cp->north;

cp->number=0;//构造一个空栈

}



void enter_car_park(car_park *cp,car_info *car)//进入停车场

{

*cp->south++=*car;

cp->number++; //修改当前栈顶指针

}



int notfull_car_park(car_park *cp)

//判断停车场是否有空位

{

int e;

if(cp->south-cp->north>=Size) //从南到北的车辆数为5，则停车场已满

e=0; //停车场满返回0

else

e=1; //停车场未满返回1

return(e);

}

int notempty_car_park_back(car_park_back *cpb)

//判断倒车道是否还有车

{

int e;

if(cpb->east==cpb->west)//倒车道为空

e=0; //返回0

else

e=1; //倒车道有车返回1

return(e);

}



void back_car_park(car_park *cp,car_info *car)

//从停车场倒车

{

*car=*cp->south;

cp->number--; 

}



void init_car_park_back(car_park_back *cpb)//初始化倒车道

{

cpb->west=(car_info *)malloc(Size *sizeof(car_info));//申请空间

if(!cpb->west) printf("error\n");//申请空间失败

cpb->east=cpb->west;

cpb->number=0;//构造一个空栈

}



void enter_car_park_back(car_park_back *cpb,car_info *car)//进入倒车道

{

*cpb->east++=*car;

cpb->number++;//修改当前的栈顶指针

}



void leave_car_park_back(car_park_back *cpb,car_info *car)//离开倒车道

{

*car=*--cpb->east;

cpb->number--;

}



void init_car_park_temp(car_park_temp *cpt)//初始化便道

//将cpt初始化为一个空的链队列

{

cpt->head=cpt->rear=(carnode *)malloc(sizeof(carnode));//申请空间

cpt->head->next=Null;

cpt->number=0; 

}



void enter_car_park_temp(car_park_temp *cpt,car_info *car)

//进入便道

//将数据元素car插入到队列cpt中

{

carnode *p;

p=(carnode *)malloc(sizeof(carnode));//申请结点p

p->data=*car;

p->next=Null;

cpt->head->next=p;

cpt->rear=p;

cpt->number++;

}



void leave_car_park_temp(car_park_temp *cpt,car_info *car,car_park *cp)

//离开便道，将队列cpt的队头元素出队

{ 

carnode *p;

p=cpt->head->next;

*car=p->data;

cpt->head=p->next; //队头元素p出队 

enter_car_park(cp, car);//调用进入停车场的函数

cpt->number--; //便道的车辆少1

}



int notempty_car_park_temp(car_park_temp *cpt)

//判断便道是否还有车

{

int e;

if(cpt->head==cpt->rear)

e=0; //便道为空则返回0

else

e=1; //便道不空则返回1

return(e);

}





void leave_car_park(car_park *cp,car_info *car,car_park_back *cpb)

//某车离开停车场，并计算该车的费用

{

int e, a1,a2,b1,b2,t;

car_info *car1,*car2,*car3;//三辆车的信息

car1=(car_info *)malloc(sizeof(car_info));//为第一辆车申请空间

car2=(car_info *)malloc(sizeof(car_info));//为第二辆车申请空间

car3=(car_info *)malloc(sizeof(car_info));//为第三辆车申请空间


while((--cp->south)->car_number!=car->car_number)//从停车场倒车

{

back_car_park(cp,car1);//出停车场

enter_car_park_back(cpb,car1);//进入倒车道

}



car->arrivetime.hour=cp->south->arrivetime.hour;car->arrivetime.min=cp->south->arrivetime.min;

a1=car->arrivetime.hour;

a2=car->arrivetime.min;

b1=car->leavetime.hour;

b2=car->leavetime.min;

t=(b1-a1)*60+(b2-a2);//计算车辆在停车场停留的时间

car->fee=t*Price;//计算车辆应交的费用



printf("\n车辆停留的时间是: %3d min\n",t);//输出车辆停留的时间

printf("\n应交的费用是 %3d 元\n",car->fee);//输出车辆应交的费用

e=notempty_car_park_back(cpb);//判断车道是否为空

while(e==1)

{

leave_car_park_back(cpb,car2);

enter_car_park(cp,car2);

e=notempty_car_park_back(cpb);

}//离开倒车道，进入停车场

cp->number--;

}



void List1(car_park *cp) /*列表显示车场信息*/ 

{int a;

int b;

int i;

if(cp->number>0) /*判断车站内是否有车*/ 

{ 

for( i=0;i<cp->number;i++)

{

cp->south--;

a=cp->south->car_number;

b=cp->number-i;

printf("\n位置%d",b);

printf("\n车牌号%d",a); 

}

}

else 

printf("\n车场里没有车");

} 



void List2(car_park_temp *cpt) /*列表显示便道信息*/ 

{ int a;

int b;

if(cpt->number>0)

{

a=cpt->number;

b=cpt->head->data.car_number;

printf("\n便道里有%d辆车",a); 

printf("\n车牌号为:%d",b);

}

else 

printf("\n便道里没有车");

} 




void main()

{

char ch;

int e,n;

//QueueNode *p;

car_park_back *cpb; 

car_park *cp; 

car_park_temp *cpt; 

car_info *car; 

cp=(car_park *)malloc(sizeof(car_park));

cpb=(car_park_back *)malloc(sizeof(car_park));

cpt=(car_park_temp *)malloc(sizeof(car_park_temp));

init_car_park(cp); //初始化停车场

init_car_park_back(cpb);//初始化倒车道

init_car_park_temp(cpt);//初始化便道


do

{

car=(car_info *)malloc(sizeof(car_info));

printf("\n\n************ 模拟停车场管理问题***************\n\n"); 

printf("请选择你的操作 \n'A' 到达\n'L'离开\n'C'查看停车场信息 \n'D'查看便道信息\n'0'退出:\n");

scanf("%s",&ch);

e=notfull_car_park(cp);

switch(ch)

{

case'A':if(e==1)//停车场未满

{

printf("请输入车牌号:\n");

scanf("%d",&car->car_number);

printf("请输入到达时间，hour min:\n");

scanf("%d %d",&(*car).arrivetime.hour,&(*car).arrivetime.min);

enter_car_park(cp,car);//进入停车场

printf("此车在停车场中,位置在: %d\n",cp->number);

}

else//停车场已满


{

car_info *car;

car=(car_info*)malloc(sizeof(car_info));

printf("请输入车牌号:\n");

scanf("%d",&car->car_number);//进入便道

enter_car_park_temp(cpt,car);

printf("停车场已满,车在便道中,位置是: %d\n",cpt->number);

}

break;

case 'L':

printf("请输入车牌号:\n");

scanf("%d",&car->car_number);

printf("请输入离开时间hour min:\n"); 

scanf("%d %d",&(*car).leavetime.hour,&(*car).leavetime.min);

leave_car_park(cp,car,cpb);//离开停车场

n=notempty_car_park_temp(cpt);//判断便道车辆是否为空

if(n==1)//便道上有车辆

leave_car_park_temp(cpt,car,cp);//离开便道

break;

case'C': List1(cp); break;

case'D': List2(cpt); break;

default:break;

}

}while(ch!='0');//退出系统

}

