<div align="center">

## Sarojini Blackjack


</div>

### Description

This a Command Line Interface version of the Blackjack game. Pretty primitive, but just as good. It's dedicated to my C++ lecturer, Ms Sarojini =)
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Wong Yat Seng](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/wong-yat-seng.md)
**Level**          |Beginner
**User Rating**    |4.7 (14 globes from 3 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Games](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/games__3-13.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/wong-yat-seng-sarojini-blackjack__3-5005/archive/master.zip)





### Source Code

```
//***********************************
//*	 SAROJINI'S VIRTUAL BLACKJACK	*
//*---------------------------------*
//*	Author  : Wong Yat Seng		*
//*	Language : C++					*
//*	File   : SaroBJ.cpp			*
//*	Date   : 20/10/2002			*
//***********************************
#include <iostream.h>
#include <stdlib.h>
#include <iomanip.h>
#include <ctype.h>
#include <math.h>
#include <time.h>
int push=0;				//for val 15 pushes
int u_limit=13;			//upper risk limit
int deck=1;				//number of decks used
int c_card[5]={0,0,0,0,0};				//card slots (computer)
int p_card[5]={0,0,0,0,0};				//      (player1)
int c_open[5]={0,0,0,0,0};				//card flip (computer)
int p_open[5]={1,1,0,0,0};				//      (player1)
int dealt[11]={0,0,0,0,0,0,0,0,0,0,0};	//dealt   (buffer)
int games_won=0;		//global stats for 1P Game
int games_lost=0;		//
int games_played=0;		//
//functions
char convert_num(int);
void one_play();		//Main Game Menu
int deal_card1();		//Deal cards for P1 and COMP
void main()
{
	char choice='z';			//init loop val
	while (choice!='Q')			//repeat menu until quit
	{
		choice='z';				//reset choice for next input
		system("cls");			//clear screen
		cout<<"\n\n\n\n"
			<<"\t\t\t**************************************\n"
			<<"\t\t\t|                  |\n"
			<<"\t\t\t| Welcome to Sarojini's Blackjack! |\n"
			<<"\t\t\t|                  |\n"
			<<"\t\t\t**************************************\n\n"
			<<"\t\t\t  (J)oin Blackjack Table\n"
			<<"\t\t\t  (D)eck Setting - "<<deck<<"\n"
			<<"\t\t\t  (Q)uit Game\n\t\t\t  ";
		while (choice!='J'&&choice!='D'&&choice!='Q')
		{								//trap bad choice
			cin >>choice;
			choice=toupper(choice);		//capitalized choice
			switch(choice)
			{
			case 'J':
				one_play();				//call 1P func
				break;
			case 'D':					//Change deck settings
				cout<<"\n\t\t\tCurrent Deck(s) : "<<deck<<" ("<<(deck*52)<<" cards)"<<endl;
				deck=0;
				while (deck<1)
				{
					cout<<"\t\t\tNew # of Deck(s) : ";
					cin >>deck;
				}
			}
		}
	}
}
void one_play()							//Table MENU
{
	char choice1='z';					//init loop val
	int result=0;						//init result indic
	srand(time(NULL));					//seed a rnd num
	while (choice1!='L')	//while not Leave...
	{
		system("cls");		//cls
		choice1='z';		//reset choice
		cout<<"Played : "<<games_played<<"\n"
			<<"Won  : "<<games_won<<"\n"
			<<"Lost  : "<<games_lost<<"\n"
			<<"__________________________"
			<<"___________________________"
			<<"___________________________\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n"
			<<"  (D)eal Next Hand\n"
			<<"  (L)eave Table\n  ";
		while(choice1!='D'&&choice1!='L')
		{
			cin >>choice1;
			choice1=toupper(choice1);
		}
		if (choice1=='D')			//Choice Deal Cards
		{
			result=deal_card1();	//return 1=WIN 0=LOSE
			if (result==1) games_won++;
			else if (result==0) games_lost++;
			games_played++;
		}
	}
}
int deal_card1()
{
redeal:
//-------------------CARD SEEDING--------------------------
	for (int k=1;k<=5;k++)
	{
		c_card[k]=0;					//reset cards
		p_card[k]=0;					//
		c_open[k]=0;					//
		p_open[k]=0;					//
		dealt[k]=0;						//
		dealt[(k+5)]=0;					//
	}
	for (k=1;k<=5;k++)						//COMP's cards
	{
		c_card[k]=(1+ rand() % (deck*52));	//seed card
		for (int l=1;l<=10;l++)
		{
			if (c_card[k]==dealt[l])		//check for duplicate
			{
				k--;						//if dupe, redeal
				break;
			}
			if (l==10)
			{
				dealt[k]=c_card[k];			//if no dupe, store
				break;
			}
		}
	}
	for (k=1;k<=5;k++)						//PLAYER's cards
	{
		p_card[k]=(1+ rand() % (deck*52));	//seed card
		for (int x=1;x<=10;x++)
		{
			if (p_card[k]==dealt[x])		//check for duplicate
			{
				k--;						//if dupe, redeal
				break;
			}
			if (x==10)
			{
				dealt[k+5]=p_card[k];			//if no dupe, store
				break;
			}
		}
	}
//-------------------END CARD SEEDING----------------------
showcard:
	int sum=0;				//for score addition
	int sum1=0;				//comp's score addition
	int BJ=0;				//blackjack results Indicator
	char card_choice='z';	//loop for choice
	int end_game=0;			//game end indic
	p_open[1]=1;			//reveal init 2 player's cards
	p_open[2]=1;			//
	while (end_game!=1)		//Infinite Loop until game ends
	{
		system("cls");		//cls
		cout<<"Played : "<<games_played<<"\n"
			<<"Won  : "<<games_won<<"\n"
			<<"Lost  : "<<games_lost<<"\n"
			<<"__________________________"
			<<"___________________________"
			<<"___________________________\n\n"
			<<"Sarojini ";						//Card Tile
		if (end_game==2)
		{
			int caption;
			caption=(1+rand()%8);
			switch (BJ)
			{
			case 1:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"I'm the BEST lecturer!";
				if (caption==2) cout<<"MUAHAHAHAHHA!";
				if (caption==3) cout<<"None can Stop ME!";
				if (caption==4) cout<<"I told you so!";
				if (caption==5) cout<<"You cant deny me!";
				if (caption==6) cout<<"All shall be MINE!";
				if (caption==7) cout<<"You are good, but I am better!";
				if (caption==8) cout<<"This is not your day ...";
				break;
			case 2:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"You learn fast";
				if (caption==2) cout<<"You suprise me everday";
				if (caption==3) cout<<"You deserve 100% for assignment";
				if (caption==4) cout<<"Oh dear!";
				if (caption==5) cout<<"You're just lucky ...";
				if (caption==6) cout<<"You're not suppose to win me! ";
				if (caption==7) cout<<"Not FAIR!";
				if (caption==8) cout<<"Hmmph!";
				break;
			case 3:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"My, my ... aren't we greedy?";
				if (caption==2) cout<<"Be patient!";
				if (caption==3) cout<<"You've got run-time errors";
				if (caption==4) cout<<"I didn't teach you that!";
				if (caption==5) cout<<"Please pay attention!";
				if (caption==6) cout<<"You didn't listen to my lectures?";
				if (caption==7) cout<<"You din't do homework, did you?";
				if (caption==8) cout<<"Don't commit plagiarism!";
				break;
			case 4:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"I was careless";
				if (caption==2) cout<<"Opps!";
				if (caption==3) cout<<"Oh #@%*!";
				if (caption==4) cout<<"Almost had you!";
				if (caption==5) cout<<"Argh! I'll get you next round";
				if (caption==6) cout<<"Crap... more homework for everyone!";
				if (caption==7) cout<<"That was close";
				if (caption==8) cout<<"I was hoping to get 21";
				break;
			case 5:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"You can never beat me!";
				if (caption==2) cout<<"Learn from me ...";
				if (caption==3) cout<<"You think that was enough?";
				if (caption==4) cout<<"I'll always be the winner";
				if (caption==5) cout<<"Ha! Nice try!";
				if (caption==6) cout<<"You cant win me without cheating";
				if (caption==7) cout<<"You disappoint me";
				if (caption==8) cout<<"Nyah! Nyah!";
				break;
			case 6:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"Impressive!";
				if (caption==2) cout<<"I cannot believe...";
				if (caption==3) cout<<"oOOOo... 5 streak";
				if (caption==4) cout<<"5 cards... good";
				if (caption==5) cout<<"You took too much risk";
				if (caption==6) cout<<"That was risky!";
				if (caption==7) cout<<"Not bad at all!";
				if (caption==8) cout<<"Excellent! That was good!";
				break;
			case 7:
				cout<<"("<<sum1<<") : ";
				if (caption==1) cout<<"What a waste ...";
				if (caption==2) cout<<"Bummer!";
				if (caption==3) cout<<"A draw, I can live with that";
				if (caption==4) cout<<"Let's try again";
				if (caption==5) cout<<"That was a close one!";
				if (caption==6) cout<<"Haw! What a coincidence";
				if (caption==7) cout<<"Just great!";
				if (caption==8) cout<<"Your skills are on par with mine";
				break;
			}
		}
		//-------------
		cout<<"\n\tÉÍÍÍÍÍ»"<<"\tÉÍÍÍÍÍ»";				//1st Line
		for (int g=3;g<=5;g++)
		{
			if (c_open[g]==1) cout<<"\tÉÍÍÍÍÍ»";
		}
		cout<<endl;
		//-------------
		for (g=1;g<=5;g++)							//2nd Line
		{
			if (c_open[g]==1)
			{
				cout<<"\tº";
				if(convert_num(c_card[g])=='0')
				{
					cout<<setw(5)<<setiosflags(ios::left)<<"10";
				}
				else
				{
					cout<<setw(5)<<setiosflags(ios::left)<<convert_num(c_card[g]);
				}
				cout<<"º";
			}
			else
			{
				if (g==1||g==2) cout<<"\tº   º";
			}
		}
		//--------------
		for (g=1;g<=3;g++)
		{
			cout<<"\n\tº   º\tº   º";			//Other Lines
			if (c_open[3]==1) cout<<"\tº   º";	//
			if (c_open[4]==1) cout<<"\tº   º";	//
			if (c_open[5]==1) cout<<"\tº   º";	//
		}
		cout<<"\n\tÈÍÍÍÍÍ¼\tÈÍÍÍÍÍ¼";
		if (c_open[3]==1) cout<<"\tÈÍÍÍÍÍ¼";	//6th Line
		if (c_open[4]==1) cout<<"\tÈÍÍÍÍÍ¼";	//
		if (c_open[5]==1) cout<<"\tÈÍÍÍÍÍ¼";	//
		cout<<"\n\nYour Hand ";					//PLAYER's HAND
		if (end_game==2)
		{
			switch (BJ)
			{
			case 1:
				cout<<"("<<sum<<") : You lost.";break;
			case 2:
				cout<<"("<<sum<<") : BlackJack! You Won!";break;
			case 3:
				cout<<"("<<sum<<") : You Lost.";break;
			case 4:
				cout<<"("<<sum<<") : You Won!";break;
			case 5:
				cout<<"("<<sum<<") : You Lost.";break;
			case 6:
				cout<<"("<<sum<<") : You Won!.";break;
			case 7:
				cout<<"("<<sum<<") : Draw Game.";break;
			}
		}
		//-------------
		cout<<"\n\tÉÍÍÍÍÍ»"<<"\tÉÍÍÍÍÍ»";				//1st Line
		for (g=3;g<=5;g++)
		{
			if (p_open[g]==1) cout<<"\tÉÍÍÍÍÍ»";
		}
		cout<<endl;
		//-------------
		for (g=1;g<=5;g++)							//2nd Line
		{
			if (p_open[g]==1)
			{
				cout<<"\tº";
				if(convert_num(p_card[g])=='0')
				{
					cout<<setw(5)<<setiosflags(ios::left)<<"10";
				}
				else
				{
					cout<<setw(5)<<setiosflags(ios::left)<<convert_num(p_card[g]);
				}
				cout<<"º";
			}
		}
		//--------------
		for (g=1;g<=3;g++)
		{
			cout<<"\n\tº   º\tº   º";			//Other Lines
			if (p_open[3]==1) cout<<"\tº   º";	//
			if (p_open[4]==1) cout<<"\tº   º";	//
			if (p_open[5]==1) cout<<"\tº   º";	//
		}
		cout<<"\n\tÈÍÍÍÍÍ¼\tÈÍÍÍÍÍ¼";
		if (p_open[3]==1) cout<<"\tÈÍÍÍÍÍ¼";	//6th Line
		if (p_open[4]==1) cout<<"\tÈÍÍÍÍÍ¼";	//
		if (p_open[5]==1) cout<<"\tÈÍÍÍÍÍ¼";	//
		//---------------PLAYER'S CHOICE-------------
		if (end_game==2)
		{
			cout<<"\n  (D)eal Next Set\n  ";
			card_choice='z';
			while (card_choice!='D')
			{
				cin >>card_choice;
				card_choice=toupper(card_choice);
			}
			end_game=1;
			if (BJ==1||BJ==3||BJ==5) return 0;
			else if (BJ==2||BJ==4||BJ==6) return 1;
			else if (BJ==7) return 2;
		}
		else
		{
			cout<<"\n  (D)raw Card\n";
			push=0;
			for (int q=1;q<=2;q++)
			{
				int face=((p_card[q]%52)%13);
				if (face>0&&face<10) push+=face;		//NORMAL CARDS
				if (face>9||face==0) push+=10;		//10,J,Q,K
			}
			if (push==15&&p_open[3]==0) cout<<"  (P)ush\n";
			cout<<"  (E)nough\n  ";
			card_choice='z';
			while(card_choice!='D'&&card_choice!='E'&&card_choice!='P')
			{
				cin >>card_choice;
				card_choice=toupper(card_choice);
			}
			if (card_choice=='P'&&push==15&&p_open[3]==0) goto redeal;	//Push
			if (card_choice=='P'&&push==15&&p_open[3]==0) goto showcard;
			if (card_choice=='D')			//Draw a card
			{
				if (p_open[4]==1 && p_open[5]==0) p_open[5]=1;
				if (p_open[3]==1 && p_open[4]==0) p_open[4]=1;
				if (p_open[3]==0) p_open[3]=1;
			}
			if (card_choice=='E')
			{
				int ace=0;
				for (int k=1;k<=5;k++)
				{
					int face=((p_card[k]%52)%13) ;
					if (p_open[k]==1)
					{
						if (face>1&&face<10) sum+=face;		//NORMAL CARDS
						if (face>9||face==0) sum+=10;		//10,J,Q,K
						if (face==1)
						{
							ace=1;
							sum++;
						}
					}
				}
				if (ace==1&&sum<=11)
				{
					sum+=10;
					if (p_open[3]==0&&sum==21) {BJ=2;}
				}
				if (sum>21) BJ=3;
//----------------------------------
				if (BJ!=3&&BJ!=2)
				{
					int ace=0;
					for (int k=1;k<=5;k++)
					{
						if (p_open[1]==1&&p_open[2]==1&&p_open[3]==1&&p_open[4]==1&&p_open[5]==1) {BJ=6;break;}
						if (sum1==sum&&sum1>u_limit) {BJ=7;break;}
						if (sum1>sum&&sum1<=21) {BJ=5;break;}
						if (sum1>21) {BJ=4;break;}
						if (sum1<sum||(sum1==sum&&sum<=u_limit))
						{
							c_open[k]=1;
							int face=((c_card[k]%52)%13) ;
							if (face>1&&face<10) sum1+=face;		//NORMAL CARDS
							if (face>9||face==0) sum1+=10;		//10,J,Q,K
							if (face==1)
							{
								ace++;
								sum1++;
							}
							if (ace==1&&sum1<=11&&c_open[2]==1)
							{
								sum1+=10;
								ace=0;
								if (c_open[3]==0&&sum1==21) {BJ=1;break;}
							}
						}
						if (sum1>21) {BJ=4;break;}
						if (sum1>sum&&sum1<=21) {BJ=5;break;}
					}
				}
				end_game=2;		//shows table one more time
			}
		}
	}
	//BJ 1 = Comp BJ
	//  2 = Play BJ
	//  3 = Play OVER
	//  4 = Comp OVER
	//	 5 = Comp Win
	//  6 = PLay Win
	//  7 = Draw
return 0;
}
char convert_num(int card)
{
	char num='0';
	card=(card%52);			//deck division
	card=(card%13);			//suit division
	switch (card)
	{
	case 1:
		num='A';break;
	case 2:
		num='2';break;
	case 3:
		num='3';break;
	case 4:
		num='4';break;
	case 5:
		num='5';break;
	case 6:
		num='6';break;
	case 7:
		num='7';break;
	case 8:
		num='8';break;
	case 9:
		num='9';break;
	case 10:
		num='0';break;
	case 11:
		num='J';break;
	case 12:
		num='Q';break;
	case 0:
		num='K';break;
	}
	return num;			//return value
}
```

