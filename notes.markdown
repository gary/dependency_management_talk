# git builtin facilities #
## subtree merge strategy ##
   * setup involves a number of different commands/options: annoying
   * cloning a repo with subtrees requires re-registration of remote branches
   * no opting out of registered dependencies--one nicety of submodules
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


