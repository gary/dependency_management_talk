# git builtin facilities #
## subtree merge strategy ##
   * setup involves a number of different commands/options: annoying
   * cloning a repo with subtrees requires re-registration of remote branches
   * no opting out of registered dependencies--one nicety of submodules
   * praised for 'less, long-term administrative burden': given the simplicity the end result, i can believe it.
   * compatible with all versions of git (submodules were introduced in 1.5.3)
   * not well-advertised... why didn't i know about this feature before?
## submodules ##
   * initizialize submodule facility, register dependency, update, notice extra step
   * dependencies pulled in at a specific commit (vs. a branch head), detached from tree: very annoying
   * updating a project's submodules auto cleans the current branch: annoying
   * 
   * chaos can also ensue with normal workflows: http://rubyurl.com/Umhn (and even with a lone developer)


# resources #
  * [How to use the subtree merge strategy](http://www.kernel.org/pub/software/scm/git/docs/howto/using-merge-subtree.html)
  * [Git User's Manual - Submodule Chapter](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#submodules)
  * [Easy Git Dependency Management wiht Giternal](http://www.rubyinside.com/giternal-easy-git-external-dependency-management-1322.html#comment-37316)
  
