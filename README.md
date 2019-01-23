# hello-world
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define m 6
#define n 8
int maze[m+2][n+2]={{1,1,1,1,1,1,1,1,1,1},
                    {1,0,1,1,1,0,1,1,1,1},
                    {1,0,0,0,0,1,1,1,1,1},
                    {1,0,1,0,0,0,0,0,1,1},
                    {1,0,1,1,1,0,0,1,1,1},
                    {1,1,0,0,1,1,0,0,0,1},
                    {1,0,1,1,0,0,1,1,0,1},
                    {1,1,1,1,1,1,1,1,1,1}};

typedef struct{
    int x;
    int y;
}item;

item move[4]={{0,1},{1,0},{0,-1},{-1,0}};

typedef struct{
    int x,y,d;
}DataType;

typedef struct{
    DataType  data[1000];
    int top;
}SeqStack,*PSeqStack;

PSeqStack Init_SeqStack()
{
    PSeqStack p;
    p=(PSeqStack)malloc(sizeof(SeqStack));
    if(p)
        p->top=-1;
    return p;
}

int Empty_SeqStack(PSeqStack p)
{
    if(p->top==-1)
        return 1;
    else
        return 0;
}

int Push_SeqStack(PSeqStack p,DataType x)
{
    if(p->top==999)
        return 0;
    else
    {
        p->top++;
        p->data[p->top]=x;
        return 1;
    }
}

int Pop_SeqStack(PSeqStack p,DataType *x)
{
    if(Empty_SeqStack(p))
        return 0;
    else
    {
        *x=p->data[p->top];
        p->top--;
        return 1;
    }
}

void Destroy_SeqStack(PSeqStack *p)
{
    if(*p)
        free(*p);
    *p=NULL;
    return;
}

int mazepath(int maze[][n+2],item move[],int x0,int y0)
{
    PSeqStack S;
    DataType temp;
    int x,y,d,i,j;
    temp.x=x0;
    temp.y=y0;
    temp.d=-1;
    S=Init_SeqStack();
    if(!S)
    {
        printf("栈初始化失败！！！");
        return 0;
    }
    Push_SeqStack(S,temp);
    while(!Empty_SeqStack(S))
    {
        Pop_SeqStack(S,&temp);
        x=temp.x;
        y=temp.y;
        d=temp.d+1;
        while(d<4)
        {
            i=x+move[d].x;
            j=y+move[d].y;
            if(0==maze[i][j])
            {
                temp.x=x;
                temp.y=y;
                temp.d=d;
                Push_SeqStack(S,temp);
                x=i;
                y=j;
                maze[x][y]=-1;
                if(x==m&&y==n)
                {
                    while(!Empty_SeqStack(S))
                    {
                        Pop_SeqStack(S,&temp);
                        printf("(%d,%d)<-",temp.x,temp.y);
                    }
                    Destroy_SeqStack(&S);
                    return 1;
                }
                else
                d=0;
            }
            else
            d++;
        }
    }
    Destroy_SeqStack(&S);
    return 0;
}

int main()
{
    mazepath(maze,move,1,1);
    return 0;
}
