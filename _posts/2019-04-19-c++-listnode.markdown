---
layout: post
title:  "C/C++之单链表的节点删除和插入"
date:   2019-04-19 22:49:52 +0800
categories: C/C++
tags: C_or_CPP
---
```C++
#include<stdio.h>
#include<stdlib.h>
 
//Linked-list structure
typedef struct ListNode
{
  int data;                      //data field
  struct ListNode *next;         //point to next node
} Node,*PNode;
 
//creat a linklist,the counts of node is cnt,datasource is pData
PNode CreatLinkedList(const int cnt,int *pData)
{
  //malloc head node
  PNode pHead=(PNode)malloc(sizeof Node);
  if(pHead==NULL)//if phead is null
  {
    return NULL;
  }
  int i=0;
  PNode pRail=pHead;            //rail node for traversing linked list
  while(i<cnt)
  {
    PNode pNext=(PNode)malloc(sizeof Node);//malloc every node
    if(pNext==NULL)
      break;
    pNext->data=*pData++;
 
    pRail->next=pNext;
    pNext->next=NULL;
    pRail=pNext;
 
    ++i;
  }
 
  return pHead;
}
 
//delete the node that the data==num 
PNode DeleteNode(PNode list,int num)
{
  if(list->data==num)
  {
    return list->next;
  }
 
  PNode p=list;
  PNode pn=NULL;
 
  while (p!=NULL)
  {
    pn=p;                         //save privious node
    p=p->next;                     //increase
    if (p==NULL)
    {
      printf("There is no finding in the list!\n");
    }
    else if(p->data==num)
    {
      pn->next=p->next;
    }
    else
    {
      //printf("There is no finding in the list!\n");
    }
  }
  return list;
}
 
//insert one node(data=ins_num) behind the node which data==num
PNode InsertNode(PNode list,int num,int ins_num)
{
  PNode p=list;
 
  while (p!=NULL)
  {
    if(p->data==num)
    {
      PNode pNew=(PNode)malloc(sizeof PNode);
      pNew->data=ins_num;
      pNew->next=p->next;
      p->next=pNew;
      break;
    }
    //increase
    p=p->next;
  }
 
  return list;
}
 
//main.c   test
int main()
{
  int n=5;
  int a[5]={0,1,2,3,4};
  PNode lnode;
  lnode=CreatLinkedList(n,a);
  DeleteNode(lnode,5);
  PNode p=InsertNode(lnode,3,6);
 
  //printf the linked list
  while(p->next!=NULL)
  {
    p=p->next;
    printf("%d\n",p->data);
  }
  printf("Hello World\n");
  system("pause");
  return 0;
}
 
 
 
 
```