13.) A barbershop consists of a waiting room with n chairs and a barber room with one barber chair. If there are no customers to be served, the barber goes to sleep. If a customer enters the barbershop and all chairs are occupied, then the customer leaves the shop. If the barber is busy but chairs are available, then the customer sits in one of the free chairs. If the barber is asleep, the customer wakes up the barber. Write a program to coordinate the barber and the customers ?

Sol:- 

We use 3 semaphores. Semaphore customers counts waiting customers; semaphore barbers is the number of idle barbers (0 or 1); and mutex is used for mutual exclusion. A shared data variable customers1 also counts waiting customers. It is a copy of customers. But we need it here because we can’t access the value of semaphores directly. We also need a semaphore cutting which ensures that the barber won’t cut another customer’s hair before the previous customer leaves


Q16.)Design a scheduling program that is capable of scheduling many processes that comes in at some time interval and are allocated the CPU not more than 10 time units. CPU must schedule processes having short execution time first. CPU is idle for 3 time units and does not entertain any process prior this time. Scheduler must maintain a queue that keeps the order of execution of all the processes. Compute average waiting and turnaround time.

Sol:-

For solving this problem the processes are scheduled considering their arrival and Wait time. I have made 5 functions:  readData(); Init(); getNextProcess(); dispTime(); computeSRT();User will get to enter process burst time and arrival time.Depending on that ComputeSRT function will calculate the shortest run time of the processes and it will be entered in gantt chart.After every unit time when a process will entered by user ComputeSRT will compare the remaining run time of currently running process and if less than new process then it will be preumpted.After completion of all the processes the program will prepare a gantt chart and program will end.
