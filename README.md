# sim-private
This repo is to simulate one private repo. 10 committs will be created.

2nd companion repo will simulate public repo as another companies's upstream repo. 

Below is the workflow to verify:
1. Alice shallow clone this private repo and takes 5 commits
2. Alice push the shallown clone repo to 2nd repo
3. Bob fork 2nd repo, and pushed his change to his own fork

Below is the git operation
$ git clone --depth 5 https://github.com/leslie-qiwa/sim-private.git sim-public
$ cd sim-public
$ git remote remove origin
# Store the hash of the oldest commit (ie. in this case, the 50th) in a var
$ START_COMMIT=$(git rev-list main|tail -n 1)
# Checkout the oldest commit; detached HEAD
$ git checkout $START_COMMIT
   
# Create a new orphaned branch, which will be temporary
$ git checkout --orphan temp_branch

# Modify readme with these commands
$ vi README.md
$ git commit -a -m "add git commands into readme"

# Now that we have that initial commit, we're ready to replay all the other commits on top of it, in order, so rebase master onto it, except for the oldest commit whose parents don't exist in the shallow clone... it has been replaced by our 'initial commit'
$ git rebase --onto temp_branch $START_COMMIT main
