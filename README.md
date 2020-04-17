13.) A barbershop consists of a waiting room with n chairs and a barber room with one barber chair. If there are no customers to be served, the barber goes to sleep. If a customer enters the barbershop and all chairs are occupied, then the customer leaves the shop. If the barber is busy but chairs are available, then the customer sits in one of the free chairs. If the barber is asleep, the customer wakes up the barber. Write a program to coordinate the barber and the customers ?
Sol:- 
We use 3 semaphores. Semaphore customers counts waiting customers; semaphore barbers is the number of idle barbers (0 or 1); and mutex is used for mutual exclusion. A shared data variable customers1 also counts waiting customers. It is a copy of customers. But we need it here because we can’t access the value of semaphores directly. We also need a semaphore cutting which ensures that the barber won’t cut another customer’s hair before the previous customer leaves
Code : 
semaphore customers = 0; 
semaphore barbers = 0; 
semaphore cutting = 0; 
semaphore mutex = 1; 
int customer1 = 0; 
 
void barber() 
{  
while(true) 
{   
wait(customers);    //sleep when there are no waiting customers   
wait(mutex);  //mutex for accessing customers1   
customers1 = customers1 - 1;   
signal(barbers);   
signal(mutex);   
cut_hair();  
} 
}  
 void customer() 
{  
wait(mutex);  //mutex for accessing customers1 
 if (customers1 < n) 
{   
customers1 = customers1 + 1;   
signal(customers);   
signal(mutex); 
wait(barbers); //wait for available barbers   
get_haircut();  
} 
 else 
{ 
//do nothing (leave) when all chairs are used.   
signal(mutex);
 } 
}
 cut_hair()
{  
waiting(cutting); 
} 
get_haircut()
{  
get hair cut for some time;  
signal(cutting); }

Q16.)Design a scheduling program that is capable of scheduling many processes that comes in at some time interval and are allocated the CPU not more than 10 time units. CPU must schedule processes having short execution time first. CPU is idle for 3 time units and does not entertain any process prior this time. Scheduler must maintain a queue that keeps the order of execution of all the processes. Compute average waiting and turnaround time.
Sol:-
For solving this problem the processes are scheduled considering their arrival and Wait time. I have made 5 functions:  readData(); Init(); getNextProcess(); dispTime(); computeSRT();User will get to enter process burst time and arrival time.Depending on that ComputeSRT function will calculate the shortest run time of the processes and it will be entered in gantt chart.After every unit time when a process will entered by user ComputeSRT will compare the remaining run time of currently running process and if less than new process then it will be preumpted.After completion of all the processes the program will prepare a gantt chart and program will end.
Code:
#include<stdio.h>
int No,bt[10],at[10],tat[10],wt[10],rt[10],finish[10],twt,ttat,total,tq=0; 
void readData()
{
    printf("Enter no. of processes\n");
    scanf("%d",&No);
    printf("Enter the arrival times in order:\n");at[0]=0;
    printf("\n%d ----(Note:-for 1st process AT is by default '0' so start from of 2nd  		process)\n",at[0]);
    for(int i=1;i<No;i++)
    scanf("%d",&at[i]);
    printf("Enter the burst times in order :\n");
    for(int i=0;i<No;i++)
    scanf("%d",&bt[i]);
}
 void Init()
{
    total=0;
    twt=0;
    ttat=0;
    for(int i=0; i<No; i++)
    {
        rt[i]=bt[i];
        finish[i]=0;
        wt[i]=0;
        tat[i]=0;
        total+=bt[i];
     }
}
int getNextProcess(int time)
{ 
     int i,low;
     for(i=0;i<No;i++)
     {      if(finish[i]==0)   {    low=i; break;  }   }
for(i=0;i<No;i++)
      	{
            	if(finish[i]!=1)
            	{ if(rt[i]<rt[low] && at[i]<=time)   {  low=i; }   }
}
        	return low;
 }

void dispTime()
{
    for(int i=0;i<No;i++)
    {
        twt+=wt[i];
        tat[i]=wt[i]+bt[i];
        ttat+=tat[i];
        printf("waiting time for P%d == %d, Turnaround time= %d \n",i+1,wt[i],tat[i]);
    }
    printf("Avg Waiting time = %f and Avg Turnaround time =  		%f\n",(double)twt/No,(double)ttat/No);
    printf("\n\t\t\t--Scheduling complete--\n");
}
void computeSRT()
{
    readData();
    Init();
    int time,next=0,old,i,new_time;
    printf("Gantt Chart\n ");
    for(time=0;time<total;time++)
    {
        old=next;
        next=getNextProcess(time);
        if((old!=next &&tq==0 ) ||time==0)
        {		printf("(t=%d)|----p%d----|",time,next+1);        }
        rt[next]=rt[next]-1;
        if(rt[next]==0) 
        {   finish[next]=1;  }
        for(i=0;i<No;i++)
        {
            if(i!=next && finish[i]==0 && at[i]<=time)
            {                wt[i]++;}
         }
  	if(time!=0&&old==next&&time%10==0){next=getNextProcess(time);printf("(t=%d)|----			p%d----|",time,next+1);}
    }
    printf("( %d ) \n",total);
    for(i=0;i<No;i++)
    {    if(!finish[i]) 
        {
            printf("Scheduling failed, cannot continue \n "); 
            return;
        }
    dispTime();
     }
}
int main(){
    void readData();
    void computeSRT();
    void Init();
    void dispTime();
    int getNextProcess(int);
    computeSRT();                  }
