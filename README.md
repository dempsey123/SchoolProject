
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include <stdlib.h>
#include<string.h>

//this is a ATm model in C language
struct node{
	int no;
	char name[10];
	char pw[7];
	double balance;
	char level;
	struct node*next;
};

void output(struct node *p)//output the data into the file after the operation.
{
	node*head = p->next;
	FILE*res = fopen("account.txt", "w");
	while (head){
		fprintf(res, "%04d %s %s %f %c\n", head->no, head->name, head->pw, head->balance, head->level);
		head = head->next;
	}
	fprintf(res, "\n");
	fclose(res);
}

void function_list(int no, char name[], char pw[], double balance, char level, struct node* des, struct node *head);

void Inquiry(double balance)//it is inquity function
{
	printf("Here is the Inquiry function\n");
	getchar();
	printf("the balance of your account is %.2lf\n", balance);
	getchar();

}

void Save(struct node *des, struct node *head)//it is save function
{
	printf("Here is the Save function\n");
	getchar();
	double amount;
	printf("Please input the money you want to save in:");
	scanf("%lf", &amount);
	while (amount <= 0)//protect the bank system if some people input a negative number or zero
	{
		printf("warning:you should save a positive amount.\n");
		scanf("%lf", &amount);
	}
	if (amount>0)
	{
		des->balance += amount;
		printf("Your inquiry is %.2lf\n", des->balance);
		output(head);// save the money in account.txt
	}

	getchar();
	getchar();

}

void Withdraw(struct node *des, struct node *head)
{
	printf("Here is the Withdraw function\n");
	getchar();
	double amount;
	printf("please input the amount of money you want to withdraw:\n");
	scanf("%lf", &amount);
	if (amount <= des->balance){//judge the amount if larger than the balance.
		if (amount>2000.0&&amount <= 3000.0){
			if (des->level == 'V')//judge the level of the user.
				des->balance -= amount;
			else if (des->level == 'N')
				printf("warning:you are not a vip account.\n");
		}
		if (amount>3000)//judge the amount
			printf("warning:do not allow to withdraw the amount larger than 3000.\n");
		if (amount <= 2000)//also judge amount.
			des->balance -= amount;
	}
	else if (amount>des->balance)
		printf("warning:you do not have enough money.\n");
	output(head);
	printf("Your inquiry is%.2lf\n", des->balance);
	getchar();
	getchar();


}

