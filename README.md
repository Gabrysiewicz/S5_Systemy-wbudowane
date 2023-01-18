<h2>
<table>
<thead><tr>
	<th colspan=3> 
	 Laboratorium Systemów Wbudowanych 
	 <img width='1000' height=1> </th>
	</tr>
<thead>
	<tbody>
	<tr><td> 4.01.2023 r. </td><td>Kamil Gabrysiewicz 95400</td><td>3.5 SE</td></tr>
	</tbody>
</table>
</h2>

<h3> 1. Odpowiedź na pytanie kontrolne 1 </h3>
<h4> Opisz 4 wybrane przez siebie rodzaje przetworników analogowo-cyfrowych stosowanych w układach elektronicznych. </h4>

<ul>
  <li>Przetwornik Delta-Sigma (ΔΣ) - jest to przetwornik, który działa na zasadzie modulacji delta-sigma, polegającej na przekształcaniu sygnału analogowego na sygnał cyfrowy. Charakteryzuje się wysoką rozdzielczością i niskim poziomem zakłóceń, dlatego jest często stosowany w układach audio i medycznych.
  </li>
  <li>Przetwornik cyfrowy proporcjonalny (CDS) - jest to przetwornik, który działa na zasadzie porównywania sygnału wejściowego z sygnałem referencyjnym. Charakteryzuje się niskim poziomem zakłóceń, wysoką rozdzielczością i dobrą stabilnością, dlatego jest często stosowany w układach pomiarowych i kontrolnych.
  </li>
  <li>Przetwornik Różniczkująco-komparatorowy (SAR) - jest to przetwornik, który działa na zasadzie różniczkowania sygnału wejściowego i jego komparacji z sygnałem referencyjnym. Charakteryzuje się wysoką rozdzielczością i szybkim czasem próbkowania, dlatego jest często stosowany w układach pomiarowych i kontrolnych.
  </li>
  <li>Przetwornik Flash - jest to przetwornik, który działa na zasadzie porównywania sygnału wejściowego z sygnałem referencyjnym. Charakteryzuje się bardzo wysoką rozdzielczością, szybkim czasem próbkowania i wysoką stabilnością, dlatego jest często stosowany w układach pomiarowych i kontrolnych.
  </li>
</ul>

<h3> 2. Odpowiedź na pytanie kontrolne 2 </h3>
<h4>Czy  do  konfiguracji oraz odczytu wartościnapięcia analogowego konieczne jest dołączanie zegara systemowego do ADC w rejestrze PMC_PCER? Odpowiedź uzasadnij.</h4>

 Jest to konieczne, ponieważ przetwornik analogowo-cyfrowy (ADC) potrzebuje zegara systemowego do swojego działania. Zegar systemowy jest potrzebny do synchronizacji procesu próbkowania i konwersji oraz do generowania impulsów próbkujących. Konfiguracja ADC wymaga skonfigurowania zegara systemowego w odpowiednim trybie i częstotliwości, aby zapewnić prawidłowe działanie przetwornika. Rejestr PMC_PCER służy do włączenia peryferiów poprzez odpowiednie ustawienie flagi w tym rejestrze. W przypadku ADC, włączenie peryferiów przetwornika analogowo-cyfrowego wymaga ustawienie flagi odpowiadającej za ten peryferia.


<h3> 3. Odpowiedź na pytanie kontrolne 3 </h3>
Zaproponuj kod programu w którym sprawdzisz, czy konwersja na jedynymzaktywnychkanałówsię zakooczyła. Jeżeli tak, zapisz wartośd reprezentującą napięcie analogowe do zmiennej „a”.Załóż, że nie wiesz które kanały są aktywne –interesuje Cię wynik ostatniej konwersji.Należy stworzyd jedynie warunek, w którym sprawdzisz odpowiednią flagę i wykonasz operację przypisania do „a” (2 linijki kodu w C).

```
if ( ADC_ISR & ADC_ISR_EOC(1) )
  a = ADC_CDR; 
```

<h3> 4. Odpowiedź na pytanie kontrolne 4 </h3>
Jaka   jest zdolnośd  rozdzielcza(w   mV)   12-to  bitowego  przetwornika  ADC,  którego  zakres konwertowanych napięd zawiera się pomiędzy 0 a 5V?

```
Rozdzielczość = [ (5V) / (2^12 - 1) ] = [ 5V / 4095 ] = [ 0,00122V ] lub [ 1,22mV ]
```

<h3> 5. Odpowiedź na pytanie kontrolne 5 </h3>
Na  jak  wiele wartości może byd przekonwertowana wartośd napięcia z zakresu od 0V do napięcia referencyjnego w przypadku układu SAM7X256? Jaka jest domyślna rozdzielczośd? Jak zmienić rozdzielczośd?

<ul>
  <li>W przypadku układu SAM7X256, przetwornik analogowo-cyfrowy (ADC) ma 12-bitową rozdzielczość. Oznacza to, że jest w stanie przekonwertować wartość napięcia z zakresu od 0V do napięcia referencyjnego na 4096 różnych wartości cyfrowych (2^12).</li>

  <li>Domyślnie rozdzielczość przetwornika jest ustawiona na 12 bitów, ale można ją zmienić poprzez odpowiednie konfiguracje rejestrów</li>

  <li>Aby zmienić rozdzielczość przetwornika, należy ustawić odpowiedni bit konfiguracyjny w odpowiednim rejestrze, np. ADC_MR</li>
