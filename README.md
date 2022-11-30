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
