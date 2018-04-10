#include<stdio.h>
#include<semaphore.h>
#include<pthread.h>
#include<stdlib.h>

int MyCats=0,MyMouse=0;
sem_t cat_count,Mymousecount;
pthread_t p1,p2,p3,p4,p5;
void * func1();//Function for the process of cat Eating the food
void * func2();//Function for the process of moyuse Eating the food
pthread_mutex_t mutex,mycatmutex,mymousemutex;
int Pieces[20],a=0 ,os[20];

void * func1()
{
	pthread_mutex_lock(&mutex);
	MyCats=MyCats+1;
	a=MyCats;
	printf("MyCat %d is  executing \n",MyCats);
	printf("MyCat %d is sleeping \n",MyCats);
	sleep(3); //The cat is pretty lazy so it slept after eating 
	
	printf("MyCat %d wake up \n",MyCats);
	while(MyMouse>0)
	{
	sem_destroy(&Mymousecount);//Mouse is being eaten here R.I.P mouse 
	printf("MyMouse %d died \n %d",MyMouse,Mymousecount);
	os[MyMouse]=-1;
	MyMouse=MyMouse-1;
	}
	printf("MyCat %d is sleeping again\n",MyCats);
	sleep(3);
	
	printf("MyCat %d wake up and eating\n",MyCats);
	Pieces[a]=a;
	printf("MyCat %d has executed \n",MyCats);
	pthread_mutex_unlock(&mutex);
}
void * func2()
{
		MyMouse=MyMouse+1;
		os[MyMouse]=MyMouse;
		int val=MyMouse;
	
	sem_wait(&Mymousecount);
	if(MyMouse==1){
		pthread_mutex_lock(&mymousemutex);
	}
	printf("MyMouse %d is eating\n",MyMouse);
	printf("MyMouse %d is sleeping \n",MyMouse);
	sleep(5);
	if(val!=os[val])
	{
		return;
	}
	printf("MyMouse %d wake  up and eating \n",MyMouse);
	sleep(6);
	printf("MyMouse %d wake  up and eating \n",MyMouse);
	printf("MyMouse %d has executed\n",MyMouse);
	
	pthread_mutex_unlock(&mymousemutex);
}







int main()
{
	sem_init(&cat_count,0,5);
	sem_init(&Mymousecount,0,5);
	
			printf(" When cat will eat and then mouse will not be eating \n");
			pthread_create(&p1,NULL,func1,NULL);
			sleep(7);
			pthread_create(&p2,NULL,func1,NULL);
			sleep(3);
			pthread_create(&p3,NULL,func1,NULL);
			sleep(6);
			printf(" When cat will eat and then mouse will be eating together \n");
			pthread_create(&p4,NULL,func1,NULL);
			pthread_create(&p5,NULL,func2,NULL);
			pthread_join(p1,NULL);
			pthread_join(p2,NULL);
			pthread_join(p3,NULL);
			pthread_join(p4,NULL);
			pthread_join(p5,NULL);
	}
