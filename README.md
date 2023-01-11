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
Opisz 4 wybrane przez siebie rodzaje przetworników analogowo-cyfrowych stosowanych w układach elektronicznych.

```
Przetwornik Delta-Sigma (ΔΣ) - jest to przetwornik, który działa na zasadzie modulacji delta-sigma, polegającej na przekształcaniu sygnału analogowego na sygnał cyfrowy. Charakteryzuje się wysoką rozdzielczością i niskim poziomem zakłóceń, dlatego jest często stosowany w układach audio i medycznych.

Przetwornik cyfrowy proporcjonalny (CDS) - jest to przetwornik, który działa na zasadzie porównywania sygnału wejściowego z sygnałem referencyjnym. Charakteryzuje się niskim poziomem zakłóceń, wysoką rozdzielczością i dobrą stabilnością, dlatego jest często stosowany w układach pomiarowych i kontrolnych.

Przetwornik Różniczkująco-komparatorowy (SAR) - jest to przetwornik, który działa na zasadzie różniczkowania sygnału wejściowego i jego komparacji z sygnałem referencyjnym. Charakteryzuje się wysoką rozdzielczością i szybkim czasem próbkowania, dlatego jest często stosowany w układach pomiarowych i kontrolnych.

Przetwornik Flash - jest to przetwornik, który działa na zasadzie porównywania sygnału wejściowego z sygnałem referencyjnym. Charakteryzuje się bardzo wysoką rozdzielczością, szybkim czasem próbkowania i wysoką stabilnością, dlatego jest często stosowany w układach pomiarowych i kontrolnych.
```

<h3> 2. Odpowiedź na pytanie kontrolne 2 </h3>
Czy  do  konfiguracji oraz odczytu wartościnapięcia analogowego konieczne jest dołączanie zegara systemowego do ADC w rejestrze PMC_PCER? Odpowiedź uzasadnij.

```
 Jest to konieczne, ponieważ przetwornik analogowo-cyfrowy (ADC) potrzebuje zegara systemowego do swojego działania. Zegar systemowy jest potrzebny do synchronizacji procesu próbkowania i konwersji oraz do generowania impulsów próbkujących. Konfiguracja ADC wymaga skonfigurowania zegara systemowego w odpowiednim trybie i częstotliwości, aby zapewnić prawidłowe działanie przetwornika. Rejestr PMC_PCER służy do włączenia peryferiów poprzez odpowiednie ustawienie flagi w tym rejestrze. W przypadku ADC, włączenie peryferiów przetwornika analogowo-cyfrowego wymaga ustawienie flagi odpowiadającej za ten peryferia.
```

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
Na  jak  wiele wartości może byd przekonwertowana wartośd napięcia z zakresu od 0V do napięcia referencyjnego w przypadku układu SAM7X256? Jaka jest domyślna rozdzielczośd? Jak zmienid rozdzielczośd?

<h3> Część praktyczna: </h3>
Określenie granicznych wartości liczbowych związanych z napięciem: (odczyt z rozdzielczością 10-bit)
1. wyświetl wartośd z konwersji na kanale ADC do którego przyłączone jest napięcie związane z temperaturą otoczenia (AN_TERM)
2. wyświetl wartośd z konwersji na kanale ADC do którego przyłączone jest napięcie związane z temperaturą odniesienia (AN_TERM)
Na  podstawie  dwóch  odczytów  (zakładając  że  pomiędzy  temperaturą  otoczenia  a  odniesienia charakterystyka termistora jest liniowa) wyświetl na ekranie LCD aktualną temperaturę z jednym miejscem po przecinku.Najpierw trzeba to przeliczyd matematycznie, potem podjąd decyzję czy pracujeciena wartościachtypu INT czy FLOAT oraz czy do konwersji zmiennej użyjecie itoa czy sprintf. Może INT i wykorzystad dzielenie z resztą „modulo” ?
Pamiętajcie, że 17/10 = 1 reszty 

JEDNOCZEŚNIE
Odczyt kanału ADC do którego przyłożone jest napięcie związane  z  potencjometrem  AN_TRIM  z rozdzielczością 10-bit, powoduje zapalenie bądź zgaszenie ekranu LCD –jeżeliodczyt większy niż 512 ekran świeci, jeśli odczyt mniejszy równy 512ekran gaśnie czyli sprawdzacie dwa kanały ADC.

<h4> Kod zadania 1 </h4>

```

```
