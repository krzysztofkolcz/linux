===Grep throug .gz files===
find -name \*.gz -print0 | xargs -0 zgrep 'doDowngradeHostingToPackage'
