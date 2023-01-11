# SAM7

Button
```
#include <targets/AT91SAM7.h>
void delay(int ms){
  int a;
  int b;
  for(a = 0; a <= ms; a++){
    for(b = 0; b <= 3000; b++){
      __asm__("NOP");
    }
  }
}

main(){
  PMC_PCER = 1 << 3; // zalaczenie zegara

  PIOB_PER = 1 << 20; // kontrola kontrolera I/O PIN 65 Kontrolowany przez linie 20
  PIOB_PER |= 1 << 24; // kontrola kontrolera I/O PIN 65 Kontrolowany przez linie 24

  PIOB_OER = 1 << 20; // linia 20 PORT B => wyjscie
  PIOB_ODR = 1 << 24; // linia 20 PORT B => wejscie
  PIOB_SODR = 1 << 20; // stan lini 20 PORT B => wysokie napiecie // zapalenie LED
  // PIOB_CODR = 1 << 20; // stan lini 20 PORT B => niskie napiecie // zgaszenie LED
  // 70 SW1 => PB24
  // 71 SW2 => PB25
  while(1){
    if( ( PIOB_PDSR & 1 << 24) == 0){ // sprawdzenie czy przycisk SW1 został wciśnięty
      PIOB_CODR = 1 << 20;
    }
    
    if( ( PIOB_PDSR & 1 << 25) == 0){ // sprawdzenie czy przycisk SW1 został wciśnięty
      PIOB_SODR = 1 << 20;
    }
  }

}
```

Switch
```
#include <targets/AT91SAM7.h>
void delay(int ms){
  int a;
  int b;
  for(a = 0; a <= ms; a++){
    for(b = 0; b <= 3000; b++){
      __asm__("NOP");
    }
  }
}

main(){
  PMC_PCER = 1 << 3; // zalaczenie zegara

  PIOB_PER = 1 << 20; // kontrola kontrolera I/O PIN 65 Kontrolowany przez linie 20
  PIOB_PER |= 1 << 24; // kontrola kontrolera I/O PIN 65 Kontrolowany przez linie 24

  PIOB_OER = 1 << 20; // linia 20 PORT B => wyjscie
  PIOB_ODR = 1 << 24; // linia 20 PORT B => wejscie
  
  PIOB_SODR = 1 << 20; // stan lini 20 PORT B => wysokie napiecie // zapalenie LED
  // PIOB_CODR = 1 << 20; // stan lini 20 PORT B => niskie napiecie // zgaszenie LED
  // 70 SW1 => PB24
  // 71 SW2 => PB25
  PIOB_OWER = 1 << 20;
  while(1){
    if( ( PIOB_PDSR & 1 << 24) == 0){ // sprawdzenie czy przycisk SW1 został wciśnięty
      PIOB_ODSR ^= 1 << 20;
      while((PIOB_PDSR & 1 << 24) == 0){
        delay(20);
      }
    }
  }
}
```

Clock
```
#include <targets\AT91SAM7.h>
#include "pcf8833u8_lcd.h"

int main (){

  InitLCD();
  SetContrast(25);
  LCDClearScreen();

  LCDPutStr("Kamil", 10, 0, LARGE, YELLOW, BLACK);
  LCDPutStr("Gabrysiewicz", 20, 0, LARGE, YELLOW, BLACK);
  LCDSetRect(40, 0, 40, 150, 0, BLACK);
  LCDSetRect(50, 50, 100, 100, 0, WHITE);

  int hour = 0;
  int min = 0;
  char hourCh[4];
  char minCh[4];
  while(1){
    Delay_(1000000);
    if(min == 59){
    LCDPutStr("XX", 50, 70, LARGE, WHITE, WHITE);
      if(hour == 23){
        hour = 0;
        min = 0;
        LCDPutStr("XX", 50, 40, LARGE, WHITE, WHITE);
      }else{
        hour++;
        min = 0;
      }
    }else{
      min++;
    }
    itoa(hour, hourCh, 10);
    itoa(min, minCh, 10);
    LCDPutStr(hourCh, 50, 40, LARGE, RED, WHITE);
    LCDPutChar(':', 50, 60, LARGE, RED, WHITE);
    LCDPutStr(minCh, 50, 70, LARGE, RED, WHITE);  
  }
}
```
Timer
```
// Zaproponuj kod programu, w którym bez stosowania przerwaoza pomocą programowego odczytu rejestru TC0_SR, 
// zmienisz stan linii 20 portu B (na płytce ewaluacyjnej dostępnej w laboratorium: LCD_BL) z częstotliwością 10 Hz 
// (zmiana stanu podświetlenia LCD co 100 ms). MCK=48MHz. UŻYJ NAJMNIEJSZEGO MOŻLIWEGO DZIELNIKA CZĘSTOTLIWOŚCI MCK. 
#include <targets\AT91SAM7.h>
#include <ctl_api.h>
#include <string.h>

int main(void){
  int i = 0;
  int limit = 2000000;

  /* LCD */
  PMC_PCER = 1 << 3;      // zalaczenie zegara

  PIOB_PER = 1 << 20;     // kontrola kontrolera I/O PIN 65 Kontrolowany przez linie 20
  PIOB_PER |= 1 << 24;    // kontrola kontrolera I/O PIN 65 Kontrolowany przez linie 24

  PIOB_OER = 1 << 20;     // linia 20 PORT B => wyjscie
  PIOB_ODR = 1 << 24;     // linia 20 PORT B => wejscie
  
  PIOB_SODR = 1 << 20;    // stan lini 20 PORT B => wysokie napiecie // zapalenie LED
  // PIOB_CODR = 1 << 20; // stan lini 20 PORT B => niskie napiecie // zgaszenie LED
  
  PIOB_OWER = 1 << 20;
  /* TIMER */
  PMC_PCER = 0;
  PIOB_OER = 1 << 20;
  PIOB_OWER = 1 << 20;
  TC0_CCR = TC0_CCR_CLUDIS;
  TC0_CMR = 3 | 1 << 14;
  TC0_RC = 37500;
  TC0_CCR = 1 << 2 | 1 << 0;
  while(1){
    if( TCO_SR & 1 << 4){
        PIOB_ODSR ^= 1 << 20;
    }
//    i++;
//    if( i >= limit){
//      i = 0;
//      PIOB_ODSR ^= 1 << 20;
//    }
  }
}
```

TEMPERATURA

```
#include <targets\AT91SAM7.h>
#include "pcf8833u8_lcd.h"
#define SWRST 0
int main (){

  InitLCD();
  SetContrast(30);
  LCDClearScreen();

  LCDPutStr("..............", 10, 20, LARGE, BLACK, WHITE);
  
  // The programmer must first enable the PWM clock in the Power Management Controller (PMC) before using the PWM
//  PMC_PCER = 1 << 2; // PIOA
  PMC_PCER = 1 << 3; // PIOB; DATASHEET page.30 

  PMC_PCER = 1 << 17; // ADC; DATASHEET page.30
  ADC_CR = 1 << SWRST; // reset; DATASHEET page.471
  ADC_MR = 1 << 0; // ???, DATASHEET page.472
  ADC_CHER = 1 << 0; // ???, DATASHEET page.474
  ADC_CR = ADC_CR_START;
  int a = -1;
  if ( ADC_SR_EOC0 == 1 ){
    a = ADC_CDR0; 
  }

  TC0_CCR = CLKDIS
  LCDPutStr( itoa(a), 50, 50, LARGE, BLACK, WHITE);


}
```
