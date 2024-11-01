### Uwieżytelnianie
#### Token
Wygenerowanie nowego tokena:
1. Wchodze na:
2. Authentication code (Telefon, Authenticator)
3. Wybieram zakres tokenu i generuje. Wygenerowany kopiuje (Desktop/github/git-https-passwords.txt)
4. Przy push podaje:
Username: krzysztofkolcz
Password: <token>

5.  git config --global credential.helper store


#### Uwierzytelnianie przez ssh
ssh-keygen -t rsa -b 4096 -C "krzysztof.kolcz@gmail.com"
sudo apt-get install xclip
xclip -sel clip < ~/.ssh/id_rsa.pub

github.com -> settings -> ssh and gpg keys -> New SSH key

git remote set-url origin git@github.com:krzysztofkolcz/linux.git


### Materiały:
https://git-scm.com/doc
https://www.atlassian.com/git/tutorials


### Untrack single file
To untrack a single file that has already been added/initialized to your repository, i.e., stop tracking the file but not delete it from your system use:
git rm --cached filename

## dlete files 
many, without doing git rm for each of them
git add -u


### gitk
https://lostechies.com/joshuaflanagan/2010/09/03/use-gitk-to-understand-git-merge-and-rebase/

gitk          - show only current branch
gitk --all    - show all branches


### wdrożenie
```
git checkout develop
git merge --no-ff feature/HOSTING-XXX
```
dla remote moge zrobic:
```
git merge --no-ff origin/feature/HOSTING-XXX
```
Nastepnie usuwam feature branche lokalne oraz remote:
```
git branch -d feature/HOSTING-XXX
git push origin --delete feature/HOSTING-XXX
```

w tym momencie wersja develop to 1.2.2-SNAPSHOT (1.2.6-SNAPSHOT)
```
git checkout stable
```
w tym momencie wersja stable to 1.2.1-20160517 (1.2.5-20160629)
zakładam, że uprzednio był commit na stable, podbijający wersję, revert 
```
git revert HEAD
```
w tym momencie wersja stable to 1.2.1-SNAPSHOT (1.2.5-SNAPSHOT), czyli poprzednia wersja developa
```
git merge --no-ff develop 
```
w tym momencie wersja stable to 1.2.2-SNAPSHOT (1.2.6-SNAPSHOT), czyli aktualna wersja developa, nie powinien wystąpić konflikt
Nie jest potrzebne wykonanie komendy:
git commit -m 'Merge branch develop into stable' 
(git?) sam robi commita.

Zakładając, że dziś jest 2016-05-20
```
./update-version.sh <ostatni tag> <nowy tag>, 
sh ./update-version.sh 1.2.2-SNAPSHOT 1.2.2-20160520  
(sh ./update-version.sh 1.2.6-SNAPSHOT 1.2.6-20160707)
```
Muszę chyba ręcznie przeprowadzić te komeny, które wyrzuca update-version.sh
```
git status
find -name '*.versionsBackup' -exec rm \{\} \;
git status
git add -A
git diff --cached
git commit -m "Update version to  1.2.2-20160520"
(git commit -m "Update version to 1.2.6-20160707")
git tag 1.2.2-20160520
(git tag v1.2.6-20160707)
```


```
git checkout develop
sh ./update-version.sh 1.2.2-SNAPSHOT 1.2.3-SNAPSHOT 
(sh ./update-version.sh 1.2.6-SNAPSHOT 1.2.7-SNAPSHOT )

git status
find -name '*.versionsBackup' -exec rm \{\} \;
git status
git add -A
git diff --cached
git commit -m "Update version to  1.2.3-SNAPSHOT"
(git commit -m "Update version to 1.2.7-SNAPSHOT")
```
Nie taguje developa

Wrzucam zmiany do repo
```
git push 
git checkout stable
git push 
```
Wrzucam taga do repo
```
git push origin v1.2.2-20160520
(git push origin v1.2.6-20160707)
```

pytanie - czy tej zmiany wersji na stable z 1.2.1-SNAPSHOT na 1.2.2-20160520 nie można by robić na developie, następnie mergować do stable, a następnie zmieniać wersję developa na 1.2.3-SNAPSHOT? Wówczas nie byłoby konfliktu
odp. Można, ale wtedy pewnie większy bałagan.
Było kilka commitów bez git revert na stable - wówczas rozumiem, że ręczne rozwiązanie konfliktu?

#### Ćwiczenie z przeprowadzenia wdrożenia
dwa foldery /try_git/ oraz /BeginningHibernate/try_git2/try_git/
wersja stable - 1.0.0-20160520
wersja develop - 1.0.1-SNAPSHOT
```
/try_git/
git checkout develop
git checkout -b feature/TRYGIT-3
touch file7.txt
git add file7.txt
git commit -m 'file7.txt added'
git checkout develop
git merge --no-ff feature/TRYGIT-3
git checkout stable
git revert HEAD??? / previous commit??? /  - tworzy nowego commita, z cofniętym numerem wersji (stable ma teraz wersję 1.0.0-SNAPSHOT)
git merge --no-ff develop  //wersja stable - 1.0.1-SNAPSHOT
//update-version.sh z 1.0.1-SNAPSHOT na 1.0.1-20160523 - to załatwia git add version.txt, git commit -m 'Update version to 1.0.1-20160523'
git checkout develop
//update-version.sh z 1.0.1-SNAPSHOT na 1.0.2-SNAPSHOT - to załatwia git add version.txt, git commit -m 'Update version to 1.0.2-SNAPSHOT'
git push
git checkout stable
git push
git branch -d feature/TRYGIT-3 //TODO - jak usunąć branch z remota? Nie dodałem tego brancha na remota, więc jeszcze raz przećwiczyć
```


