# CSRF - Zadania praktyczne
## 0. Instalacja programów

https://www.docker.com/products/docker-desktop/ - potrzebny do zestawienia aplikacji JuiceShop od OWASP

https://portswigger.net/burp/releases/professional-community-2023-10-3-7 - narzędzie do analizy i modyfikacji zapytań (z rozwijanej listy wybrać 'community edition')

https://github.com/juice-shop/juice-shop#from-sources -  repo podatnej aplikacji internetowej JuiceShop 

https://owasp.org/www-project-juice-shop/ - strona OWASP JuiceShop

https://www.mozilla.org/pl/firefox/download/thanks/ - opcjonalnie przeglądarka Firefox. To na niej przedstawiona zostanie przykładowa konfiguracja serwera proxy Burpa.

Aby zestawić JuiceShop należy wykonać poszczególne kroki (można zrobić to w inny sposób, nie używając Dockera patrz: link do repo JuiceShop):
1. Zainstalować oraz uruchomić Docker
2. Pobrać kontener ```docker pull bkimminich/juice-shop```
3. Uruchomić aplikację ```docker run --rm -p 3000:3000 bkimminich/juice-shop```
4. Aplikacja JuiceShop powinna działać na adresie localhost, porcie 3000. Upewnij się, że tak jest wchodząc na dowolną przeglądarkę i wpisując w wyszukiwarce adres <http://localhost:3000>.

Teraz 


## 1. Podmiana nazwy użytkownika - modyfikacja formularza.

Pierwsze zadanie polega na modyfikacji istniejącego kodu na stronie JuiceShop od OWASP w celu przeprowadzenia ataku CSRF.

WAŻNE!!! Zadanie testowane były przy pomocy przeglądarki Firefox. Zalecamy aby to na niej wykonywać zadania aby uniknąć problemów z wykonaniem ćwiczenia.

Na początku należy założyć konto na stronie JuiceShop. W tym celu należy udać się w zakładkę “Account”, wcisnąć “Login”. Na samym dole karty z polami do logowania powinien znajdować się tekst “Not yet a customer?”, który po wciśnięciu przekierowuje na stronę do rejestracji. 

Następnie rejestrujemy się podając dane (nie ma znaczenia jakie dane będą, można podać np: email = 123@123.pl, hasło = 12345)

Po poprawnej rejestracji zostajemy przekierowani na panel logowania - logujemy się na stronę. 

![image](https://github.com/Mibazach/BAIiM/blob/main/Logowanie.jpg)

Później przechodzimy do “Account” i naciskamy guzik z podanym wcześniej emailem.

Naszym oczom ukazuje się panel profilu. Skupimy się na polu “Username”, aby móc przeprowadzić atak CSRF i podmienić nazwę nieświadomego użytkownika. Jednak przygotowanie takiego ataku wymaga stworzenia jakiejś strony, która wykonuje preparację fałszywego zapytania - zróbmy to tak, aby ofiara po wejściu na stworzoną stronę nieświadomie dokonała tej zmiany.

![image](https://github.com/Mibazach/BAIiM/blob/main/nazwa.jpg)

W tym celu wykorzystamy zapytanie POST, które wysyłane jest do serwera z informacją o zmianie nazwy użytkownika.

Musimy kolejno odnaleźć tag ‘form’, który służy do aktualizowania nazwy użytkownika, skopiować kod html do dowolnego edytora kodu i zapisać plik z rozszerzeniem .html, aby móc go otworzyć przez przeglądarkę. Kod wygląda w następujący sposób:

![image](https://github.com/Mibazach/BAIiM/blob/main/Form.jpg)

Najważniejszy jest formularz dotyczący zmiany nazwy użytkownika - kod można okroić z niepotrzebnych tagów: 

![image](https://github.com/Mibazach/BAIiM/blob/main/Okrojony%20form.jpg)

Teraz pozostaje zmodyfikować istniejący kod tak, aby przeprowadzić atak. Chcemy aby:
1. Nasz forms był niewidoczny,
2. Nazwę użytkownika zmieniano wprost z kodu html, a nie z poziomu strony
3. Forms przesyłany był automatycznie, bez ingerencji użytkownika.
4. W podstawowym kodzie jest błąd, który uniemożliwia przekierowanie zapytania na prawidłową podstronę JuiceShop. Rozwiąż problem. 

Rezultatem poprawnego wykonania zadania będzie plik html, który po odtworzeniu przez przeglądarkę automatycznie prześle forms zmieniając nazwę użytkownika ofiary.

Voilà! Teraz przy pomocy socjotechnik możemy nakłonić ofiarę zalogowaną do wykonania niechcianego zapytania poprzez wejście w plik.


## 2. Dlaczego atak się powiódł? - polityka ochronna

Wyjaśnij dlaczego atak zadziałał. Jakich mechanizmów obronnych zabrakło podczas wykorzystywania podatności CSRF. Jak można się było zabezpieczyć?

## 3. GRUYERE

