### Untrack single file
To untrack a single file that has already been added/initialized to your repository, i.e., stop tracking the file but not delete it from your system use:
git rm --cached filename


### gitk
https://lostechies.com/joshuaflanagan/2010/09/03/use-gitk-to-understand-git-merge-and-rebase/

gitk          - show only current branch
gitk --all    - show all branches


### wdrożenie
git checkout develop
git merge --no-ff feature/HOSTING-XXX

#### w tym momencie wersja develop to 1.2.2-SNAPSHOT

git checkout stable

#### w tym momencie wersja stable to 1.2.1-20160517

git revert #### zakładam, że uprzednio był commit na stable, podbijający wersję, revert 

#### w tym momencie wersja stable to 1.2.1-SNAPSHOT, czyli poprzednia wersja developa

git merge --no-ff develop 

#### w tym momencie wersja stable to 1.2.2-SNAPSHOT, czyli aktualna wersja developa, nie powinien wystąpić konflikt

#### git commit -m 'Merge branch develop into stable' ??? - czy to jest potrzebne, czy sam się robi ten commit przy merge ???
#### to niepotrzebne, 

#### ./update-version.sh <ostatni tag> <nowy tag>, 
sh ./update-version.sh 1.2.2-SNAPSHOT 1.2.2-20160520  #### dziś jest 2016-05-20

#### Ostatni wiersz wyjścia z komendy spowoduje zakomitowanie zmiany wersji
#### czyli:
#### git add #### wszystkie pom.xml
#### git commit -m 'Update version to 1.2.2-20160520'

git tag v1.2.2-20160520

git checkout develop

sh ./update-version.sh 1.2.2-SNAPSHOT 1.2.3-SNAPSHOT 


#### pytanie - czy tej zmiany wersji na stable z 1.2.1-SNAPSHOT na 1.2.2-20160520 nie można by robić na developie, następnie mergować do stable, a następnie zmieniać wersję developa na 1.2.3-SNAPSHOT? Wówczas nie byłoby konfliktu
#### było kilka commitów bez git revert na stable - wówczas rozumiem, że ręczne rozwiązanie konfliktu?