void Transfer(double balance, char level, struct node *des, struct node *head) //this is using for deliver the head addaress of link list that creat in the sub-function
{
	printf("Here is the Transfer function\n");
	getchar();
	double m;
	int id;
	char name[30];
	struct node *current;
	current = head; //get the haed address of link list
	printf("your balance is %.2lf\n", balance);
	if (level == 'N') // at first judge that if the user if is vip or not
	{
		printf("you are the normal account, the most money you can transfer is 10000\nplease input the money you want to transfer: ");
		scanf("%lf", &m);  //receive the money user want to transfer
		if (m > balance) // and the judge the amount
		{
			printf("not sufficient funds,please input the new one: ");
			scanf("%lf", &m);  //if it is excess the requirement, give a warning and require user to input again
			if (m < balance&&m>10000)
			{
				printf("the money excess the requirement,input the new one: ");
				scanf("%lf", &m);
				printf("input the name: ");
				getchar(); // this is use for clearing the Enter
				gets_s(name); //receive the name and next judge the name if exist in the account.txt file
				printf("input the id: ");
				scanf("%04d", &id);
			}
			else
			{
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}

		}
		else if (m < balance)  // this is esactly the same with before.
		{
			if (m>10000)
			{
				printf("the money excess the requirement,input the new one: ");
				scanf("%lf", &m);
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}
			else
			{
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}

		}
		while (current != NULL)  //in the subfunction we have put the data in the link list we had created and now we let the name user input to compare with the name in link list.
		{
			if (current->no != id)
			{
				current = current->next; //point the the next link list
			}
			else if (current->no == id)  // if it is matched, alter the information.
			{
				des->balance = balance - m - (m*0.01);
				current->balance = (current->balance) + m;
				break;
			}
		}
		if (current == NULL)
			printf("the people you want to transfer to does not exist in out bank\n");
		else if (current->no == id)
		{
			printf("the person you choose to transfer is: %s\n", current->name);
			printf("you transfer %.2f money,the balance now you have is %lf\n", m, des->balance);
			//printf("%lf\n", current->balance);
		}
	}
	else if (level == 'V')  //the procedures in here is the same as when the user is normal
	{
		printf("you are the VIP account, the most money you can transfer is 20000\nplease input the money you want to transfer: ");
		scanf("%lf", &m);
		if (m > balance)
		{
			printf("not sufficient funds,please input the new one: ");
			scanf("%lf", &m);
			if (m<balance&&m>20000)
			{
				printf("the money excess the requirement,input the new one: ");
				scanf("%lf", &m);
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}
			else
			{
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}
		}
		else if (m < balance)
		{
			if (m>20000)
			{
				printf("the money excess the requirement,input the new one: ");
				scanf("%lf", &m);
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}
			else
			{
				printf("input the name: ");
				getchar();
				gets_s(name);
				printf("input the id: ");
				scanf("%04d", &id);
			}

		}
		while (current != NULL)
		{
			if (current->no != id)
			{
				current = current->next;
			}
			else
			{
				des->balance = balance - m - (m*0.01);
				current->balance = (current->balance) + m;
				break;
			}
		}
		if (current == NULL)
			printf("the people you want to transfer to does not exist in out bank\n");
		else if (current->no == id)
		{
			printf("the person you choose to transfer is: %s\n", current->name);
			printf("you transfer %.2f money,the balance now you have is %lf\n", m, des->balance);
		}
	}
	output(head); //after we change the information in link list, we call the sub-function to store the information into link list
	getchar();
	getchar();
}

void Quit(struct node *head)  //quit the bank system
{
	struct node *current, *next;
	current = head;
	while (current != NULL)  //free the link list one by one
	{
		next = current->next;
		free(current);
		current = next;
	}
	exit(0);
}

void ChangePassword(struct node *current, struct node*head)
{
	char pw1[7], pw2[7];
	getchar();
	printf("the password must less than 6 character!\n");
	printf("input the new password:");
	gets_s(pw1);
	printf("input your new password again:");
	gets_s(pw2);
	while ((strlen(pw1) != 6) && (strlen(pw2) != 6))//if the new password is more than 6 digits, then input again
	{
		printf("the password you type in isn't equal to 6, please change!!!\n");
		getchar();
		printf("input the new password:");
		gets_s(pw1);
		printf("input your new password again:");
		gets_s(pw2);
	}	
	while (strcmp(pw1, pw2) != 0) //after that judge pw1 and pw2 if they are match.
	{
		printf("the password you input is not the same, please change\n");
		printf("input the new password:");
		gets_s(pw1);
		printf("input your new password again:");
		gets_s(pw2); //if not match, quit out the system.
	}
	if(strcmp(pw1, pw2)==0)
	{
		strcpy(current->pw, pw1);
		printf("change password sucessfully, please press ENTER,return to surface\n");
	}
	
	output(head);
	getchar();
}

struct node* interface(FILE *des)//read the data and store into a linked list.
{
	int no;
	char name0[10], pw0[7];
	double balance;
	char level;
	struct node *p1, *head, *p2;
	p1 = (struct node*)malloc(sizeof(struct node));//creat a dynamic memory/
	head = p1;//head is a empty node,just use for storing the pointer  points to next node
	p2 = p1;//initalize the p2
	while (fscanf(des, "%d %s %s %lf %c", &no, name0, pw0, &balance, &level) != EOF){
		p2 = (struct node*)malloc(sizeof(struct node));//creat another node which use for store data
		p2->no = no;
		strcpy(p2->name, name0);
		strcpy(p2->pw, pw0);
		p2->balance = balance;
		p2->level = level;
		p1->next = p2;//link to the last node
		p1 = p2;
	}
	p2->next = NULL;
	return head;
}

