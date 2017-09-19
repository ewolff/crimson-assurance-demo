# Beispiel starten

Die ist eine Schritt-für-Schritt-Anleitung zum Starten der Beispiele.

## Installation

* Die Beispiele laufen in Docker Containern. Dazu ist eine
  Installation von Docker Community Edition notwendig, siehe
  https://www.docker.com/community-edition/ . Docker kann mit
  `docker` aufgerufen werden. Das sollte nach der Installation ohne
  Fehler möglich sein.

* Die Beispiele benötigen zum Teil sehr viel Speicher. Daher sollte
  Docker ca. 4 GB zur Verfügung haben. Sonst kann es vorkommen, dass
  Docker Container aus Speichermangel beendet werden. Unter Windows
  und macOS findet sich die Einstellung dafür in der Docker-Anwendung
  unter Preferences/ Advanced.

* Nach der Installation von Docker sollte `docker-compose` aufrufbar
  sein. Wenn Docker Compose nicht aufgerufen werden kann, ist es nicht
  als Teil der Docker Community Edition installiert worden. Dann ist
  eine separate Installation notwendig, siehe
  https://docs.docker.com/compose/install/ .

## Docker Container bauen

Zunächst musst du die Docker Images bauen. Wechsel in das Verzeichnis 
`docker` und starte `docker-compose build`. Das lädt die Basis-Images
herunter und installiert die Software in die Docker Images:

```
[~/crimson-insurance-demo]docker-compose build
....
[INFO] Installing /crimson-postbox/pom.xml to /root/.m2/repository/com/innoq/lvm-las-postbox/0.1.0-SNAPSHOT/lvm-las-postbox-0.1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 52.883 s
[INFO] Finished at: 2017-09-19T07:39:42+00:00
[INFO] Final Memory: 29M/209M
[INFO] ------------------------------------------------------------------------
 ---> b091fcc2b55f
Removing intermediate container f1207111f599
Step 22/23 : CMD cd crimson-postbox && mvn spring-boot:run
 ---> Running in 9318bed1fea6
 ---> 8ef39a0f8e37
Removing intermediate container 9318bed1fea6
Step 23/23 : EXPOSE 3001
 ---> Running in f86f143f8972
 ---> 4a4f63a978eb
Removing intermediate container f86f143f8972
Successfully built 4a4f63a978eb
Successfully tagged crimsoninsurancedemo_crimson-postbox:latest
```

Danach sollten die Docker Images erzeugt worden sein. Sie haben das
Präfix `crimsoninsurancedemo`:

```
~/crimson-insurance-demo]docker images 
REPOSITORY                                              TAG                 IMAGE ID            CREATED             SIZE
crimsoninsurancedemo_crimson-postbox                    latest              4a4f63a978eb        9 minutes ago       427MB
crimsoninsurancedemo_crimson-portal                     latest              e3f50765a44b        12 minutes ago      207MB
crimsoninsurancedemo_crimson-damage                     latest              59fead419bfd        14 minutes ago      201MB
crimsoninsurancedemo_crimson-letter                     latest              f8d68ef9ad9b        16 minutes ago      203MB
crimsoninsurancedemo_crimson-backend                    latest              0c1ce5a82033        23 minutes ago      86.2MB
```

## Docker Container starten

Nun kannst Du die Container mit `docker-compose up -d` starten. Die
Option `-d` bedeutet, dass die Container im Hintergrund gestartet
werden und keine Ausgabe auf der Kommandozeile erzeugen.

```
[~/crimson-insurance-demo]docker-compose up -d
crimsoninsurancedemo_crimson-backend_1 is up-to-date
Recreating crimsoninsurancedemo_crimson-portal_1 ... 
Recreating crimsoninsurancedemo_crimson-damage_1 ... 
Recreating crimsoninsurancedemo_crimson-letter_1 ... 
Recreating crimsoninsurancedemo_crimson-portal_1
Recreating crimsoninsurancedemo_crimson-postbox_1 ... 
Recreating crimsoninsurancedemo_crimson-damage_1
Recreating crimsoninsurancedemo_crimson-letter_1
Recreating crimsoninsurancedemo_crimson-portal_1 ... done
```

Du kannst nun überprüfen, ob alle Docker Container laufen:

```
[~/crimson-insurance-demo]docker ps
CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                                                NAMES
c0f6167fd919        crimsoninsurancedemo_crimson-postbox   "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3001->3001/tcp   crimsoninsurancedemo_crimson-postbox_1
b27142edc51f        crimsoninsurancedemo_crimson-letter    "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3002->3002/tcp   crimsoninsurancedemo_crimson-letter_1
4da7ff2e8e5d        crimsoninsurancedemo_crimson-damage    "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3003->3003/tcp   crimsoninsurancedemo_crimson-damage_1
f2ab6a5e2695        crimsoninsurancedemo_crimson-portal    "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp   crimsoninsurancedemo_crimson-portal_1
5c47a52e0317        crimsoninsurancedemo_crimson-backend   "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp   crimsoninsurancedemo_crimson-backend_1
```

Die Docker container sollten auf `localhost` lauden. Sonst setzte die
Umgebungsvariable `CRIMSON_SERVER` auf den Namen des Servers.

`docker ps -a`  zeigt auch die terminierten Docker Container an. Das
ist nützlich, wenn ein Docker Container sich sofort nach dem Start
wieder beendet..

Wenn einer der Docker Container nicht läuft, kannst du dir die Logs
beispielsweise mit `docker logs crimsoninsurancedemo_crimson-letter_1` anschauen. Der Name
der Container steht in der letzten Spalte der Ausgabe von `docker
ps`. Das Anzeigen der Logs funktioniert auch dann, wenn der Container
bereits beendet worden ist. Falls im Log steht, dass der Container
`killed` ist, dann hat Docker den Container wegen Speichermangel
beendet. Du solltest Docker mehr RAM zuweisen z.B. 4GB. Unter Windows
und macOS findet sich die RAM-Einstellung in der Docker application
unter Preferences/ Advanced.

Um einen Container genauer zu untersuchen, kannst du eine Shell in dem
Container starten. Beispielsweise mit `docker exec -it
crimsoninsurancedemo_crimson-letter_1 /bin/sh` oder du kannst in dem Container ein
Kommando mit `docker exec mskafka_catalog_1 /bin/ls` ausführen.

Unter http://localhost:3000/ kannst du nun eine Bestellung
erfassen. Nach einiger Zeit sollte für die Bestellung eine Lieferung
und eine Rechnung erstellt worden sei.

Mit `docker-compose down` kannst du alle Container beenden.
