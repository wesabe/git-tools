git-tools
=========

git-tools is a collection of scripts and tools to make developing with git and hosting a git repo easier.

Creating a Repository
---------------------

The `mkgit` command is provided to create a git repository on the machine you intend to use to host the repo. It assumes you have a user named `git` and will try to `sudo` as `git` if it isn't run as `git`.

It takes a single argument: the name of the project.

    git@code-$ mkgit project-one
    Creating repo project-one.git
    Initialized empty Git repository in /tmp/project-one.git/
    
    Next steps:
      mkdir project-one
      cd project-one
      git init
      touch README
      git add README
      git commit -m 'first commit'
      git remote add origin git@code:project-one.git
      git push origin master
    
    Existing Git Repo?
      cd existing_git_repo
      git remote add origin git@code:project-one.git
      git push origin master

When you're done, it prints instructions on how to set up the repo on your client (dev) machine. Kudos to github for thinking of this.

github-style post-receive hook
------------------------------

The `post-receive` script will send a JSON payload to all the uris listed in the repo's `hooks.uris` configuration option. Most likely you'll want to set it on a system level. Example:

    git@code-$ git config -l
    uris.project=http://code.example.com/projects/show/%name%
    uris.commit=http://code.example.com/repositories/revision/%name%?rev=%id%
    hooks.uris=http://localhost:8001/

The above will post to `http://localhost:8001/` with the JSON payload. The other two configuration options are for uris that go into the payload, such as the uri for the commit and the uri for the project. The above examples are for Redmine.