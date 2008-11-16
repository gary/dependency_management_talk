# git builtin facilities #
## subtree merge strategy ##
   * setup involves a number of different commands/options: annoying
   * cloning a repo with subtrees requires re-registration of remote branches
   * no opting out of registered dependencies--one nicety of submodules
   * dependencies are included in superproject: bloat fail
   * praised for 'less, long-term administrative burden': given the simplicity the end result, i can believe it.
   * compatible with all versions of git (submodules were introduced in 1.5.3)
   * not well-advertised... why didn't i know about this feature before?

## submodules ##
   * initizialize submodule facility, {add,update}, notice extra step
   * dependencies pulled in at a specific commit (vs. a branch head), detached from tree: very annoying
   * updating a project's submodules auto cleans the current branch: annoying
   * removing registered submodules is a real pain in the ass, and can have mixed results
   * changes to the submodule *must* be published before changes to the project that references it.  if you forget, the repo will no longer be cloneable... fail.
   * chaos can also ensue with normal workflows: http://rubyurl.com/Umhn (and even with a lone developer)
   * dependencies can be individually imported into a {cloned,forked} project after registration
   * capistrano comes with builtin, hassle-free support, but comes at the cost of having your deployment rely on (more) external servers

# tools that manage the git complexity #
## Tim Dysinger's subtree merge sake tasks ##
   * pastie didn't work for me, so i created a new one [here](http://pastie.org/315840.txt)
   * simple to use, simple to setup
   * outdated and in need of updating for git 1.6.x

## Pat Maddox's giternal ##
   * follows the {rails,merb} configuration convention (i.e., YAML in config/)
   * did not work as advertised (giternal update) out of the box--err, installing from rubygems
   * hand installing the master did, however
   * a great fit for dependencies that you have commit access to
   * push/pull changes, freeze for deploy, unfreeze for updates
   * can also be used for the simpler, track-and-update use case as well
   * read [this thread](http://rubyurl.com/Umhn) for clarification of use cases, differences from braid
   * uses repository cloning and pulling in conjunction with ignore entries
   * that said, lacks the ability to import a dep at a stable version (master branch only)
   * deploying with a known-stable version of a dependency is manual, but not a non-issue: manually change to desired branch, freeze, deploy

## Cristi Balan's braid ##
   * direct competition to François Beausoleil's piston (remember that one?)
   * supports both git and svn
   * allows for complete control (add, update, delete) of dependencies, with more features planned for 0.6
   * appears to be a wrapper for git's subtree merge strategy: yes
   * straightforward and intuitive to use
   * handy rails plugin shorthand: braid add <repo_url> -p
   * on update, halts for dirty branches: good
   * deletion of deps doesn't clean up their remote branches, yet?
   * no support for sending changes upstream to dependencies, a la giternal
   * dependency status has not been implemented yet

## François Beausoleil's piston ##
   * made maintaining forked subversion deps a cinch before git took over... at the cost of speed
   * scm-agnostic rewrite **still** in the works... been a while (project initially debuted in 2006)
   * still very rough around the edges: status call blows up
   * minor gripe: some command names remain stuck in the past.  e.g., {un,}locking vs. {un,}freezing
   * most of documentation is still geared for the 1.4.x (svn) generation: fail

## Miles Georgi's externals ##
   * yet another scm-agnostic approach like piston and braid
   * royal pain: --help all but requires piping to a pager
   * takes agnostic to the project-level, although other project types haven't been implemented yet
   * installation shorthand driven by project type (e.g., ext install <url> would install into vendor/plugins for a rails project)
   * shows status for all dependencies or a specific one if specified
   * deletion doesn't clean out .gitignore or .externals, yet?
   * built with deployment in mind: export functionality
   * clever, unusual options such as a touch_emptydirs (for forcing git to track an empty dir)
   * hasn't seen any activity for a while

# symlinking #
## 37Signals' Cached Externals ##
   * operates under the assumption that your dependencies won't change often... so why copy them with each deployment?
   * trivial to install: `script/plugin install git://github.com/37signals/cached_externals.git`
   * after that, dependencies are configured in `config/externals.yml`
   * path, repository URL and revision are required (shorthand for revisions is supported, but can slow down the process)
   * setup via capistrano, established locally via `cap local externals:setup`
   * updating is trivial: updated the desired revision and execute the setup recipe again
   * ignore rules must be set manually

# server-side (the other half) #
## copying ##
   * all of the approaches support some form of {copy,export}ing their managed dependencies:
     * git submodules: pulled from various remote repositories (`set :git_enable_submodules, 1` in capistrano; initialization, update `:web_command` in vlad)
     * git subtree merge strategy: dependencies embedded in project
     * giternal, braid, and piston: freeze dependencies, call to `update` in deployment script
     * ext: `ext --workdir=#{current_release} ex`

## 37 Signals' Fast Remote Cache ##
   * custom Capistrano deployment option that hard links the project's assets instead of copying them
   * intended for larger projects where copy time noticeably slows deployment
   * not a standalone solution: use in conjunction with any of the above options