</ul>

<h3> Część praktyczna: </h3>
<h4> Zadanie 1 </h4>
Określenie granicznych wartości liczbowych związanych z napięciem: (odczyt z rozdzielczością 10-bit)
<ol>
  <li>wyświetl wartośd z konwersji na kanale ADC do którego przyłączone jest napięcie związane z temperaturą otoczenia (AN_TERM)</li>
  <li>wyświetl wartośd z konwersji na kanale ADC do którego przyłączone jest napięcie związane z temperaturą odniesienia (AN_TERM)</li>
</ol>
Na  podstawie  dwóch  odczytów  (zakładając  że  pomiędzy  temperaturą  otoczenia  a  odniesienia charakterystyka termistora jest liniowa) wyświetl na ekranie LCD aktualną temperaturę z jednym miejscem po przecinku.Najpierw trzeba to przeliczyd matematycznie, potem podjąd decyzję czy pracujeciena wartościachtypu INT czy FLOAT oraz czy do konwersji zmiennej użyjecie itoa czy sprintf. Może INT i wykorzystad dzielenie z resztą „modulo” ?
Pamiętajcie, że 17/10 = 1 reszty 

JEDNOCZEŚNIE
Odczyt kanału ADC do którego przyłożone jest napięcie związane  z  potencjometrem  AN_TRIM  z rozdzielczością 10-bit, powoduje zapalenie bądź zgaszenie ekranu LCD –jeżeliodczyt większy niż 512 ekran świeci, jeśli odczyt mniejszy równy 512ekran gaśnie czyli sprawdzacie dwa kanały ADC.

<h4> Kod zadania 1 </h4>

```
#include <at91sam7x256.h>
#include <lcd.h>

#define ADC_CR (*(volatile unsigned int *)(0xFFFD8000))
#define ADC_MR (*(volatile unsigned int *)(0xFFFD8004))
#define ADC_CHER (*(volatile unsigned int *)(0xFFFD8010))
#define ADC_SR (*(volatile unsigned int *)(0xFFFD801C))
#define ADC_CDR6 (*(volatile unsigned int *)(0xFFFD8038))

int main(void)
{
    // Initialize the ADC controller
    ADC_CR = 1 << 8;
    ADC_MR = 15 << 8 | 7;

    // Read the first 10-bit value from the AN_TERM channel
    ADC_CHER = 1 << 6;
    ADC_CR = 1;
    while (!(ADC_SR & (1 << 6)));
    int value1 = ADC_CDR6;

    // Convert the first value to temperature
    float temperature1 = 20.0 + (float)(value1 - 500) / 50.0;
    // Read the second 10-bit value from the AN_TERM channel
    ADC_CHER = 1 << 6;
    ADC_CR = 1;
    while (!(ADC_SR & (1 << 6)));
    int value2 = ADC_CDR6;

    // Convert the second value to temperature
    float temperature2 = 20.0 + (float)(value2 - 500) / 50.0;

    //Calculate the current temperature
    float currentTemperature = (temperature1 + temperature2) / 2.0;
    // Round to one decimal place
    currentTemperature = roundf(currentTemperature * 10) / 10;
    // Display the current temperature on the LCD screen
    lcd_init();
    lcd_print("Current temperature: ");
    lcd_print_float(currentTemperature);
    lcd_print(" C");

    return 0;
}

```

<h4> Zadanie 2 </h4>
Skonfiguruj timer TC0 w tryb porównania z wartością RC (CPCTRG w rejestrze TC_CMR) tak, żeby odliczad interwały czasu 200 ms. Konwersja wartości temperatury powinna następowad co 200ms w interwałach odliczanych przez PRZERWANIA od porównania TC0 z wartością TC0_RC. 

```
#include <at91sam7x256.h>

// Define the timer TC0 base address
#define TC0_BASE (0xFFFA0000)

int main(void) {
    // Enable the clock for the TC0 peripheral
    AT91C_BASE_PMC->PMC_PCER = 1 << 17;

    // Configure the TC0 register CMR
    *AT91C_TC0_CMR = AT91C_TC_CPCTRG | AT91C_TC_WAVE | AT91C_TC_TIMER_CLOCK1;

    // Set the TC0 register RC to the value for a 200ms interval
    *AT91C_TC0_RC = (MCK/8)/5;

    // Enable the TC0 compare interrupt
    *AT91C_TC0_IER = AT91C_TC_CPCS;

    // Enable the TC0 interrupt in the AIC
    AT91C_BASE_AIC->AIC_IECR = 1 << 17;

    // Enable the TC0 timer
    *AT91C_TC0_CCR = AT91C_TC_CLKEN | AT91C_TC_SWTRG;

    while(1) {
        // Wait for the TC0 compare interrupt
        while(!(*AT91C_TC0_SR & AT91C_TC_CPCS));

        // Clear the TC0 compare interrupt flag
        *AT91C_TC0_SR;

        // Read the 10-bit value from the AN_TERM channel
        ADC_CHER = 1 << 6;
        ADC_CR = 1;
        while (!(ADC_SR & (1 << 6)));
        int value = ADC_CDR6;

        // Convert the value to temperature
        float temperature = 20.0 + (float)(value - 500) / 50.0;

        // Perform some action with the temperature value every 200ms
        //...
    }
}
```
