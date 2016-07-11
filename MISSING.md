git checkout stable
git revert HEAD ```ok
```1.2.6-20160707 -> 1.2.6-SNAPSHOT

git checkout develop
git revert HEAD ```ok
```1.2.7-SNAPSHOT -> 1.2.6-SNAPSHOT

git merge --no-ff origin/feature/HOSTING-2898
git checkout stable
git merge develop

git checkout develop

TODO
sh ./update-version.sh 1.2.6-SNAPSHOT 1.2.7
```update version 
```1.2.6-SNAPSHOT -> 1.2.7-SNAPSHOT


git checkuot stable
sh ./update-version.sh 1.2.6-SNAPSHOT 1.2.6-20170708
```update version
```1.2.6-SNAPSHOT -> 1.2.6-20160708

