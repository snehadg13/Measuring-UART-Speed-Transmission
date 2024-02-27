header.h
--------------
typedef unsigned int u32;
typedef signed int s32;
typedef unsigned char u8;
typedef signed char s8;

extern void uart0_tx_string(char * ptr);
extren void uart0_init(u32 baud);
extern u8 uart0_rx(void);
extern void uart0_tx(u8 data);

main()
----------------------------------------------------------------------------------------------------------------------------------------------
#include"header.h"
main()
{
u8 temp;
uart0_init(9600);
uart0_tx_string ("Finance Minister Arun Jaitley Tuesday hit out at former RBI governor Raghuram Rajan for predicting that 	the next banking crisis would be triggered by MSME lending, saying postmortem is easier than taking action when it was required. Rajan, who had as the chief economist at IMF warned of impending financial crisis of 2008, in a note to a parliamentary committee warned against ambitious credit targets and loan waivers, saying 	that they could be the sources of next banking crisis. Government should focus on sources of the next crisis, not just the last one. In particular, government should refrain from setting ambitious credit targets or waiving loans. Credit targets are sometimes achieved by abandoning appropriate due diligence, creating the environment for future NPAs," Rajan said in the note." Both MUDRA loans as well as the Kisan Credit Card, while popular, have to 	be examined more closely for potential credit risk. Rajan, who was RBI governor for three years till September 2016, is currently");

	while(1)			///uart0 loop back///
	{
	temp=uart0_rx();
	uart0_tx(temp);
	}
}

uart0_driver.c
---------------------------------------------------------------------------------------------------------------------------------------------
#include<lpc21xx.h>
#include"header.h"

s32 m[]={15,60,30,0,15};
void uart0_init(u32 baud)		///uart0 baud setting function///
{
	u32 temp=0, pclk=0;
	pclk=m[VPBDIV]*1000000;
	
	if(VPBDIV!=3)
	temp(pclk/(16*baud));

	PINSEL0| = 0x5;
	U0LCR = 0x83;
	U0DLL= temp&0xFF;
	U0DLM = (temp>>8)&0xFF;
	U0LCR = 0x03;
}

void uart0_tx_string(char * ptr)	///uart0 string transfer function///
{
	while(*ptr!=0)
	{
		U0THR=*ptr;
		while((U0LSR>>5)&1);
		ptr++;
	}
}

void uart0_tx(u8 data)		///uart0  transfer function///
{
	U0THR=data;
	while(((U0LSR>>5)&1)==0);
}

u8 uart0_rx(void)			///uart0 recv function///
{
	while((U0LSR&1)==0);
	return U0RBR;
}




