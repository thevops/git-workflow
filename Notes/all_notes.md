GIT notes out of order.


# Log

- `git log --oneline`
- `git log --since....`
- `git log --graph --oneline`


# Tagi

- `git tag NAZWA` -> lightweight tag
- `git tag NAZWA COMMIT`
- `git tag -d NAZWA`
- `git tag -a NAZWA -m "wiadomosc" COMMIT` -> annotated tag


# Poruszanie się po historii

- `git show master~2` - 2 commit od góry
- `git log --oneline tag1..tag3` - od do, można użyć hashy


# Gałęzie

Tworzenie używa aktualnego stanu.

- `git branch NAZWA` - tworzy
- `git branch` - listuje
- `git branch -d NAZWA` - usuwa

- `git checkout` - zmiana gałęzi/taga/commita/ref
- `git checkout -b NAZWA` - tworzy i od razu przechodzi

- `head` - wskaźnik na najnowszy commit
- `HEAD` lub `@` - aktualny commit, którego używamy
- `git show @@'{15 minutes ago}'` - na którym commicie byłem 15 minut temu

- `git branch -m NOWA_NAZWA` - zmiana nazwy aktualnej gałęzi

### Detached HEAD
- checkout tip'a który nie jest końcówką żadnej gałęzi
- commity nie działają
- powrót - `git checkout -`


### Pobranie commita do aktualnej kopii
- `git checkout @^ -- plik/do/skopiowania` - z poprzedniego
    - `--` jak w bashu zapobiega interpretacji argumentów jako parametry

# Merge
- połączenie dwóch lub więcej gałęzi poprzez przeniesienie zmian z jednej do drugiej


### Kroki ogólne
1. Przejście na gałąź, która jest celem (ma zostać)
2. `git merge INNA_GALAZ` - źródłowa


### Rodzaje merge
- Fast Forward - nie zostawia śladu w historii
    - gdy gałąź docelowa nie posiada commitów spoza gałęzi źródłowej
    - inaczej: gdy na docelowej nie zrobiono commita od czasu utworzenia gałęzi źródłowej
- 3-way merge
    - base
    - source
    - target
- octopus merge - połączenie więcej niż 2 gałęzi


# Kopiowanie commitów między gałęziami - cherry-pick

1. `git tag magenta` - tagujemy commit do przeniesienia
2. `git checkout master` - przechodzę na docelową gałąź
3. `git cherry-pick magenta` - przenoszę otagowany commit



# Git workflow

- branch per feature - dla każdego zadania tworzymy nową, dedykowaną gałąź
- każda zmiana = commit
- przydatne
    - `git diff --word-diff` - diff inline
- **BEST PRACTICE** - nie używaj FF merge
    - `git merge --no-ff NAZWA` - wyłączenie Fast Forward. Dzięki temu widać w historii, że była oddzielna gałąź i mamy jej opis. Potem `git branch -d NAZWA` bo już niepotrzebne


# Praca ze zdalnym repozytorium

- `git clone`
- `git branch -vv`
- `git remote show`
- `git remote add origin URL/PATH` -> dodanie zdalnego repo
    - `remove` -> usuwa

## Bare repository

Służy do dzielenia się commitami. Nie ma Working Copy. Może być używane jako backup.

- `mkdir repo.git` -> **BEST PRACTICE**
- `git init --bare`
- `git remote add backup` -> tak można dodać repo i oznaczyć jako backup
- `git push --mirror backup` -> wysłanie wszystkiego do repo backupowego

## Wysyłanie zmian

- `git push origin master -u` -> "-u" ustawia mapowanie
- `git push origin my_experiment_branch:aniserowicz_#44` -> mapowanie ręczne => lokalna:zdalna
-  `git push origin :aniserowicz_#44` -> usunięcie gałęzi zdalnej
    - lub `--delete`
- `git remote rename STARA NOWA` -> zmiana zdalnej gałęzi

## Pobieranie zmian ze zdalnych repo

- `git fetch` - pobiera zdalne repo nie integrując tego z naszymi lokalnymi zmianami
- `git pull` = fetch + merge
- **BEST PRACTICE** - używać fetch


## Tagi zdalne
Domyślnie nie są wysyłane z push (chyba, że --mirror)

- `gita tag portfolio @^` -> otagowanie poprzedniego commita
- `git push origin portfolio` -> explicit wysłanie taga
- `git push --tags` -> wysyłanie wszystkich tagów (nie polecane, bo zaśmieca)
- **BEST PRACTICE** - `git push --follow-tags` - wyśle tylko tagi typu annotated
    -  `git config push.followTags true` - ustawienie globalne
- `git push origin :portfolio` -> usunięcie taga ze zdalnego repo
- `git fetch --prune --prune-tags` -> usunąc lokalnie tagi, których nie ma już na zdalnym repo
- **BEST PRACTICE** - przy tymczasowo dodanym remote używaj `git fetch --no-tags` żeby nie śmiecić sobie commitów

# Praca lokalna


## INDEX
Staging area lub cache.

