// NgoQuyHuy.Baitap1. S20-K59TH. MSSV: 1751060671
BaiTap1
#include <conio.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
using namespace std;
struct sv
{
    char ten[20];
    char MSSV[10];
    char tenmon[30];
    int diem;
};
typedef struct NODE
{
    sv info;
    NODE *next;
};
typedef struct LIST
{
    NODE *head;
    NODE *tail;
    int n;
};
NODE* CreateNode (sv x)
{
    NODE *p;
    p=new NODE;
    if(p==NULL)  exit(1);
    p->info=x;
    p->next=NULL;
    return p;
}
void CreateList (LIST &L)
{
    L.head=L.tail=NULL;
}
void input (sv &x)
{
    printf("\nNhap MSSV: ");  fflush(stdin); gets(x.MSSV);
    printf("\nNhap ten: ");  fflush(stdin); gets(x.ten); 
    printf("\nNhap ten mon hoc: ");  fflush(stdin); gets(x.tenmon);     
    printf("\nNhap diem: "); scanf("%d", &x.diem);
}
void AddTail (LIST &L, NODE *p)
{
    if(L.head==NULL)  L.head=L.tail=p;
    else
    {
        L.tail->next=p;
        L.tail=p;
    }
}
void nhap (LIST &L)
{
    sv x;
    char kt;   
    printf("\nNhan phim bat ki de tiep tuc nhap.");
    printf("\nNhan 0 de dung nhap.");
    do
    {
        kt=getch();
        if(kt=='0')  break;
        input(x);
        NODE *p=CreateNode(x);
        AddTail(L,p);
    } while (1);
}
void output (sv x)
{
    printf("\n%s  %s %s %d",x.MSSV,x.ten,x.tenmon,x.diem);
}
void xuat (LIST L)
{
    NODE *p;
    p=L.head;
    while(p!=NULL)
    {
        output(p->info);
        p=p->next;
    }
}
void init(LIST &L)
{
		L.head = L.tail=NULL;
}
void maxdiem (LIST L)
{
    NODE *p,*max;
    int dem;
    p=L.head;
    max=p;
    while (p!=NULL)
    {
        if(p->info.diem>max->info.diem)  { max=p; dem=0; }
        if(p->info.diem==max->info.diem) { max=p; dem++; }
        p=p->next;
    }
    printf("\nSV co diem cao nhat la: \n");
    if(dem==0)
	  output(max->info);
    else
    {
        NODE *q=L.head;
        while (q!=NULL)
        {
            if(q->info.diem==max->info.diem) output(q->info);
            q=q->next;
        }
    }
}
void mindiem (LIST L)
{
    NODE *p,*min;
    int dem;
    p=L.head;
    min=p;
    while (p!=NULL)
    {
        if(p->info.diem<min->info.diem)  { min=p; dem=0; }
        if(p->info.diem==min->info.diem) { min=p; dem++; }
        p=p->next;
    }
    printf("\nSV co diem thap nhat la: \n");
    if(dem==0)
	  output(min->info);
    else
    {
        NODE *q=L.head;
        while (q!=NULL)
        {
            if(q->info.diem==min->info.diem) output(q->info);
            q=q->next;
        }
    }
}
void listappend (LIST &l1, LIST &l2)
{
	if(l2.head==NULL) return;
	if(l1.head == NULL)
		l1=l2;
	else
	{
		l1.tail->next=l2.head;
		l1.tail=l2.tail;
	}
	init(l2);
}
NODE *PickHead(LIST &L)
{
	NODE *p=NULL;
	if(L.head!=NULL){
		p=L.head;
		L.head=L.head->next;
		p->next==NULL;
		if(L.head==NULL) L.tail=NULL;
	}
	return p;
}
void tim (LIST L)
{
    NODE *p;
    int dem=0;
    char k[20];
    printf("\nNhap ten sv can tim: ");
    fflush(stdin);
    gets(k);
    p=L.head;
    while (p!=NULL)
    {
        if(strcmp(k,p->info.ten)==0)      dem++;
        p=p->next;
    }
    if(dem!=0)
    {
            printf("\nTim thay sv: "); output(p->info);
    }
    else printf("\nKo tim thay.");
}
void xoa (LIST &L)
{
    NODE *p, *q;
    char a[10];
    p=L.head;
    q=NULL;
    printf("\nNhap MSSV can xoa: ");
    fflush(stdin);
    gets(a);
    while (p!=NULL)
    {
        if(strcmp(a, p->info.MSSV)==0)    break;
        else printf("\nKo co sv can xoa.");
        q=p;
        p=p->next;
    }
    if(q!=NULL)
    {
        if(p!=NULL)
        {
            q->next=p->next;
            delete (p);
            if(p==L.tail)  L.tail=q;
            delete(p);
        }
    }
    else
    {
        L.head=p->next;
        delete(p);
        if(L.head==NULL)  L.tail=NULL;
    }
}

