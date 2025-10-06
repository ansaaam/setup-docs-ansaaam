## Backtest Release Management

1. Versioning
    - A hotfix or bug fix increments the patch version
    - This increment can happen via:
        - A hotfix branch, or
        - A normal release process (if the last commit is a bug fix)

2. Brancheing strategy
    - Always create branches from the tag corresponding to the production version. Naming convention:
      - Hotfix: hotfix/\<version\>
      - Bugfix: bugfix/\<ticket-id\>-\<description\>

3. Steps for pushing on hotfix branch
```
# Checkout from master tag and create hotfix branch
git checkout tags/v250529.96.0 -b hotfix/v250529.96.0

# Push hotfix branch to remote
git push origin hotfix/v250529.96.0
```

4. Steps for pushing to bugfix branch 
```
git checkout tags/v250529.97.4 -b bugfix/IEI-3009-missing-column-data
git push origin bugfix/IEI-3009-missing-column-data
```

5. Seperate CICD pipeline will be automatically triggerred for hotfix

> p.s. On sandbox, at a time, changes of only one hotfix can be seen as merging PR of another hotfix will override the changes of previous hotfix