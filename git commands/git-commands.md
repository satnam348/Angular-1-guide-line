# Useful Git commands


#### Line Endings

http://stackoverflow.com/questions/9976986/force-lf-eol-in-git-repo-and-working-copy

```bash
$ git config core.eol lf
$ git config core.autocrlf input
```


#### show list of commit logs (1st line only) grouped by author

`git shortlog --after="2016-10-25"`

#### List all changes to non spec files between two SHA grouped by angular object type

```
    git diff --stat=300 9e2b69b 43de47c | grep angular-ui/ | grep \.service\.    | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep angular-ui/ | grep \.controller\. | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep angular-ui/ | grep \.factory\.    | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep angular-ui/ | grep \.directive\.  | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep angular-ui/ | grep \.filter\.     | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep angular-ui/ | grep \.component\.  | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
```

#### List all changes to spec files between two SHA grouped by angular object type

```
    git diff --stat=300 9e2b69b 43de47c | grep \.service\.    | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep \.controller\. | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep \.factory\.    | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep \.directive\.  | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep \.filter\.     | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    git diff --stat=300 9e2b69b 43de47c | grep \.component\.  | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
```

#### use a bash function

```bash
function listDiff() {
    if [ -z $1 -o -z $2 ]; then
        echo 'require a begin and end sha1 to search';
        return;
    fi

    local sha1=$1;
    local sha2=$2;

    local objectPatterns=(\\.service\\. \\.controller\\. \\.factory\\. \\.directive\\. \\.filter\\. \\.component\\.);

    for pat in "${objectPatterns[@]}"
    do
        echo -e ‘
—— $pat ———————————
’
        git diff --stat=300 $sha1 $sha2 | grep angular-ui/ | grep $pat    | grep -v "/[^/]*\.spec\.js " | grep "/[^/]*\.js"
    done
}
```


#### update author information that is submitted with each commit/push
 

```bash
git config --global user.name
git config --global user.email
```



##  cleaning-up your local repository:
```bash
$ git gc --prune=now
$ git remote prune origin
```

### -- for more information

##### git garbage collection
```bash
man git-gc(1):

git-gc - Cleanup unnecessary files and optimize the local repository

git gc [--aggressive] [--auto] [--quiet] [--prune=<date> | --no-prune]

       Runs a number of housekeeping tasks within the current repository, such as compressing file revisions
       (to reduce disk space and increase performance) and removing unreachable objects which may have been
       created from prior invocations of git add.

       Users are encouraged to run this task on a regular basis within each repository to maintain good disk
       space utilization and good operating performance.
```

##### git orphaned remote tracking removal

```bash
man git-remote(1):

git-remote - manage set of tracked repositories

git remote prune [-n | --dry-run] <name>

           Deletes all stale remote-tracking branches under <name>. These stale branches have already been
           removed from the remote repository referenced by <name>, but are still locally available in
           "remotes/<name>".    
```
