# sigsegv
#include<stdio.h>
#include<stdlib.h>
//typedef struct Node* List;
typedef struct PolyNode* Polymial;
struct PolyNode {
	int coef;
	int expon;
	Polymial link;
} ;
/*   需要设计的函数：*/ 
//读并保存多项式 
void attach(int coef,int expon,Polymial* ptrear){
	Polymial p;
	p=(Polymial)malloc(sizeof(struct PolyNode));
	p->coef=coef;
	p->expon=expon;
	p->link=NULL;
	(*ptrear)->link=p;
	*ptrear=p;
}
Polymial ReadPoly(){
	int n;
	Polymial rear,front,temp;
	rear=(Polymial)malloc(sizeof(struct PolyNode));
	front=rear;
	front->link=NULL;
	int coef,expon;
	scanf("%d",&n);
	while(n--){
		scanf("%d %d",&coef,&expon);
		attach(coef,expon,&rear);
	}
	temp=front;
	front=front->link;
	free(temp);
	return front;
}
//多项式相乘
Polymial Mulit(Polymial p1,Polymial p2){
	Polymial t1=p1,t2=p2;
	Polymial rear,temp,p;
	p=rear=(Polymial)malloc(sizeof(struct PolyNode));
	rear->link=NULL;
	if(!t1||!t2) return NULL;
	while(t2){
		attach(t1->coef*t2->coef,t1->expon+t2->expon,&rear);
		t2=t2->link;
	}
	t1=t1->link;
	while(t1){
		int c,e;
		t2=p2;
		rear=p;
		while(t2){
			c=t1->coef*t2->coef;
			e=t1->expon+t2->expon;
			while(rear->link&&e<rear->link->expon){
				rear=rear->link;
			}
			if(rear->link&&e==rear->link->expon){
				if(c+rear->link->coef){
					rear->link->coef+=c;
				}
				else{
					temp=rear->link;
					rear->link=rear->link->link; 
					free(temp);
				}
			}
			else{
				temp=(Polymial)malloc(sizeof(struct PolyNode));
				temp->coef=c;
				temp->expon=e;
				temp->link=rear->link;
				rear->link=temp;
				rear=temp;
			}
			t2=t2->link;
		}
		
	}
	temp=p;
	p=p->link;
	free(temp);
	return p;
}
//多项式相加 
Polymial Add(Polymial p1,Polymial p2){
	if(!p1&&!p2) return NULL;
	Polymial t1=p1,t2=p2,front,rear,temp;
	front=rear=(Polymial)malloc(sizeof(struct PolyNode));
	front->link=NULL;
	while(t1&&t2){
		if(t1->expon>t2->expon){
			attach(t1->coef ,t1->expon,&rear);
			t1=t1->link;
		}
		else if(t2->expon>t1->expon){
			attach(t2->coef ,t2->expon,&rear);
			t2=t2->link;
		}
		else{
			attach(t2->coef+t1->coef ,t2->expon,&rear);
			t1=t1->link;
			t2=t2->link;
		}
		while(t1){
			attach(t1->coef ,t1->expon,&rear);
			t1=t1->link;
		}
		while(t2){
			attach(t2->coef ,t2->expon,&rear);
			t2=t2->link;
		}
		temp=front;
		front=front->link;
		free(temp);
	}
	return front;
}
//输出多项式 
void PrintPoly(Polymial p){
	int flag=0;
	while(!p){
		printf("0 0");return;
	}
	while(p){
		if(p->coef!=0){
			if(!flag)
			flag=1;
		else
			printf(" ");
		printf("%d %d",p->coef,p->expon);	
		}	
		p=p->link;	
		}
		//printf("\n");
		return;
	}

int  main() {
	//变量预处理
	Polymial P1,P2,PP,PS; 
	//读入多项式1
	P1=ReadPoly(); 
	//读入多项式2
	P2=ReadPoly();
	//乘法运算并输出
	PP=Mulit(P1,P2);
	PrintPoly(PP);printf("\n");
	//加法运算并输出 
	PS=Add(P1,P2);
	PrintPoly(PS);
	 
	return 0;
}
