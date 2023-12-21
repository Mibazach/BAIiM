# CSRF - Zadania praktyczne
## 0. Instalacja programów

https://www.docker.com/products/docker-desktop/ - potrzebny do zestawienia aplikacji JuiceShop od OWASP

https://github.com/juice-shop/juice-shop#from-sources -  repo podatnej aplikacji internetowej JuiceShop 

https://owasp.org/www-project-juice-shop/ - strona OWASP JuiceShop

https://www.mozilla.org/pl/firefox/download/thanks/ - opcjonalnie przeglądarka Firefox. To na niej przedstawione zostanie jedno z zadań.

Aby zestawić JuiceShop należy wykonać poszczególne kroki (można zrobić to w inny sposób, nie używając Dockera patrz: link do repo JuiceShop):
1. Zainstalować oraz uruchomić Docker
2. Pobrać kontener ```docker pull bkimminich/juice-shop```
3. Uruchomić aplikację ```docker run --rm -p 3000:3000 bkimminich/juice-shop```
4. Aplikacja JuiceShop powinna działać na adresie localhost, porcie 3000. Upewnij się, że tak jest wchodząc na dowolną przeglądarkę i wpisując w wyszukiwarce adres <http://localhost:3000>.

## 1. Podmiana nazwy użytkownika - modyfikacja formularza.

Pierwsze zadanie polega na modyfikacji istniejącego kodu na stronie JuiceShop od OWASP w celu przeprowadzenia ataku CSRF.

WAŻNE!!! Zadanie testowane były przy pomocy przeglądarki Firefox. Zalecamy aby to na niej wykonać ćwiczenie aby uniknąć problemów natury technicznej.

Na początku należy założyć konto na stronie JuiceShop. W tym celu należy udać się w zakładkę “Account”, wcisnąć “Login”. Na samym dole karty z polami do logowania powinien znajdować się tekst "Not yet a customer?”, który po wciśnięciu przekierowuje na stronę do rejestracji. 

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
2. Nazwę użytkownika zmieniano wprost z kodu html, a nie z poziomu strony.
3. Forms przesyłany był automatycznie, bez ingerencji użytkownika.
4. W podstawowym kodzie jest błąd, który uniemożliwia przekierowanie zapytania na prawidłową podstronę JuiceShop. Rozwiąż problem. 

Rezultatem poprawnego wykonania zadania będzie plik html, który po odtworzeniu przez przeglądarkę automatycznie prześle forms zmieniając nazwę użytkownika ofiary.

Voilà! Teraz przy pomocy socjotechniki możemy nakłonić ofiarę zalogowaną do wykonania niechcianego zapytania poprzez wejście w plik.

## 2. Dlaczego atak się powiódł? - polityka ochronna

Wyjaśnij dlaczego atak zadziałał. Jakich mechanizmów obronnych zabrakło podczas wykorzystywania podatności CSRF. Jak można się było zabezpieczyć?

## GRUYERE

Będziemy korzystać z narzędzia Google Gruyere, które było projektem stworzonym przez Google do nauki i testowania umiejętności związanych z bezpieczeństwem aplikacji internetowych. Strona jest podatna na wiele rodzajów ataków, w tym prezentowany przez nas CSRF.

Twoim zadaniem będzie wcielić się w hakera i jak najbardziej namieszać na profilu zalogowanego użytkownika, który wejdzie w link na twoim profilu.

Link do używanej strony: <https://google-gruyere.appspot.com/start>

## 3. Zmiana danych na profillu użytkownika poprzez zapytanie POST

Przebieg ataku, którego dokonamy:

Ofiara widzi post hakera na swojej stronie głównej i jest zainteresowana innymi postami użytkownika, więc chce wejść na jego stronę “Homepage”. Niczego nieświadoma klika w link do złośliwego kodu, który zmienia jej zdjęcie profilowe oraz nazwę użytkownika – padła ofiarą ataku CSRF.

Poniższy przykład przedstawia ten sam post widoczny przed i po ataku.

Przed atakiem:

![image](https://github.com/Mibazach/BAIiM/blob/main/przed.jpg)

Po ataku:

![image](https://github.com/Mibazach/BAIiM/blob/main/po.jpg)

Jak to zrobić?

Aby dokonać ataku najpierw potrzebujesz się zaznajomić ze stroną, na której będziesz wykonywać atak.

Stwórz konto hakera oraz drugie konto do porównania. Zauważ, że niezależnie od tego na jakim koncie jesteś zalogowany, w adresie URL nadal znajduje się ten sam Gruyere ID (zapisz ten numer, będzie ci później potrzebny).

Zapoznaj się z zakładką Profile, gdzie wykonasz pierwsze zadanie.  Przeanalizuj jej strukturę przy pomocy narzędzi deweloperskich (F12).

Twoje zadanie będzie polegać na napisaniu kodu, który po wykonaniu automatycznie wyśle formularz zmiany danych na profilu (bez zatwierdzania przyciskiem “Update” jak to jest konieczne normalnie).

Napisany przez ciebie kod ma co najmniej zmienić nazwę użytkownika oraz ustawić mu nowe zdjęcie profilowe (Icon).

Najprościej to zrobić w ten sposób:
1. W nowym pliku z rozszerzeniem html zapisz podstawowy szkielet strony.
2. Przekopiuj formularz z zakładki profile do body (wpisane tutaj wartości nie mają znaczenia – i tak będziemy je zmieniać).
3. W nagłówku napisz kod w JavaScript, który zmienia zawartość formularza z body. Podpowiedź: poniżej znajdziesz przykład zmiany nazwy użytkownika:

```
<script>
    	window.onload = function() {
        	// Po załadowaniu strony wykonaj modyfikacje formularza i automatycznie go wyślij
        	var form = document.forms[0];
        	form.elements['name'].value = 'Nowa Nazwa';                	// Zmiana wartości pola 'name'        	
        	// Automatyczne wysłanie formularza
        	form.submit();
    	};
</script>
```

4. Po napisaniu kodu i zapisaniu go, dodaj plik na swoją stronę hakera (w zakładce Upload). Skopiuj wygenerowany przez stronę link do pliku – w ten sposób przekażesz go swojej ofierze.
5. W zakładce Home użytkownik widzi posty innych osób i jeśli jakiś go zainteresuje wejść na stronę główną (Homepage) tej osoby. Chcemy żeby właśnie tutaj nastąpiło przekierowanie. Wklej link do swojego kodu do Homepage w zakładce Profile.
6. Wyloguj się z konta hakera i zaloguj jako ofiara. Zobacz co się stanie, gdy klikniesz w Homepage pod postem hakera.

## 4. Wylogowanie użytkownika poprzez zapytanie GET

Przebieg ataku, którego dokonamy:

Analogicznie do zadania 3. ofiara klika w link do złośliwego kodu i zostaje wylogowana. Zdezorientowana loguje się na konto i próbuje znów wejść na Homepage autora, który ją zainteresował i znowu zostaje wylogowana…

1. Otwórz konsolę deweloperską i zapoznaj się z kodem źródłowym paska menu. Zobacz, w jaki sposób jest wykonywane wylogowanie użytkownika.
2. Napisz kod html, który po kliknięciu przekieruje użytkownika na /logout (podpowiedź: użyj meta http-equiv="refresh")
3. Analogicznie do zadania 3. dodaj kod na swoje konto i udostępnij ofierze link, jako swój Homepage.
4. Wyloguj się z konta hakera i zaloguj jako ofiara. Zobacz co się stanie, gdy klikniesz w Homepage pod postem hakera.





