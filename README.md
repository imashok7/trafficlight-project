# trafficlight-project
Four way traffic light System using Keil Software.

Code in Keil:

#include <at89x51.h>
static int sec =0;
static int time = 0;



void sEOS_init_timer2(){
	TCON=0x01;
	TH0=0xd8;
	TL0=0xef;
	//RCAP2H=0xfc;
	//RCAP2L=0x18;
	ET0=1;
	TR0=1;
	EA=1;
}

 void reload_timer()
 {
 	TR0=0;
	TH0=0xfc;
	TL0=0x18;
	TR0=1;

 }
void sEOS_ISR(void) interrupt 1{
	
	sec++;
	if(sec%100==0){
	sec=0;
	P3_0 = ~P3_0;
	time ++;
	}
	switch(time){
		case 11: case 0:{
			P1=0x12;
	    	P2=0x24;
		} break;
		case 12: {
		   P1=0x09;
	       P2=0x24;
		} break;

		case 13: case 17:{
			P1=0x24;
			P2=0x14;
		}break;
		
		case 18: {
		 	P1=0x24;
			P2=0x0c;
		}break;

	   case 19: case 23: {
	   		P1=0x24;
			P2=0x22;
		}break;

		case 24: {
			P1=0x24;
            P2=0x21;	
		}break;
		case 25:{
		   time=0;
		  sec =0;
		}	break;


	}		

}

void sEOS_sleep(){
	PCON |=0x01;
}

void main(void){
	
	P1=0xff;
	P2=0xff;
	P3_0=1;
	sEOS_init_timer2();
	while(1)
	{
		sEOS_sleep();
	}	
}