- **teoria** - SRP - jeden commit = jedna rzecz, bo:
    - łatwe code review
    - łatwe cherry-pick (przenoszeni commitów)
    - identyfikacja problemów
    - komunikacja w zespole
    - nie ma czegoś takiego jak za dużo commitów

- `git add` - dodaje zmian do indexu

### Przygotowanie commita w INDEX
- `git add -i` -> interactive mode for addind to INDEX
- `git diff --cached` -> show diffs in INDEX
- `git reset` -> usuwa wszystkie zmiany w INDEX

#### Patch
- **hunk** - kawałek kodu


- **BSET PRACTICE** - `git add -p` -> patch hunk po hunku


## Czyszczenie Working Copy


- czyszczenie śledzonych plików
    - `git checkout <nic lub commit> -- index.html` - przywrócenie zawartości z aktualnego (jeśli nic) commita
        - `*.html` - też zadziała
    - `git reset --hard` - usuwa zmiany też z dysku
- czyszczenie plików nieśledzonych
    - `git clean -n` - dry run
    - `git clean -i` - interactive
    - `git clean -f` - force
    - `git clean -fdx` - kasuje też katalogi i pliki ignorowane


## Stash (schowek)

- `git stash -u`
- `git stash list/pop`
- named stash - `git stash save -u "message"`

Alternatywa
- `git add .`
- `git commit -m "msg"`
- gry chcemy wrócić to
- `git reset @^`
- **BSET PRACTICE** zamiast stash używać commita

## Ignorowanie plików
- https://gitignore.io/
- https://github.com/github/gitignore
- `git check-ignore FILE` - sprawdzenie czy plik jest ignorowany
    - `git check-ignore *cz*`
- `git update-index --assume-unchanged PLIK` - wymuszenie uznania że plik się nie zmienił. Takie ignorowanie prywatne. Ignoruje cały plik, nawet zmiany późniejsze.
    - `git ls-files -v | grep "^[[:lower:]]"` - tak można znaleźć pliki ignorowane w ten sposób
    - `git update-index --no-assume-unchanged PLIK` - powrót do śledzenia


# Konfiguracja GITa
- `git config --list`
- `git config --edit` -> edytor

Ustawienie własnego wersjonowanego konfiga do współdzielenie z innymi
- `git config --file .gitprjconfig push.followTags true` -> custom ustawienia w pliku
- `git config include.path ../.gitprjconfig` - include custom konfiga

### Warunkowa konfiguracja
- `git config -f ~/.gitconfig.lh user.email krzysztof.luczak@lh.pl` - adres email do pracy
- `git config --global includeif.gitdir:/home/krzysiek/lokalne/projekty/chicoree/repo/.path .gitconfig.lh` - ustawienie include jeśli...


# Commit messages

**BEST PRACTICE**
- 1 linia: podsumowanie
- 2 linia: pusta
- 3 linia: szczegółowy opis (jeśli potrzebny)
- każda linia max 72 znaki
- wiadomość w czasie przeszłym: Added something


# Praca zespołowa

 **BEST PRACTICE** - builder CI/CD musi zaciągać repo, więc warto podać mu `git clone --depth 1` (lub inną liczbę), żeby nie zaciągał wszystkich commitów

 ## Rozwiązywania konfliktów

- `git merge --abort` -> cofnięcie merga jeśli konflikt
- `git reset --hard ORIG_HEAD` -> powrót do zmian sprzed wykonania "niebezpiecznej" akcji
    - git zapamiętuje taki, np. merge
- `git config --global merge.tool meld` -> ustawienie merge toola na meld

#### RERERE - Reuse Recorded Resolution
- `git config rerere.enabled true` -> włączenie
- zapisuje rozwiązania dla konfliktów, które można potem reużywać, ale nie oznacza konfliktu jako rozwiązany
- **BEST PRACTICE** -> włączyć globalnie

## Modyfikacja historii
**BEST PRACTICE** - nie modyfikuj historii już wysłanej

### Revert
- po wykonaniu nieporządanego commita można zrobić revert
- `git revert @` -> zrobi nowego commita przywracającego poprzedni stan

### Amend
- `git commit --amend` -> poprawia ostatni commit msg. Modyfikuje historię
- nie używać po `PUSH`
- nie przenosi tagów, trzeba je samemu usunąć i dodać
- Commit Driven Development

### Rebase - **BEST PRACTICE** (względem merge)
- przenosi zmiany budując liniową historię
- `git checkout NIE_MASTER` -> gdy przenosimy do mastera
- `git rebase` -> potem trzeba rozwiązać konflikty
- `git rebase --continue`
- `-i` -> interactive
    - może zmieniać kolejności commitów, usuwać je itp

**TOOL**
- https://rtyley.github.io/bfg-repo-cleaner/
    - potrafi usuwać commity z całego repo, np. gdy wrzuciliśmy hasło
- filter-branch
    - można oskryptować GITa

**BEST PRACTICE** - nie używaj `git push --force`

## Rekomendowany Workflow
Zasady

0. nie robimy commitów do mastera
1. tworzymy feature branche dla każdego zadania
    - unikaj głębokich zagnieżdżeń
    - one są prywatne
