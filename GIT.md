### Materiały:
https://git-scm.com/doc
https://www.atlassian.com/git/tutorials

### Untrack single file
To untrack a single file that has already been added/initialized to your repository, i.e., stop tracking the file but not delete it from your system use:
git rm --cached filename


### gitk
https://lostechies.com/joshuaflanagan/2010/09/03/use-gitk-to-understand-git-merge-and-rebase/

gitk          - show only current branch
gitk --all    - show all branches


### wdrożenie
```
git checkout develop
git merge --no-ff feature/HOSTING-XXX
```
w tym momencie wersja develop to 1.2.2-SNAPSHOT
```
git checkout stable
```
w tym momencie wersja stable to 1.2.1-20160517
zakładam, że uprzednio był commit na stable, podbijający wersję, revert 
```
git revert 
```
w tym momencie wersja stable to 1.2.1-SNAPSHOT, czyli poprzednia wersja developa
```
git merge --no-ff develop 
```
w tym momencie wersja stable to 1.2.2-SNAPSHOT, czyli aktualna wersja developa, nie powinien wystąpić konflikt
Nie jest potrzebne wykonanie komendy:
git commit -m 'Merge branch develop into stable' 
(git?) sam robi commita.

Zakładając, że dziś jest 2016-05-20
```
./update-version.sh <ostatni tag> <nowy tag>, 
sh ./update-version.sh 1.2.2-SNAPSHOT 1.2.2-20160520  
```
Ostatni wiersz wyjścia z komendy spowoduje zakomitowanie zmiany wersji
czyli:
git add #### wszystkie pom.xml
git commit -m 'Update version to 1.2.2-20160520'
```
git tag v1.2.2-20160520
git checkout develop
```
```
sh ./update-version.sh 1.2.2-SNAPSHOT 1.2.3-SNAPSHOT 
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



##