int main()
{
	char user[30], newname[30], pw[7], newpw[6];
	int counter = 0, number;
	struct node *current, *head, *pnew;
	FILE*acc = fopen("account.txt", "r");
	printf("choose to login in           --Press 1\n");
	printf("choose to create an account  --Press 2\n");
	scanf("%d", &number);
	//getchar();
	while ((number != 1) && (number != 2)){
		printf("Please input number 1 or 2:\n");
		scanf("%d", &number);
	}
	getchar();
	if (number == 1)//if input 1, then it is login in the account function
	{
		printf("input yout account:\n");
		gets_s(user);
		printf("input your password:\n");
		scanf("%s", pw);
		current = interface(acc);//use this subfunction to load the data in
		fclose(acc);
		head = current;
		while (current->next != NULL)//use for read the account name which is the same as the user name
		{
			if (strcmp(user, current->name) != 0)
				current = current->next;
			else
				break;
		}
		if (strcmp(user, current->name) == 0)//judge the password
		{
			if (strcmp(pw, current->pw) == 0) {
				function_list(current->no, current->name, current->pw, current->balance, current->level, current, head);
			}
			else do//calulate how many times the user have input the password
			{
				printf("please input again:\n");
				scanf("%s", pw);
				counter++;
			} while (counter < 2 && strcmp(pw, current->pw) != 0);
			if (counter == 2 && strcmp(pw, current->pw) != 0)//if the user input 3 times
			{
				printf("Warning:You had input 3 times.\n");
				Quit(head);
			}
			else {
				printf("You are in.\n");
				function_list(current->no, current->name, current->pw, current->balance, current->level, current, head);
			}
		}
		else//if the account name is not exist
		{
			printf("Warning:You are not a account in bank\n");
			Quit(head);
		}
	}
	else if (number == 2)//if input 2, then it is create a new account function
	{
		current = interface(acc);//read the account.txt into link list
		fclose(acc);
		head = current;
		printf("Please input your new name:\n");//input some account data
		gets_s(newname);
		printf("Please input your new password:\n");
		scanf("%s", newpw);
		while (current->next != NULL)
		{
			current = current->next;
		}
		if (current->next == NULL)
		{
			pnew = (struct node*)malloc(sizeof(struct node));
			pnew->no = current->no + 1;	//store the id 
			strcpy(pnew->name, newname);
			strcpy(pnew->pw, newpw);
			pnew->balance = 0;
			pnew->level = 'N';
			pnew->next = NULL;
			current->next = pnew;
			current = pnew;
			output(head);//save the data in the account.txt
			printf("Create successful\n");
			printf("Your id is:%04d\n", pnew->no);
			getchar();
			getchar();
			function_list(current->no, current->name, current->pw, current->balance, current->level, current, head);//login in the bank
		}

	}
}

void function_list(int no, char name[], char pw[], double balance, char level, struct node *des, struct node *head) {
	int i;

	do {

		printf("\t\t|$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|\n");
		printf("\t\t|                                               |\n");
		printf("\t\t|       Welcome to Use ATM System         	|\n");
		printf("\t\t|                                               |\n");
		printf("\t\t|    Please Select The Following Functions:     |\n");
		printf("\t\t|                                               |\n");
		printf("\t\t|   $$-Inquery	 -- Press 1			|\n");
		printf("\t\t|   $$-Save      -- Press 2			|\n");
		printf("\t\t|   $$-Withdraw	 -- Press 3			|\n");
		printf("\t\t|   $$-Transfer	 -- Press 4			|\n");
		printf("\t\t|   $$-Quit      -- Press 5			|\n");
		printf("\t\t|   $$-Change Password     -- Press 6		|\n");
		printf("\t\t|                                               |\n");
		printf("\t\t|$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|\n");

		printf("\n\n\n\n");

		printf("Please Input Number:");
		scanf("%d", &i);

		switch (i)
		{
		case 1: Inquiry(balance); break;
		case 2: Save(des, head); break;
		case 3: Withdraw(des, head); break;
		case 4: Transfer(balance, level, des, head); break;
		case 5: Quit(head); break;
		case 6: ChangePassword(des, head); break;
		default: printf("Number should between 1 -- 6!\n");
		}

	} while (1);
}

