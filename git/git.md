Git Mini HowTo
=================

Questo file è stato scritto per imparare ad usare git.
Autore: Adrian Unterfinger
Data: 4 giungno 2014

Note! Questo file non è stato ancora tradurre in italiano.



## Configurazione globale:

`git config --global user.name "Adrian Unterfinger"`  
`git config --global user.email "wuenci[at]msn.com"`  
```
Il path del file di configurazione si trova nella %HOMEPATH%\.gitconfig
```

## Inizializzare un progetto
Cambiare nella cartella desidereta e scrivere seguente comando:  
`git init`


## Visualizzare i log
`git log`  


##  Visualizzare lo stato
`git status`  

## Visualizzare lo stato sul branch remoto
`git remote update && git status`  


## Aggiungere un file:
`git add helloworld.c`  
oppure tutti i file con estensione "c":  
`git add *.c`  
oppure aggiungere tutti :  
`git add .`  

## Aggiungere tutte le modifiche
`git add -A`  

## Commitare:
`git commit -m "initial commit"`  
oppire:  
`git commit`  


## Eliminare file in git e sul filesystem:
`git rm -f unusedfile.c`  

## Änderungen egal welcher natur unbedingt zurücksetzten:
`git reset HEAD --hard` (HEAD = Letzte bekante version)


## Differenzen anzeigen:
`git diff`  
`git diff a56dc182f8211932c58cea4ce4dd0`  
`git diff a56dc182f8211932c58cea4ce4dd0 helloworld.c`  


## Zu einer früheren version zurücksetzten:
`git checkout a56dc182f8211932c58cea4ce4dd0`  
`git add -A`  
`git commit -m "Version fünf"`  
`git branch`  
`git checkout master`  

Note! Achtung ich befinde mich zuzeit in einem inkonsitenten zustand, das bedeutet immer noch im commit a56dc182f8211932c58cea4ce4dd0.

Um auf den master Strang zu gelangen  muss ich folendes tun:

Attention! Wichtig, nur diese Befehle nutzen:

`git revert --no-commit a56dc182f8211932c58cea4ce4dd0`  
`git commit -m "Zurückgesetzt"`  

oder auch absolot zuücksetzten:  
``git reset --hard a56dc182f8211932c58cea4ce4dd0`` (dann werden alle dazwichen gemachten commit ebenfalls gelöscht.)

## Den aktuellen Branch anzeigen:
`git branch`


## Esempi
```
BEISPIEL Stränge zusammenführen:
==================================
Branches zusammenführen:
 (in den feature Strang wechseln)
git merge master
git commit -a
git checkout master
 (in den master Strang wechseln)
git merge feature/strangname
 (es ergibt sich einen fast forward merge,
  da sich alle inkongruenzen im feature/strangname Strang aufgelöst wurden.
Den feature/formular strang löschen:
git branch -d feature/formular
```


```
GitHub:
==========

Ein bereits erstelltes Project hunzufügen:
- Das project muss auf gitlab erstellt sein.

git config --global user.name "Adrian Unterfinger"
git config --global user.email "wuenci@msn.com"
git init
git remote add origin git@github.com:wuenci/makefileex.git
git add .
git commit
git push -u origin master


Den master auf GitHub senden:
git push -u origin master
```