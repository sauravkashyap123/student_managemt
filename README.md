# student_managemt
# its record management system
# source code of Student management system is 
#include<stdio.h>
#include<string.h>
#include<conio.h>
#include<windows.h>
#include<stdlib.h>
struct student{
	char name[30];
	char mobno[10];
	int roll;
	char course[10];
}s;
void gotoxy(int x,int y);
void menu();
void add();
void view();
void modify();
void deleteres();
void search();
void leave();
int main()
{
	gotoxy(15,8);
	printf("<!-- student record management system -->\n");
	gotoxy(15,12);
		printf("press any key to continue ...	");
	getch();
	system("cls");
	menu();
	return 0;
}
void menu()
{
	int n;
	gotoxy(40,3);
	printf("<!-- Menu -->");
	gotoxy(15,5);
		printf("1. ADD STUDENT RECORD");
		gotoxy(15,7);
	printf("2. view STUDENT RECORD\n");
	gotoxy(15,9);
	printf("3. modify STUDENT RECORD\n");
	gotoxy(15,11);
	printf("4. deleteres STUDENT RECORD\n");
	gotoxy(15,13);
	printf("5. search student record\n");
	gotoxy(15,15);
	printf("6. exit from file\n");
	gotoxy(15,17);
		printf("		enter your choise :	");
		scanf("%d",&n);
	switch(n)
	{
		case 1:
			add();
			break;
		case 2:
			view();
			break;
		case 3:
			modify();
			break;
		case 4:
			deleteres();
			break;
		case 5:
			search();
			break;
		case 6:
			leave();
			break;
	}
		
}
void add()
{
	char p='y';
	FILE *fp;
	fp=fopen("recordsystem.txt","rb+");
	if(fp==NULL)
	{
		printf("file can not open");
		exit(1);
	}
	while(p=='y')
	{
			system("cls");
		printf("	<!--adding record-->	\n\n");
		printf("\n\n	enter your name :	");
		fflush(stdin);
		//getchar();
		gets(s.name);
		printf("\n	enter your mobile number :	");
		gets(s.mobno);
		printf("\n	enter your roll number :	");
		scanf("%d",&s.roll);
		fflush(stdin);
		printf("\n	enter course :	");
		gets(s.course);
		fwrite(&s,sizeof(s),1,fp);
		printf("do you want add another record press 'y' for yes and 'n' for no  : ");
		fflush(stdin);
		p=getch();	
	}
	fclose(fp);
		printf("\npress any key to continue ...	");
	getch();
	system("cls");
	menu();	
}
void view()
{
	system("cls");
	int i=1;
	printf("	<!--viewing record-->	\n\n");
	char p='y';
	FILE *fp;
	fp=fopen("recordsystem.txt","rb+");
	if(fp==NULL)
	{
		printf("file does not exit please add record ");
		exit(1);
	}
	rewind(fp);
	printf("	s no	  name		   mobile no	      roll	   course");
	printf("\n  ------------------------------------------------------------------------\n");
	while(fread(&s,sizeof(s),1,fp)==1)
	{
		printf("%9d %17s %20s %10d %10s\n",i,s.name,s.mobno,s.roll,s.course);
		i++;
	}
	fclose(fp);
		printf("\npress any key to continue ...	");
	getch();
	system("cls");
	menu();
}
void leave()
{
	exit(1);
}
void search()
{
	FILE *fp;
	fp=fopen("recordsystem.txt","rb+");
	if(fp==NULL)
	{
		printf("file can not open");
		exit(1);
	}
	char ch[30];
	printf("	enter your name :	");
	getchar();
	gets(ch);
	
	while(fread(&s,sizeof(s),1,fp)==1)
	{
		if(strcmp(ch,s.name)==0)
		{
			printf("\n\n	Record found..	\n");
			printf("	  name		   mobile no	      roll	  course");
			printf("\n  ------------------------------------------------------------------------\n");
			printf("%17s %20s %10d %10s\n",s.name,s.mobno,s.roll,s.course);
		}
	}
	fclose(fp);
		printf("\npress any key to continue ...	");
	getch();
	system("cls");
	menu();
}
void modify()
{
	char ch[20];;
	FILE *fp;
	fp=fopen("recordsystem.txt","rb+");
	if(fp==NULL)
	{
		printf("file can not open");
		exit(1);
	}
	printf("\nenter the name of person to which you want to modify data ");
	fflush(stdin);
	gets(ch);
/*	printf("choose the option for modifying data\n");
	printf("1). modify name\n");
	printf("2). modify mobile number\n");
	printf("3). modify roll no\n");
	printf("4). modify course\n");*/
	while(fread(&s,sizeof(s),1,fp)==1)
	{
		if(strcmp(ch,s.name)==0)
		{
			fseek(fp,-sizeof(s),SEEK_CUR);
			printf("\n\n	enter name :	");
		fflush(stdin);
	//	getchar();
		gets(s.name);
		printf("\n	enter mobile number :	");
		gets(s.mobno);
		printf("\n	enter roll number :	");
		scanf("%d",&s.roll);
		fflush(stdin);
		printf("\n	enter course :	");
		gets(s.course);
		fseek(fp,-sizeof(s),SEEK_CUR);
		fwrite(&s,sizeof(s),1,fp);
		fclose(fp);
		menu();
		}
	}
}
void deleteres()
{
	FILE *fp,*ft;
	char ch[20];
	fp=fopen("recordsystem.txt","rb+");
	if(fp==NULL)
	{
		printf("file can not open");
		exit(1);
	}
	ft=fopen("another.txt","wb+");
	if(ft==NULL)
	{
		printf("file can not open");
		exit(1);
	}
	printf("enter the name to which want to delete data\n");
	getchar();
	gets(ch);
	while(fread(&s,sizeof(s),1,fp)==1)
	{
		if(strcmp(ch,s.name)!=0)
		{
		fwrite(&s,sizeof(s),1,ft);
		}
	}
			fclose(fp);
		fclose(ft);
		remove("recordsystem.txt");
		rename("another.txt","recordsystem.txt");
	printf("\n press any key to continue ...");
	getch();
	system("cls");
	menu();	
}
void gotoxy(int x,int y)
{
	COORD coord;
	coord.X=x;
	coord.Y=y;
	SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),coord);
}
