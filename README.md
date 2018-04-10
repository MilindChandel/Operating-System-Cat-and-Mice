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
