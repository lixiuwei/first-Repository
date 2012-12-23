first-Repository
================

import java.io.*;
public class Method
{
	private int intinput() throws 

IOException,NumberFormatException
	{
		BufferedReader br=new BufferedReader(new 

InputStreamReader(System.in));
		String str=br.readLine();
		int i=Integer.parseInt(str);
		return i;
	}
	private String stringinput() throws IOException
	{
		BufferedReader br=new BufferedReader(new 

InputStreamReader(System.in));
		String str=br.readLine();
		return str;
	}
	private boolean between(int n,int min,int max)
	{
		for(int i=min;i<=max;i++)
			if(n==i)
				return true;
		return false;
	}
	private void printerror()
	{
		System.out.println("输入错误！请重新输入！");
	}
	String sip()
	{
		String str;
		while(true)
		{
			try
			{
				str=this.stringinput();
			}
			catch(IOException e)
			{
				this.printerror();
				continue;
			}
			break;
		}
		return str;
	}
	int iip(int min,int max)
	{
		int n=0;
		while(true)
		{
			try
			{
				n=this.intinput();
			}
			catch(IOException e)
	    	{
	    		this.printerror();
	    		continue;
	    	}
	    	catch(NumberFormatException e)
	    	{
	    		this.printerror();
	    		continue;
	    	}
	    	if(!this.between(n,min,max))
	    	{
	    		this.printerror();
	    		continue;
	    	}
	    	break;
		}
		return n;
	}
}
public class Car     
{
	String car_no;
	String state;
	Car()
	{
		car_no=null;
		state=null;
	}
}
public class Stop 
{
	Car data[];
	int size;
	Stop()
	{
		data=new Car[1];
		size=0;
	}
	private Car peek()
	{
		return data[size-1];
	}
	private boolean isFull()
	{
		return size==data.length;
	}
	private Car pop()
	{
		size--;
		return data[size];
	}
	void pop(int location,Passway p,Temp t)  
	{
		if(location==this.size)
		{
			System.out.println(this.peek().car_no+"号

车离开停车场");
			this.pop();
		}
		else
		{
			int f=this.size;
			for(int i=1;i<=f-location;i++)
			{
				t.push(this.peek());
				this.pop();
			}
			System.out.println(this.peek().car_no+"号

车离开停车场");
			this.pop();
			for(int i=0;!t.isEmpty();i++)
			{
				this.push(t.peek());
				t.pop();
			}
		}
		if(!p.isEmpty()) 
		{
			this.push(p.data[p.front]);
			p.remove();
		}
	}
	void push(Car target)
	{
		if(this.isFull())
			stretch();
		data[size]=target;
		size++;
		target.state=target.car_no+"号车位于停车场"+size

+"号位";
		System.out.println(target.car_no+"号车进入停车

场"+size+"号位");
	}
	boolean isIn(String s)
	{
		if(size==0)
			return false;
		for(int i=0;i<size;i++)
			if(data[i].car_no.equals(s))
				return true;
		return false;
	}
	private void stretch()
	{
		Car[] newData=new Car[data.length*2];
		for(int i=0;i<data.length;i++)
			newData[i]=data[i];
		data=newData;
	}
	void print()
	{
		for(int i=0;i<size;i++)
			System.out.println(data

[i].state.toString());
	}
}
public class Passway 
{
	Car [] data;
	int size;
	int front;
	Passway()
	{
		data=new Car[1];
		size=0;
		front=0;
	}
	boolean isEmpty()
	{
		return size==0;
	}
	private boolean isFull()
	{
		return size==data.length;
	}
	public Car remove()
	{
		Car result=data[front];
		front=(front+1)%data.length;
		size--;
		Passway t=new Passway();  
		for(int i=0;i<this.size;i++)
			t.add(this.data[front+i]);
		return result;
	}
	void add(Car target)
	{
		if(this.isFull())
			this.stretch();
		data[(front+size)%data.length]=target;
		size++;
		target.state=target.car_no+"号车位于便道"+size+"

号位";
		System.out.println(target.car_no+"号车进入便

道"+size+"号位");
	}
	boolean isIn(String s) 
	{
		if(size==0)
			return false;
		for(int i=0;i<size;i++)
			if(this.data[(front

+i)%data.length].car_no.equals(s))
				return true;
		return false;
	}
	private void stretch()
	{
		Car newData[]=new Car[data.length*2];
		for(int i=0;i<data.length;i++)
			newData[i]=data[(front+i)%data.length];
		data=newData;
		front=0;
	}
	void print()
	{
		for(int i=0;i<size;i++)
			System.out.println(data[(front

+i)%data.length].state.toString());
	}
}
public class Temp 
{
	private Car[] data;
	private int size;
	Temp()
	{
		data=new Car[1];
		size=0;
	}
	boolean isEmpty()
	{
		return size==0;
	}
	Car peek()
	{
		return data[size-1];
	}
	private boolean isFull()
	{
		return size==data.length;
	}
	Car pop()
	{
		size--;
		return data[size];
	}
	void push(Car target)
	{
		if(this.isFull())
			stretch();
		data[size]=target;
		size++;
		System.out.println(target.car_no+"号车暂时离开停

车场");
		target.state=target.car_no+"号车暂时离开停车场";
	}
	private void stretch()
	{
		Car[] newData=new Car[data.length*2];
		for(int i=0;i<data.length;i++)
			newData[i]=data[i];
		data=newData;
	}
}
public class cms
{
	public static void main(String args[])
	{
		Stop stop=new Stop();
		Passway passway=new Passway();
		Temp temp=new Temp();
		Method method=new Method();
		System.out.println("欢迎使用停车场管理系统！");
		while(true)
		{
			System.out.println("请选择操作");
			System.out.println("1: 初始化");
			System.out.println("2：进车");
			System.out.println("3：出车");
			System.out.println("4：查询");
			System.out.println("5：退出");
			int select=method.iip(1,5);
			switch(select)
			{
			case 1:int i;
			       Stop newstop=new Stop();
			       Passway newpassway=new Passway();
			       stop=newstop;
			       passway=newpassway;
			       for(i=1;i<=5;i++)
			       {
			    	   System.out.println("请输入停车

位"+i+"号车位汽车的编号，键入$完结");
			    	   String str=null;
			    	   while(true)
			    	   {
			    		   str=method.sip();
			    		   if(stop.isIn(str))
			    		   {
			    			   

System.out.println("此车已在停车场里，请重新输入！");
				  		 	   

continue;
			    		   }
			    		   if(passway.isIn(str))
			    		   {
			    			   

System.out.println("此车已在便道里，请重新输入！");
				  	 		   

continue;
				  		   }
			  			   break;
			    	   }
			    	   if(str.equals("$"))
			    		   break;
			    	   else
			    	   {
			    		   Car c=new Car();
			    		   c.car_no=str;
			    		   stop.push(c);
			    	   }
			       }
			       if(stop.size==5)
			    	   for(int n=1;;n++)
		    		   {
		    			   System.out.println("请

输入便道"+n+"号位汽车的编号，键入$完结");
		    			   String str01=null;
		    			   while(true)
				    	   {
				    		   

str01=method.sip();
				    		   if(stop.isIn

(str01))
				    		   {
				    			   

System.out.println("此车已在停车场里，请重新输入！");
					  		 	  

 continue;
				    		   }
				    		   if

(passway.isIn(str01))
				    		   {
				    			   

System.out.println("此车已在便道里，请重新输入！");
					  	 		  

 continue;
					  		   }
				  			   break;
				    	   }
		    			   if(str01.equals("$"))
		    				   break;
		    			   else
		    			   {
		    				   Car c=new Car

(); 
		    				   

c.car_no=str01;
		    				   passway.add

(c);
		    			   }
		    		   }
			       continue;
			case 2:System.out.println("请输入待进汽车

的编号：");
			       String str02=null;
			       while(true)
		    	   {
		    		   str02=method.sip();
		    		   if(stop.isIn(str02))
		    		   {
		    			   System.out.println("此

车已在停车场里，请重新输入！");
			  		 	   continue;
		    		   }
		    		   if(passway.isIn(str02))
		    		   {
		    			   System.out.println("此

车已在便道里，请重新输入！");
			  	 		   continue;
			  		   }
		  			   break;
		    	   }
			       Car c=new Car();
			       c.car_no=str02;
			       if(stop.size<5)  
			    	   stop.push(c);
			       else 
			    	   passway.add(c);
			       continue;
			case 3:System.out.println("请输入待出汽车

的停车位编号：");
			       int i2;
			       i2=method.iip(1,5);
		    	   if(i2>stop.size)
		    	   {
		    		   System.out.println("此车位尚无

汽车！");
		    		   continue;
		    	   }
			       stop.pop(i2, passway, temp);
			       continue;
			case 4:System.out.println("请选择查询区域

：");
			       System.out.println("1:停车场");
			       System.out.println("2:便道");
			       System.out.println("3:打印全部");
			       int i4=method.iip(1,3);
			       if(i4==1)
	    		   {
                       System.out.println("请输入待查询停车场车位

编号：");
	    			   int i41=method.iip(1,5);
	    			   if(stop.size<i41)
		    	    	   System.out.println("此车位尚无

汽车！");
		    	       else
		    		       System.out.println

(stop.data[i41-1].state.toString());
	    		   }
	    		   else
	    			   if(i4==2)
	    			   {
		    			   System.out.println("请

输入待查询便道车位编号：");
		    			   int i42=method.iip(1, 

100);
		    			   if(passway.size<i42)
			    			   

System.out.println("此车位尚无汽车！");
			    		   else
			    		   {
			    			   int ii=(i42-

1+passway.front)%passway.data.length;
			    			   

System.out.println(passway.data[ii].state.toString());
			    		   }
		    	       }
	    			   else
	    			   {
	    				   stop.print();
	    				   passway.print();
	    			   }
				   continue;
			case 5:System.out.println("欢迎再次使用！

");
			}
			break;
		}
    }
}
