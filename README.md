<table>
<thead><tr>
	<th colspan=3> 
	 Laboratorium Systemów Wbudowanych 
	 <img width='1000' height=1> </th>
	</tr>
<thead>
	<tbody>
	<tr><td> 16.11.2022 r. </td><td>Kamil Gabrysiewicz 95400</td><td>3.5 SE</td></tr>
	</tbody>
</table>

<h1> 1. Odpowiedź na pytanie kontrolne 1 </h1>

 

<h1> 2. Odpowiedź na pytanie kontrolne 2 </h1>

<h1> Część praktyczna: </h1>

1. Kod zadania 1.
```
#define F_CPU 8000000UL						/	/ Ustawienie częstotliwości taktowania procesora na 8 MHz
#define BAUDRATE 9600 							// Ustawienie prędkiści (baud) dla programu Putty
#define BAUD_PRESCALLER (((F_CPU / (BAUDRATE * 16UL))) -1)      	// Wyznaczenie  zawartości rejestru UBRR 
#include <avr/io.h>							// Wczytanie biblioteki standardowej
#include <util/delay.h>							// _delay_ms()

// USART
void usart_init(void){							// Funkcja Inicjalizacji portu USART
	// Konfiguracja... (baud rate = 9600, ramka: 8 bitów danych, brak kontroli parzystości, 1 bit stopu)
	UBRRH = (uint8_t)(BAUD_PRESCALLER>>8);
	UBRRL = (uint8_t)(BAUD_PRESCALLER);
	
	UCSRB = ( 1 << RXCIE ) | ( 1 << RXEN ) |  ( 1 << TXEN );
	
	UCSRC = ( 1 << URSEL ) | ( 1 << UCSZ0 ) | ( 1 << UCSZ1);
}
unsigned char usart_RxChar(){
	while( ( UCSRA & ( 1 << RXC ) ) == 0 );				/* Oczekiwanie na przyjęcie danych */
	return(UDR);							/* Zwrócenie bitu */
}
void usart_TxChar(unsigned char znak){
	while(! (UCSRA & ( 1 << UDRE ) ) );  				/* Oczekiwanie na opróżnienie buffera przekazującego*/
	UDR = znak;							// Przypsanie pobranej wartości do rejestru nadawczo odbiorczego UDR
}
void usart_string(char *str){						// Funkcja wyświetlająca zadany łańcuch znaków na ekran Putty
	unsigned char i = 0;
	_delay_ms(10);							// Delay zapobiegający problemom z wysłaniem pierwszego znaku (czasem sie zdarza)
	while( str[i] != 0 ){						// Wykonuj dopuki nie natrafisz na koniec łańcucha znaków
		usart_TxChar( str[i] );					// Wywołanie ww. funkcji wyświetlającej jeden znak
		_delay_ms(10);						// Dajemy czas procesorowi i rejestrom aby te poprawnie zapisały/odczytały dane
		i++;
	}
}
int main(void){
	usart_init();							// Wywołanie ww. funkcji inicjalizującej
	usart_string("Kamil Gabrysiewicz");				// Wysłánie Imienia i Nazwiska na terminal Putty
	while(1){
	}
	return 0;
}

```

 

2. Kod zadania 2., Zdjęcie pulpitu z PC z wyświetlonymi komunikatami, opis słowny działania programu (2-3 zdania). 

 

3. Kod zadania 3, opis działania programu (2-3 zdania).  
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
