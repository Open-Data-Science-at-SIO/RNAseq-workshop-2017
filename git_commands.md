Git Commands
by Luke Thompson, NOAA
=====

### Creating your own repository

<http://kbroman.org/github_tutorial/pages/init.html>

### Forking an existing repository

On GitHub, go the your favorite repository, such as qiime:

`https://github.com/biocore/qiime`

Click fork, which will create a copy of the forked repository (and branch, generally master by default).

On the command line, the following will create a ./qiime directory (replace `myusername` with your username):

	$ git clone https://github.com/myusername/qiime
	$ cd qiime
 
Update your remotes, which allows you to get updates from the master "origin" repository (biocore/qiime in this case).

We're going to call this upstream as it is upstream of your fork since other people merge to it:

	$ git remote add upstream https://github.com/biocore/qiime
 
Sync just to be sane; we're going to pull down the master branch from upstream:

	$ git pull upstream master
 
Sync your fork on github:

	$ git push
 
Now we can do some coding.
 
Create a new branch (called "foo") and check it out:

	$ git checkout -b foo
 
Edit files, create files, delete files, etc.:

	$ git add <each file or directory you want to commit>
	$ git mv <old file> <new file>
	$ git commit -m "Comments about commit"
	... repeat ...
 
Push to origin (your fork) the branch you're working on:

	$ git push origin foo

Go to your github fork page, select the branch and click "issue pull request".

(Optional) Remove local branch:

	$ git branch -d foo

(Optional) Delete remote branch AND local branch:

    $ git push origin --delete <branch_name>
    $ git branch -d <branch_name>

(Status) Shows list of existing remotes with remote url after name:

	$ git remote -v
	
See log of commits:

	$ git log
	
Download someone else's branch:

	$ git remote add username https://github.com/username/emp.git (use username for sanity)
	$ git checkout -b branchname (use same name as the person issuing PR just for sanity)
	[should be another command to pull their branch]

Note: upstream is a location on the internet, which can have more branches than master in upstream (but not usually).

It seems like a bit much at first, but gets to become very natural very quickly. Also, Yoshiki (ElDeveloper) has a nifty git function that updates your prompt to show what repo you're in (code below). And, last, you can add tab completion to git so you can tab out commands, branches, etc. Very handy, can be found here:
 
`https://github.com/git/git/blob/master/contrib/completion/git-completion.bash`
 
Add to your `~/.bash_profile` (written originally by Yoshiki Vazquez Baeza):

	function egit (){
	    # git specific usage for branching
	    function branch_separator () {
	        #git name-rev HEAD 2> /dev/null | sed 's#HEAD\ \(.*\)#(git::\1) #'
	        if [[ -n $(git rev-parse --abbrev-ref HEAD 2> /dev/null) ]]
	        then
	        echo "@"
	        fi
	    }
	    function get_git_branch () {
	        git rev-parse --abbrev-ref HEAD 2> /dev/null
	    }
	    export PS1='\[\033[1;32m\]\t \[\033[0m\]\W$(branch_separator)\[\e[m\]\[\e[01;38;5;196m\]$(get_git_branch)\[\e[m\]$ '
	}
	function dgit (){
	    export PS1='\[\033[1;32m\]\t \[\033[0m\]\W$ '
	}
 
Enable/disable git-flavored prompt:

	$ egit
	... some git magic ...
	$ dgit

Add this if you have the git bash completion, or other bash completion scripts:

	for f in ~/.bash_completion.d/*;
	do
	    source $f;
	done
