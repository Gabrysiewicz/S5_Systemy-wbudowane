#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>

#define LCD PORTA
#define EN 7
#define RW 6
#define RS 5

void lcdcmd(unsigned char cmd){
    PORTD &= ~(1<<RS);   // RS = 0 for command mode
    // 1                 = 00000 0001
    // 1<<RS             = 0010 0000
    // ~(1<<RS)          = 1101 1111
    // PORTD             = xxxx xxxx
    //                     1101 1111
    // PORTD &= ~(1<<RS) = xx0x xxxx
    PORTD &= ~( 1 << RW );      // RW = 0 for write
    LCS = cmd & 0xF0;           // Send high [v]
    PORTD |=  ( 1 << EN );      // EN = 1; High to Low pulse
    _delay_ms(1);
    PORTD &=  ~( 1 << EN );     // EN = 0; High to Low pulse
    LCD = cmd << 4;             // Send low [v]
    PORTD |= ( 1 << EN );       // EN = 1; High to Low pulse    
    _delay_ms(1);

    PORTD &= ~( 1 << EN );
}

void lcddata(unsigned char data){
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
    lcdcmd(0x33);
    lcdcmd(0x32);
    lcdcmd(0x28);               // LCD 4 bit mode
    lcdcmd(0x0E);               // display on cursor
    lcdcmd(0x01);               // clear LCD
    _delay_ms(2);
}

void lcd_print(char *str){
    unsigned char i = 0;
    while( str[i] != 0 ){
        lcddata( str[i] );
        i++;
    }
}

int main(void){
    lcd_init();
    lcd_print("Hello world");
    while(1){
        
    }
    return 0;
}
