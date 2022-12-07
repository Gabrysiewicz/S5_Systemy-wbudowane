# SAM7

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

Zadanie 2

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