2. `git push origin -u my_feature_branch:luczak/ficzer`
3. `git add -p` -> `git commit` -> `git push` regularnie
4. Porządki w historii
    - `git rebase -i master` -> interesują nas commity zrobione PO gałęzi master (wtedy przeszliśmy na naszą gałąź)
5. Integracja
    - mogę pojawić się konflikty
    - `git checkout master` -> idziemy na master
    - `git pull`
    - `git checkout -` -> wracamy do naszej gałęzi
    - `git rebase master -p` -> nakładamy master na naszą gałąź
        - `-p` spłaszczy inne odejścia w master
    - `git push`
    - **BEST PRACTICE** - włącz RERERE, rób regularnie rebase
6. Merge
    - `git checkout master`
    - `git merge --no-ff -e -m "" -` -> włącz edytor, daj pusta wiadomosc, a nie domyślną, `-` merguj poprzednią gałąź
7. Push
    - jeśli będzie błąd to trzeba cofnąć merge i od 5 jeszcze raz zrobić
    - `git push origin :luczak/ficzer` -> usuwam zdalne
    - `git branch -d feature_branch` -> usuwam lokalnie
8. Porządku
    - `git remote update origin --prune` - usuwa lokalnie czyjeś gałęzie które ktoś usunął zdalnie

TIPS
- `git log COMMIT..master --oneline --merges --reverse` - pokaż wszystkie merge commity od najstarszego do najnowszego, czyli w odwrotnej kolejności, pomiędzy wybranym commitem a masterem

## Alternatywne sposoby pracy zespołowej

1. Git-flow
    - czasem to będzie zbędna komplikacja

2. Feature toggles
    - tylko jedna gałąź
    - wszyscy commitują tam
    - nowe funkcje powstają równolegle, ale są **wyłączone**

3. Pull Request
    - szczególnie popularna w Open Source
    - fork -> commit -> pull request
    - rób PR z własnego feature brancha


## Analiza historii
blame
- pokazuje autora OSTATNIEJ zmiany
- `git blame PLIK` - ==fajne==
- `git gui blame index.html`

pickaxe
- wyszukuje commity zawierające podany tekst (w treście zmian, a nie commit msg)
- `git log -S TEXT -p` - ==fajne==

log
- `--oneline`, `--graph`, `--prety=oneline`, `--until '25 minutes ago'`
- `--format=%h`
    - `git log -S TEXT -p --format=%h` - potem skopiować i `git show COMMIT` lub `gitk COMMIT`

**mega fajny log graph**
```
git log --graph --pretty=format:'%Cred%h%Creset
%w(72,1,2)%s -%C(yellow)%d%Creset %Cgreen(%cr)
%C(bold blue)<%an>%Creset' --abbrev-commit --
date=relative
```

## Bug hunting level PRO - git bisect
1. wejść w commit gdzie nie było czegoś czego nie chcemy
2. `git bisect start`, `git bisect good` (oznaczamy aktualny jako good), `git bisect bad master` (oznaczamy master jako bad)
3. i po kolei sprawdzając zmiany robimy `git biset good lub bad`
4. na końcu git pokazuje ktory commit wprowadził złe zmiany
5. `git bisect reset`
6. Są jescze opcje `run` do automatyzacji

# Praca zespołowa - extras


## Praca dla wielu klientów - multi-tenant
np. jest jeden core, ale każdy klient ma inne mniejsze ficzery

Git-way
- core na master
- feature branch per client (niestety max 5, bo sie nie skaluje)
- to nie jest problem GITa - to jest multi-tenant app, rozwiązanie na poziomie kodu - architektoniczne


## GIT a pliki binarne
- https://git-lfs.github.com/ - transperente, przechowuje wybrane pliki binarne (zdjęcia, itp) w oddzielnym miejscu. Github, gitlab, bitbucket mają to wbudowane


## Hooks
- lokalne i zdalne
- .git/hooks
- return code inny niż 0 powoduje odrzucenie operacji
- hooki powinny mieć chmod +x


## Projekty współdzielone
- **BEST PRACTICE** - twórz oddzielno repo dla każdego projektu/biblioteki

Submodules
- `git submodule add --depth 10 https://github.com/jquery/jquery.git`
    - repo jako plik
    - skomplikowane
    - każdy w projekcie musi znać i umieć obsłużyć submodules
    - nie używać

Gitslave
- jedno repo master
- wiele sub-repozytoriów - slaves
- komendy wykonywane są we wszystkich repo, np. wszedzie powstają gałęzie
- http://gitslave.sourceforge.net/

Repo
- narzędzie Google
- działa podobnie do gitslaves i submodules
- https://github.com/ingydotnet/git-subrepo

Subrepo
- kopia kodu jako jeden commit
- trzeba uważać z commitami
- https://github.com/ingydotnet/git-subrepo

Subtree
- https://github.com/git/git/tree/master/contrib/subtree

Wnioski
- nie robić tego przez gita, tylko poprzez zarządzanie zależnościami/paczkami menadżerem