### Zmiana historii 
feature/TRYGIT-4
na branchu dodaje pliki a.txt oraz b.txt
robię commita i pusha
modyfikuję plik a.txt
robię commita i pusha
chcę całkowicie usunąć plik a.txt z historii

Commit 1
         \
          -- Commit 2 -- Commit 3

Ponieważ branch jest publiczny, nie mogę skorzystać z reseta, więc muszę skorzystać z reverta, co zachowa mi plik w historii, ale go usunie.
git revert <commit> a.txt - nie działa - fatal bad revision a.txt

git reset <commit> --hard //resetuje do punktu przed dodaniem plików, plików nie ma na dysku ani w staging area. Co dziwne, w gitk --all widać brancha
git checkout develop
git branch -d feature/TRYGIT-4 //wywala brancha, ale branch jest ciągle zdalnie
git push origin --delete <branchName>

feature/TRYGIT-5
robię to samo bez pushowania na remota z plikami x.txt i y.txt
git reset <commit> x.txt
muszę zrobić commit
git commit -m 'x.txt reset'
nie usuwa pliku z historii, ale wywala go z gita...

git checkout develop
git branch -d feature/TRYGIT-5
error: branch is not fully merged
git branch -D feature/TRYGIT-5


feature/TRYGIT-6
robię to samo bez pushowania na remota z plikami c.txt i d.txt
git reset <commit> --soft  //resetuje do punktu przed dodaniem plików, pliki są w staging area (jako new)
git reset <commit> --mixed  //resetuje do punktu przed dodaniem plików, pliki są na dysku, ale nie ma ich w staging area
rm c.txt
rm d.txt //usuwanie z dysku

git branch -d feature/TRYGIT-6


Checkout zdalnego brancha do lokalnego (z utworzeniem lokalnego brancha o podanej nazwie):
git checkout -b sf origin/serverfix
Tworzy lokalnego brancha sf trackującego zdalnego brancha origin/serverfix


### TODO
git revert 
cherry pick?

### Ustawienie gita, bez podawania usera i hasła w ssh:
git remote set-url origin git@github.com:krzysztofkolcz/try_git.git
Przy klonowaniu nowego repozytorium używać ssh zamiast https



## Przeniesienie commitów z jednego brancha do drugiego 

```
Branch master           Commit 1
                                \
Branch feature-1                 -- Commit 2 -- Commit 3 -- Commit 4 -- Commit 5
```

Chcę otrzymać:

```
Branch master           Commit 1 -- Commit 4 -- Commit 5
                                \
Branch feature-1                 -- Commit 2 -- Commit 3 
```

On branch master
```
git log
```

commit 7d0f00ba9510e618cc5b83dbd5ceb8bb50f93551
  file8 changed

commit bcdb04804a81d0eb168f32fd3c1a1debdc320bc5
  file8 added - commit to be moved on top of develop

commit 594580d089957b49c5384317e5984f20616a2e4c
  file7 changed

commit 0f6c63cace9e2a2d7fe030b1d013a2d4c023982c
  file7 changed

commit b613942a17d642ceaa130d5dbc6f242d8f97905d
  Update vesion to 1.0.2-SNAPSHOT


Testuje cherry pick:
http://think-like-a-git.net/sections/rebase-from-the-ground-up/cherry-picking-explained.html

```
git checkout develop
git cherry-pick bcdb04804a81d0eb168f32fd3c1a1debdc320bc5
git cherry-pick 7d0f00ba9510e618cc5b83dbd5ceb8bb50f93551

```

Otrzymałem takie coś:

```
Branch master           Commit 1 -- Commit 4` -- Commit 5`
                                \
Branch feature-1                 -- Commit 2 -- Commit 3 -- Commit 4 -- Commit 5
```


TODO
usunięty został plik. Dowiedzieć się, w którym commicie i w którym branchu

git diff --cached
git commit -m "Update version to 1.2.3-20160610"
git tag v1.2.3-20160610



Wrzucenie bug fixa do developa i na stable, bez konfliktów w wersjach
1. Muszę cofnąć wersję developa?
2. Muszę cofnąć wersję stable?
3. Dodać poprawkę do developa
4. Domergować developa do stable
5. Ustawić ponownie wersje dla develop i stable?


1. git checkout develop; git revert HEAD; develop: 1.2.6-SNAPSHOT -> 1.2.5-SNAPSHOT
2. git checkout stable; git revert HEAD; stable 1.2.5-20160628 -> 1.2.5-SNAPSHOT
3. git checkout develop; git merge origin/featuer/HOSTING-2886
4. git checkout stable; git merge develop;
5. update version (stable)
sh ./update-version.sh 1.2.5-SNAPSHOT 1.2.5-20160629
6. update version (develop)
sh ./update-version.sh 1.2.5-SNAPSHOT 1.2.6-SNAPSHOT




## cofnięcie merga przed rozwiązaniem konfliktow
git merge branch
git merge --abort

## copy remote branch to local
git checkout -b branch_name origin/branch_name

## rebase
git checkout branch
git rebase master
git push --force origin branch


## git config - ingnore file permissions
git config core.fileMode false

## resolve easy conflicts
git checkout --ours PATH/FILE
git checkout --theirs PATH/FILE
