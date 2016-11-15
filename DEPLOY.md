1.2.5-20160629 - aktualna wersja stable
1.2.6-SNAPSHOT - aktualna wersja develop
1.2.5-SNAPSHOT - poprzednia wersja develop oraz stable
1.2.7-SNAPSHOT - kolejna wersja develop
1.2.6-20160707 - kolejna wersja stable
```
git checkout develop
git merge --no-ff feature/HOSTING-XXX
git merge --no-ff origin/feature/HOSTING-XXX

git branch -d feature/HOSTING-XXX
git push origin --delete feature/HOSTING-XXX
git checkout stable


```
w tym momencie wersja: 
develop to 1.2.6-SNAPSHOT 
stable to 1.2.5-20160629
```
git revert HEAD
```
w tym momencie wersja stable to 1.2.5-SNAPSHOT, (poprzednia wersja developa)
```
git merge --no-ff develop 
```
w tym momencie wersja stable to 1.2.6-SNAPSHOT, (aktualna wersja developa)
```
sh ./update-version.sh 1.2.6-SNAPSHOT 1.2.6-20160707
```
git status
find -name '*.versionsBackup' -exec rm \{\} \;
git status
git add -A
git diff --cached
git commit -m "Update version to 1.2.6-20160707"
git push
git tag v1.2.6-20160707
git push origin v1.2.6-20160707
```
w tym momencie wersja stable to 1.2.6-20160707 otagowana tagiem v1.2.6-20160707
```
git checkout develop
sh ./update-version.sh 1.2.6-SNAPSHOT 1.2.7-SNAPSHOT
git status
find -name '*.versionsBackup' -exec rm \{\} \;
git status
git add -A
git diff --cached
git commit -m "Update version to 1.2.7-SNAPSHOT"
git push 
```
w tym momencie wersja develop to 1.2.7-STABLE
