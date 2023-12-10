# CSRF - Zadania praktyczne
## 0. Instalacja programów

https://www.docker.com/products/docker-desktop/ - potrzebny do zestawienia aplikacji JuiceShop od OWASP

https://portswigger.net/burp/releases/professional-community-2023-10-3-7 - narzędzie do analizy i modyfikacji zapytań

https://github.com/juice-shop/juice-shop#from-sources -  repo podatnej aplikacji internetowej JuiceShop 

https://owasp.org/www-project-juice-shop/ - strona OWASP JuiceShop

https://www.mozilla.org/pl/firefox/download/thanks/ - opcjonalnie przeglądarka Firefox. To na niej przedstawiona zostanie przykładowa konfiguracja serwera proxy Burpa.

Aby zestawić JuiceShop należy wykonać poszczególne kroki (można zrobić to w inny sposób, nie używając Dockera patrz: link do repo JuiceShop):
1. Zainstalować oraz uruchomić Docker
2. Pobrać kontener ```docker pull bkimminich/juice-shop```
3. Uruchomić aplikację ```docker run --rm -p 3000:3000 bkimminich/juice-shop```
4. Aplikacja JuiceShop powinna działać na adresie localhost, porcie 3000. Upewnij się, że tak jest wchodząc na dowolną przeglądarkę i wpisując w wyszukiwarce adres <http://localhost:3000>.

Teraz 


## 1. Zadanie Pierwsze - zamiana adresu email przy pomocy 
