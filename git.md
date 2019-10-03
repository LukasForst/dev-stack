### Revert last commit
```bash
# reset to last change in head
git reset HEAD~
# do necessary changes
...
# add things again
git add .
# commit again
git commit -c ORIG_HEAD
```

### Remove files that are in gitignore
NOTE: everything will be removed, commit your changes

```bash
# remove all not tracked data
git rm -r --cached .
# add everything again
git add .
# commit it
git commit -m ".gitignore fix"
# push to the repo
git push
```

### Parse Jira task from branch and add to commit
Put following code into `.git/hooks/commit-msg` and make it executable via  `chmod +x .git/hooks/commit-msg`
```bash
# Add git branch if relevant
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
# Extract tracker abbreviation and ticket number (e.g. DS-123)
parse_git_tracker_and_ticket() {
  parse_git_branch | grep -e '[A-Z]\+-[0-9]\+' -o
}
MESSAGE="$(cat $1)"
TICKET=`parse_git_tracker_and_ticket`
if [ -n "$TICKET" ]
then
   echo "New commit message: [$TICKET] $MESSAGE"
   echo "$TICKET - $MESSAGE" > $1
fi
```