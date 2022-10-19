# Systemy-wbudowane
```
r;
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 1
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 4
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 7
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk *
            _delay_ms(500);
        }
        
        // dla K2
        PORTB = 0b1101;
        _delay_ms(1);
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 2
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 5
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 8
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 0
            _delay_ms(500);
        }
        
        // dla K3
        PORTB = 0b1011;
        _delay_ms(1);
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 3
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 6
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk 9
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk #
            _delay_ms(500);
        }
        
        // dla K4
        PORTB = 0b0111;
        _delay_ms(1);
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk A
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk B
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk C
            _delay_ms(500);
        }
        if((PINA & 0b0001) == 0){
            // zdarzenie na przycisk D
            _delay_ms(500);
        }
    }
}
```