void interchangesort(LIST L) 
{ 
	NODE *i,*j; 
 	for(i=L.head;i!=L.tail;i=i->next)
 		for(j=i->next;j!=NULL;j=j->next)
  			 if(i->info.diem>j->info.diem){ 
       			int tmp=j->info.diem;
 				j->info.diem=i->info.diem;
				i->info.diem=tmp;
			}
}

void selectionsort (LIST &L)
{
    NODE *p,*q,*min;
    p=L.head;
    sv temp;
    while (p!=L.tail)
    {
        min=p;
        q=p->next;
        while (q!=NULL)
        {
            if(q->info.diem<min->info.diem)  min=q;
            q=q->next;
        }
        temp=p->info;
        p->info=min->info;
        min->info=temp;
        p=p->next;
    }
}
void listdistribute (LIST &L, LIST &l1, LIST &l2)
{
	NODE *p=PickHead(L);
	AddTail(l1,p);
	if(L.head)
		if(p->info.diem <= L.head->info.diem)
			listdistribute(L,l1,l2)	;
		else
			listdistribute(L,l2,l1);
}
void listmerge(LIST &L, LIST &l1, LIST &l2)
{	
	NODE *p;
	while ((l1.head) && (l2.head))
	{
		if(l1.head->info.diem <= l2.head->info.diem)
			p=PickHead(l1);
		else
			p=PickHead(l2);
		AddTail(L,p);  
	}
	listappend(L,l1);listappend(L,l2);
}
void listmergesort (LIST &L)
{	
	LIST l1,l2;
	if(L.head == L.tail) return;
	init(l1); init(l2); 
	listdistribute(L,l1,l2);
	if(l2.head){
		listmergesort(l1);
		listmergesort(l2);
		listmerge(L,l1,l2);
	}
	else listappend(L,l1);
}

void menu()
{
    LIST L;
    NODE *p,*q;
    sv x;
    char chon;
    CreateList(L);
    do
    {
        printf("\n\t\t\tMENU");
        printf("\n \t 1. Nhap ds");
        printf("\n \t 2. In ds");
        printf("\n \t 3. Ds sv co diem cao nhat");
        printf("\n \t 4. Ds sv co diem thap nhat");
        printf("\n \t 5. Sap xep ds: selection sort");
        printf("\n \t 6. Sap xep ds: interchange sort");
	printf("\n \t 7. Sap xep ds: merge sort");
	printf("\n \t 0. Huy thao tac dang thuc hien va hien lai menu");
        printf("\n \tNhap cac so de thuc hien thao tac!"); 
        chon=getch();
        switch(chon)
        {
            case '1': {nhap(L); break;}
            case '2': {xuat(L); break;}
            case '3': {maxdiem(L); break;}
            case '4': {mindiem(L); break;}
	    case '5': {selectionsort(L);printf("\nDs sau khi sap xep theo seclection sort: "); xuat(L); break;}
	    case '6': {interchangesort(L);printf("\nDs sau khi sap xep theo interchange sort: "); xuat(L); break;}
	    case '7': {listmergesort(L);printf("\nDs sau khi sap xep theo merge sort: "); xuat(L); break;}
            case '0': exit(1);
            default: printf("\nNhap lai.");
        }
    } while (chon!='0');
}
int main()
{
    while(1)
    {
        menu();
        getch();
    }
}
