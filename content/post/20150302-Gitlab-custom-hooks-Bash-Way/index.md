+++
title = 'Gitlab custom hooks - Bash Way'
date = 2015-03-02T13:20:23+02:00
draft = false
tags = [ 'Git', 'GitLab' ]
categories = [ 'SoftwareFactory' ]
image = 'gitlab-hook.png'
+++

If you’re using Gitlab, you may know you could use custom hooks to validate contents. Everything is detailed in [http://doc.gitlab.com/ce/hooks/custom_hooks.html](http://doc.gitlab.com/ce/hooks/custom_hooks.html)

I found many examples where hooks are Ruby based and using Gitlab APIs but I wanted something in good old bash.

I found some bash hooks, [https://github.com/Praqma/git-hooks](https://github.com/Praqma/git-hooks), but 7 months old. They still works with Gitlab 7.8.1 and adapted them for my purposes.

Here is a sample pre-receive hook :

```
#!/bin/bash
#
# pre-receive hook for Commit Check
#

COMPANY_EMAIL="mycorp.org"

readonly PROGNAME=$(basename $0)
readonly PROGDIR=$(readlink -m $(dirname $0))

check_single_commit()
{
  COMMIT_CHECK_STATUS=0
  #
  # Put here any logic you want for your commit
  #
  # COMMIT_MESSAGE contains commit message
  # COMMIT_AUTHOR contains commit author (without email)
  #
  # Set COMMIT_CHECK_STATUS to non zero to indicate an error
}

check_all_commits()
{
  REVISIONS=$(git rev-list $OLD_REVISION..$NEW_REVISION)
  IFS='\n' read -ra LIST_OF_REVISIONS <<< "$REVISIONS"

  for rid in "${!LIST_OF_REVISIONS[@]}"; do
    REVISION=${LIST_OF_REVISIONS[rid]}
    COMMIT_MESSAGE=$(git cat-file commit $REVISION | sed '1,/^$/d')
    COMMIT_AUTHOR=$(git cat-file commit $REVISION | grep committer | sed 's/^.* \([^@ ]\+@[^ ]\+\) \?.*$/\1/' | sed 's/<//' | sed 's/>//' | sed 's/@$COMPANY_EMAIL//')
    check_single_commit

    if [ "$COMMIT_CHECK_STATUS" != "0" ]; then
      echo "Commit validation failed for commit $REVISION" >&2
      exit 1
    fi

  done
}

# Get custom commit message format
while read OLD_REVISION NEW_REVISION REFNAME ; do
  check_all_commits
done

exit 0
```

Install is easy (example with Omnibus based Gitlab) :

```
mkdir /var/opt/gitlab/git-data/repositories/MYGROUP/MYREPO.git/custom_hooks
cp pre-receive /var/opt/gitlab/git-data/repositories/MYGROUP/MYREPO.git/custom_hooks/pre-receive
chmod 755 /var/opt/gitlab/git-data/repositories/MYGROUP/MYREPO.git/custom_hooks/pre-receive
chown -R git:git /var/opt/gitlab/git-data/repositories/MYGROUP/MYREPO.git/custom_hooks
```
