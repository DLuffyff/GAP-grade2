# GAP-grade2
数据结构 图
 #include<stdio.h>
#include <stdlib.h>
#define  OK   1
#define  ERROR   -1
#define  OVERFLOW   -1
#define TRUE 1
#define FALSE 0
#define SIZE 100 //循环队列长度
typedef  int  Status ;
typedef  int  ElemType ; 
int a[8][8]={
	{0,1,1,0,  0,0,0,0},
	{1,0,0,1,  1,0,0,0},
	{1,0,0,0,  0,1,1,0},
	{0,1,0,0,  0,0,0,1},
	{0,1,0,0,  0,0,0,1},
	{0,0,1,0,  0,0,1,0},
	{0,0,1,0,  0,1,0,0},
	{0,0,0,1,  1,0,0,0}
};
typedef struct
{
	int *base;
	int rear;
	int front;
}squeue;
int creat(squeue &s)// 建队列 
{
		s.base=(int*)malloc(SIZE*sizeof(squeue));
			if(!s.base) return 0;
			s.front=s.rear=0;
			return 1;
 } 

int state(squeue &s)//队列状态
{
if((s.rear-s.front+SIZE)%SIZE==0)   return 0;//队空 
return 1; 
}
int enter(squeue &s,int e)//入队 
{
	if((s.rear+1)%SIZE==s.front)s.base[s.rear]=e;
	s.rear=(s.rear+1)%SIZE;
	return OK; 
}
int  del(squeue &s)//出队 
{
int e;
if(!state(s))	return 0;
e=s.base[s.front];
s.front=(s.front+1)%SIZE;
return e;
}
bool visited[8];   // 访问标志数组
Status (* VisitFunc)(int v);    // 函数变量
Status visit( int v ){
	printf("%d ",v+1);
	return OK;
}
int FirstAdjVex(int a[8][8], int v){//返回下一个元素
 
	int i;
	for(i=v;i<8;i++){
		if(a[v][i]!=0)
		return i;
	}			
	return -1;
}
int NextAdjVex(int a[8][8],int v,int w){//返回下一个的下一个 
	int i;
	for(i=w;i<8;i++){
		if(a[v][i]!=0&&!visited[i])
		return i;
	}
	return -1;
}
void DFS(int a[8][8], int v) // 从第v个顶点出发递归地深度优先遍历图G。
 { 
 int w;
 visited[v]=TRUE;VisitFunc(v);
 for(w=FirstAdjVex(a,v);w>=0;w=NextAdjVex(a,v,w))
 if(!visited[w])DFS(a,w);
}
void DFSTraverse(int a[8][8], Status (*Visit)(int v))// 对图G作深度优先遍历。
 {  int v;
	VisitFunc=visit;
	for(v=0;v<8;++v)visited[v]=FALSE;
	for(v=0;v<8;++v)
	{
		if(!visited[v])DFS(a,v);
	}
}
void BFSTraverse(int a[8][8], Status (*Visit)(int v )) {
	int v,u,w; squeue s;
	for(v=0;v<8;++v)visited[v]=FALSE;
	creat(s);
	for(v=0;v<8;++v)
	if(!visited[v]){
		visited[v]=TRUE;
		visit(v);
		enter(s,v);
		while(state(s)){
			u=del(s);
		 for(w=FirstAdjVex(a,u);w>=0;w=NextAdjVex(a,u,w))
		 if(!visited[w])
		 {
		 	visited[w]=TRUE;visit(w);
		 	enter(s,w);
			 }	
		}		
	}
}
int main(){
	DFSTraverse(a,visit);
	printf("\n");
	BFSTraverse(a,visit);
	return 0;
}
