page_title: Work on your issue
page_description: Basic workflow for Docker contributions
page_keywords: contribute, pull request, review, workflow, beginner, squash, commit


# Work on your issue

The work you do for your issue depends on the specific issue you picked.
This section gives you a step-by-step workflow. Where appropriate, it provides
command examples. 

However, this is a generalized workflow, depending on your issue you may repeat
steps or even skip some. How much time the work takes depends on you --- you
could spend days or 30 minutes of your time.

## How to work on your local branch

Follow this workflow as you work:

1. Review the appropriate style guide.

    If you are changing code, review the <a href="../coding-style"
    target="_blank">coding style guide</a>. Changing documentation? Review the
    <a href="../doc-style" target="_blank">documentation style guide</a>. 
	
2. Make changes in your feature branch.

    Your feature branch you created in the last section. Here you use the
    development container. If you are making a code change, you can mount your
    source into a development container and iterate that way. For documentation
    alone, you can work on your local host. 

    Make sure you don't change files in the `vendor` directory and its
    subdirectories; they contain third-party dependency code. Review <a
    href="../set-up-dev-env" target="_blank">if you forgot the details of
    working with a container</a>.


3. Test your changes as you work.

    If you have followed along with the guide, you know the `make test` target
    runs the entire test suite and `make docs` builds the documentation. If you
    forgot the other test targets, see the documentation for <a
    href="../test-and-docs" target="_blank">testing both code and
    documentation</a>.  
	
4. For code changes, add unit tests if appropriate.

    If you add new functionality or change existing functionality, you should
    add a unit test also. Use the existing test files for inspiration. Aren't
    sure if you need tests? Skip this step; you can add them later in the
    process if necessary.
	
5. Format your source files correctly.

    <table>
      <thead>
      <tr>
        <th>File type</th>
        <th>How to format</th>
      </tr>
      </thead>
      <tbody>
      <tr>
        <td><code>.go</code></td>
        <td>
            <p>
            Format <code>.go</code> files using the <code>gofmt</code> command.
            For example, if you edited the `docker.go` file you would format the file
            like this:
            </p>
            <p><code>$ gofmt -s -w docker.go</code></p>
            <p>
            Most file editors have a plugin to format for you. Check your editor's
            documentation.
            </p>
        </td>
      </tr>
      <tr>
        <td style="white-space: nowrap"><code>.md</code> and non-<code>.go</code> files</td>
        <td>Wrap lines to 80 characters.</td>
      </tr>
      </tbody>
    </table>

6. List your changes.

        $ git status
        On branch 11038-fix-rhel-link
        Changes not staged for commit:
          (use "git add <file>..." to update what will be committed)
          (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   docs/sources/installation/mac.md
        modified:   docs/sources/installation/rhel.md

    The `status` command lists what changed in the repository. Make sure you see
    the changes you expect.

7. Add your change to Git.

        $ git add docs/sources/installation/mac.md
        $ git add docs/sources/installation/rhel.md


8. Commit your changes making sure you use the `-s` flag to sign your work.

        $ git commit -s -m "Fixing RHEL link"

9. Push your change to your repository.

        $ git push origin
        Username for 'https://github.com': moxiegirl
        Password for 'https://moxiegirl@github.com': 
        Counting objects: 60, done.
        Compressing objects: 100% (7/7), done.
        Writing objects: 100% (7/7), 582 bytes | 0 bytes/s, done.
        Total 7 (delta 6), reused 0 (delta 0)
        To https://github.com/moxiegirl/docker.git
         * [new branch]      11038-fix-rhel-link -> 11038-fix-rhel-link
        Branch 11038-fix-rhel-link set up to track remote branch 11038-fix-rhel-link from origin.

## Review your branch on GitHub

After you push a new branch, you should verify it on GitHub:

1. Open your browser to <a href="https://github.com" target="_blank">GitHub</a>.

2. Go to your Docker fork.

3. Select your branch from the dropdown.

	![Find branch](/project/images/locate_branch.png)
	
4. Use the "Compare" button to compare the differences between your branch and master.

	 Depending how long you've been working on your branch, your branch maybe
	 behind Docker's upstream repository. 
	 
5. Review the commits.

	 Make sure your branch only shows the work you've done.
	 
## Pull and rebase frequently

You should pull and rebase frequently as you work.  

1. Return to the terminal on your local machine.

2. Make sure you are in your branch.

		$ git checkout 11038-fix-rhel-link

3. Fetch all the changes from the `upstream master` branch.

		 $ git fetch upstream master

  	This command says get all the changes from the `master` branch belonging to
  	the `upstream` remote.

4. Rebase your master with the local copy of Docker's `master` branch.

		 $ git rebase -i upstream/master
   
  	This command starts an interactive rebase to rewrite all the commits from
	Docker's `upstream/master` onto your local branch, and then re-apply each of
	your commits on top of the upstream changes. If you aren't familiar or
  	comfortable with rebase, you can <a
  	href="http://nathanleclaire.com/blog/2014/09/14/dont-be-scared-of-git-
  	rebase" target="_blank">learn more about rebasing</a> on the web.
  
5. Rebase opens an editor with a list of commits.

			pick 1a79f55 Tweak some of the other text for grammar 
			pick 53e4983 Fix a link 
			pick 3ce07bb Add a new line about RHEL
        
  	If you run into trouble, `git --rebase abort` removes any changes and gets
  	you back to where you started. 

6. Squash the `pick` keyword with `squash` on all but the first commit.

			pick 1a79f55 Tweak some of the other text for grammar
			squash 53e4983 Fix a link
			squash 3ce07bb Add a new line about RHEL

  	After closing the file, `git` opens your editor again to edit the commit
  	message. 

7. Edit the commit message to reflect the entire change.

	Make sure you include your signature.

8. Push any changes to your fork on GitHub.

	The rebase rewrote history, so you'll need to use the `-f` or `--force` flag
	to push your change.

		$ git push -f origin 11038-fix-rhel-link


## Where to go next

At this point, you should understand how to work on an issue. In the next
section, you [learn how to make a pull request](/project/create-pr/).
