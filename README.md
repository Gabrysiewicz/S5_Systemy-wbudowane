```
#define F_CPU 8000000UL
#define BAUDRATE 9600 
#define BAUD_PRESCALLER (((F_CPU / (BAUDRATE * 16UL))) -1) 
#include <avr/io.h>
#include <util/delay.h>

// USART
void usart_init(void){
	UBRRH = (uint8_t)(BAUD_PRESCALLER>>8);
	UBRRL = (uint8_t)(BAUD_PRESCALLER);
	
	UCSRB = ( 1 << RXCIE ) | ( 1 << RXEN ) |  ( 1 << TXEN );
	
	UCSRC = ( 1 << URSEL ) | ( 1 << UCSZ0 ) | ( 1 << UCSZ1);
}
unsigned char usart_RxChar(){
	while( ( UCSRA & ( 1 << RXC ) ) == 0 );/* Wait till data is received */
	return(UDR);							/* Return the byte */
}
void usart_TxChar(unsigned char znak){
	while(! (UCSRA & ( 1 << UDRE ) ) );  /* Wait for empty transmit buffer */
	UDR = znak;
}
void usart_string(char *str){
	unsigned char i = 0;
	while( str[i] != 0 ){
		usart_TxChar( str[i] );
		_delay_ms(10);
		i++;
	}
}
int main(void){
	usart_init();
	usart_string("Kamil Gabrysiewicz");
	while(1){
		
		
		
		
	}
	return 0;
}


```

```
int main(void){
	usart_init();
	usart_string("Nacisnij klawisz [x]");
	char userChar = usart_RxChar();
	if(userChar == 'x'){
		usart_string("Good");
	}else{
		usart_string("Bad");
	}
	while(1){
		
		
	}
	return 0;
}
```
