```
#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>

#define LCD PORTA
// PA4 -> D4
// PA5 -> D5
// PA6 -> D6
// PA7 -> D7
#define EN 7
// PD7 -> E
#define RW 6
// BUILT-IN GROUNDED
#define RS 5
// PD5 -> RS

void lcd_cmd(unsigned char cmd){
	PORTD &= ~(1<<RS);   // RS = 0 for command mode
	// 1                 = 00000 0001
	// 1<<RS             = 0010 0000
	// ~(1<<RS)          = 1101 1111
	// PORTD             = xxxx xxxx
	//                     1101 1111
	// PORTD &= ~(1<<RS) = xx0x xxxx
	PORTD &= ~( 1 << RW );      // RW = 0 for write
	LCD = cmd & 0xF0;           // Send high [v]
	PORTD |=  ( 1 << EN );      // EN = 1; High to Low pulse
	_delay_ms(1);
	PORTD &=  ~( 1 << EN );     // EN = 0; High to Low pulse
	LCD = cmd << 4;             // Send low [v]
	PORTD |= ( 1 << EN );       // EN = 1; High to Low pulse
	_delay_ms(1);

	PORTD &= ~( 1 << EN );
}

void lcd_data(unsigned char data){
	PORTD |=  (1 << RS);        // RS = 1 for data
	PORTD &= ~(1 << RW);        // RW = 0 for write
	LCD = data & 0xF0;          // send high [v]
	PORTD |=  (1 << EN);        // EN = 1; High to Low pulse
	_delay_ms(1);
	PORTD &= ~(1 << EN);        // EN = 0; High to Low pulse
	LCD = data << 4;            // Send low [v]
	PORTD |=  (1 << EN);        // EN = 1; High to Low pulse
	_delay_ms(1);
	PORTD &= ~(1 << EN);
}

void lcd_init(){
	DDRA = 0xff;                // output port
	DDRD = 0xff;                // define RS, EN, RW as output
	PORTD &= ~(1 << EN);        // initialize EN = 0
	lcd_cmd(0x33);
	lcd_cmd(0x32);
	lcd_cmd(0x28);               // LCD 4 bit mode
	lcd_cmd(0x0E);               // display on cursor
	lcd_cmd(0x01);               // clear LCD
	_delay_ms(2);
}

void lcd_write(char *str){
	unsigned char i = 0;
	while( str[i] != 0 ){
		lcd_data( str[i] );
		i++;
	}
}
void lcd_clear(){
	lcd_init();
	for(int i = 0; i < 16; i++){
		lcd_write(" ");
		_delay_ms(1);
	}
}
void lcd_clear2(){
	lcd_cmd(0x01);
}

int main(void){
	
	while(1){
		lcd_init();
		lcd_write("Hello World");
		_delay_ms(100);
		lcd_clear2();
		_delay_ms(100);
	}
	return 0;
}


```
