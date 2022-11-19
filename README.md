<h2>
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
</h2>

<h3> 1. Odpowiedź na pytanie kontrolne 1 </h3>
Pytanie 1. Jak należy skonfigurować port USART do trybu nadawanie z prędkością 9600 bo-dów (bps) dla F_CPU= 4 MHz.

```
#define F_CPU 4000000UL					// Ustawienie częstotliwości taktowania procesora na 4 MHz
#define BAUDRATE 9600 						// Ustawienie prędkiści (baud) dla programu Putty
#define BAUD_PRESCALLER (((F_CPU / (BAUDRATE * 16UL))) -1)      // Wyznaczenie  zawartości rejestru UBRR 
```
Wewnątrz funkcji inicjalizującej
```
UBRRL = (uint8_t)(BAUD_PRESCALLER);
```

<h3> 2. Odpowiedź na pytanie kontrolne 2 </h3>
Pytanie 2. W jaki sposób określić moment wysłania kolejnej danej poprzez port USART.

```
while(! (UCSRA & ( 1 << UDRE ) ) );  			/* Oczekiwanie na opróżnienie buffera przekazującego*/
```

<h3> Część praktyczna: </h3>

<h4> Kod zadania 1 </h4>

```
#define F_CPU 8000000UL						// Ustawienie częstotliwości taktowania procesora na 8 MHz
#define BAUDRATE 9600 						// Ustawienie prędkiści (baud) dla programu Putty
#define BAUD_PRESCALLER (((F_CPU / (BAUDRATE * 16UL))) -1)      // Wyznaczenie  zawartości rejestru UBRR 
#include <avr/io.h>						// Wczytanie biblioteki standardowej
#include <util/delay.h>						// _delay_ms()

// USART
void usart_init(void){					        // Funkcja Inicjalizacji portu USART
	// Konfiguracja... (baud rate = 9600, ramka: 8 bitów danych, brak kontroli parzystości, 1 bit stopu)
	UBRRH = (uint8_t)(BAUD_PRESCALLER>>8);
	UBRRL = (uint8_t)(BAUD_PRESCALLER);
	
	UCSRB = ( 1 << RXCIE ) | ( 1 << RXEN ) |  ( 1 << TXEN );
	
	UCSRC = ( 1 << URSEL ) | ( 1 << UCSZ0 ) | ( 1 << UCSZ1);
}
unsigned char usart_RxChar(){
	while( ( UCSRA & ( 1 << RXC ) ) == 0 );			/* Oczekiwanie na przyjęcie danych */
	return(UDR);						/* Zwrócenie bitu */
}
void usart_TxChar(unsigned char znak){
	while(! (UCSRA & ( 1 << UDRE ) ) );  			/* Oczekiwanie na opróżnienie buffera przekazującego*/
	UDR = znak;						// Przypsanie pobranej wartości do rejestru nadawczo odbiorczego UDR
}
/* Alternatywna wersja
void usart_TxChar(){
	while( (UCSRA & ( 1 << UDRE ) ) == 0 );  		// Oczekiwanie na opróżnienie buffera przekazującego
	return(UDR);						// Przypsanie pobranej wartości do rejestru nadawczo odbiorczego UDR
}
*/
void usart_string(char *str){					// Funkcja wyświetlająca zadany łańcuch znaków na ekran Putty
	unsigned char i = 0;
	_delay_ms(10);						// Delay zapobiegający problemom z wysłaniem pierwszego znaku (czasem sie zdarza)
	while( str[i] != 0 ){					// Wykonuj dopuki nie natrafisz na koniec łańcucha znaków
		usart_TxChar( str[i] );				// Wywołanie ww. funkcji wyświetlającej jeden znak
		_delay_ms(10);					// Dajemy czas procesorowi i rejestrom aby te poprawnie zapisały/odczytały dane
		i++;
	}
}
int main(void){
	usart_init();						// Wywołanie ww. funkcji inicjalizującej
	usart_string("Kamil Gabrysiewicz");			// Wysłánie Imienia i Nazwiska na terminal Putty
	while(1){
	}
	return 0;
}

```
<img src='https://github.com/Gabrysiewicz/Systemy-wbudowane/blob/sprawozdanie/Zadanie1.png' width=1000>
 

<h4> Kod zadania 2 </h4> 
<h5> Zdjęcie pulpitu z PC z wyświetlonymi komunikatami, opis słowny działania programu (2-3 zdania) </h5> 

<h6> W moim przypadku fragment kodu odpowiedzialny za pobieranie danych nie działał. Cięzko powiedzieć czym mogło to być spowodowane, próbowałem na wiele sposobów. Np. umieszczałem fragmenty kodu w głównej pętli, modyfikowałem Putty, podmieniałem fragmenty kodu na inne, korzystałem z klawiatury ekranowej, ale wciąż u mnie pobranie znaku nie działało. Być może było to spowodowałem tym że AVR był podpięty do komputera przez USB w monitorze (COM4) ale i tak ciężko to stwierdzić. Zapomniałem też o zrobienu screena, ponieważ odczyt nie działał a wyświetlanie nie różniło się od zadania poprzedniego.</h6>

```
int main(void){
	usart_init();					// Wywołanie ww. funkcji inicjalizującej 
	usart_string("Nacisnij klawisz [x]");		// Wysłánie polecenia na terminal Putty
	char userChar = usart_RxChar();			// Pobranie znaku od użytkownika i zapis do zmiennej
	if(userChar == 'x'){				// Sprawczenie poprawnośći otrzynamego znaku
		usart_string("Good");
	}else{
		usart_string("Bad");
	}
	while(1){	
	}
	return 0;
}
```

<h4> Kod zadania 3 </h4>
<h6> Niestety ze względu na komplikacje i powikłania w zadaniu 2 nie udało mi się tu dotrzeć</h6>
