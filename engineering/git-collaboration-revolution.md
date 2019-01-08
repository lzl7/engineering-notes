Git is an excellent source control system, not only greatly improve the source code versioning management's quality and experience, but also enable developers to do development in parallel. With the natural characteristic of distributed, it changes the the way people collaborate with each other.

The Git has a bunch of advantages, like tamper-resistant, distributed, fast branching, quick merging etc. However, the distributed is the real core weapon and the root reason why it is distinguished from other source control systems. The Git and its ecosystem is now revoluting the collaboration between developers via `Cloning`. Because of cloning, the source codes could be distrbuted to any places and fully controlled by the developers. Since the development does not rely on any *central/global control*.

**Fork** is an *advanced extension* of **Clone** which is more a concept, and it focus on resolving the different problems. To be clear, fork does not a 'real extension' of Git, it comes from the software development management system (like Github, Gitlab and VSTS etc.) and only exists in that context.

## Clone vs Fork
### What are they?
**Clone** the repository enables developer has the full copy of original repository, and do whatever they want in the local repository.

**Fork** is similar like clone, but managed by the software development system. What is more, when doing the forking the new repository in the software development system only has all the code/data from the original repository without branches and other manamgement related data (like pull request). 

### What are the differences?
**From feature perspective**

Fork repository is more like a hosted repository with full copy all elements from the original repository. The goal is to have a proxy host for the feature teams to do the code review, which is also to enable the parallel developlment of features by different small groups/teams. The whole development is totally isolated from others.

Clone, is a local copy of the repository where you actively do the development works for specific feature (even differnet features).

**From service/tooling perspective**

Fork repository is more like the central coordinated server while the clone repository acts as concrete task processing clients/nodes.

**From project management perspective**

Fork is useful to break down the big system/works into different components and assigned to different small teams. Clone a flexible way to let developer to take concret work items/tasks and complete it. Fork repository could focus on management and coordination for the group or team, while clone is more focusing on individual. Fork helps on coarse-grained or higher level's management, while clone improves developer's agility and development.

Fork is more team/group or feature oriented while clone is more like developler oriented. Within a clone repository, a developer could do whatever he/she wants. While a fork resository, a small team could do whatever it wants to execute.

## Evolutions by *Forking*
**From cooperation perspective**

Fork development model is more suitbale for open sources community. Different groups or individuals could easily do a fork to their own account and fully own that repository. They could do any change to the code base, and also they could have their own code review process or their own project management activities. 

The fully control brings the great flexibility for the collaboration. Individual contributor or group could take that task from the roadmap in the original project and do the implementation, even they could working on the same task independently. Then the project owner could pick the best implementation to merge, which will greatly improve the whole quality for the project. What is more, the talents might have some creative ideas (not even in the planned roadmap) and implement the fancy features, and finally they will be merged back to the original repository.

The distributed control enable to distribute the (creative) works around the whole world. The cooperations are no more tasks assignment based, and the work is not 'push' to developer but 'pull' by the developer, or even created by the developer. That is the biggest difference!

**From group/team perspective**
Without forking feature, the way for distribute development is only via cloning. Even in the local repository, the developer could do any change they want in local with arbitrary branches. From the developer's point of view, there is a significant aglity improvement. 

However, all the individuals need to be the contributor of the project and the owner of project might need to do code review for all pull requests, which is not friendly to the owner or maintainer. What is more, those pull request might be a minor/small changes (Usually, we suggest to have the pull request as small as possible to make the code review easier) and mostly they are the intermediate changes which could not reflect the overview for the feature/components and they does not looks that straight forward. Further more, the intermediate changes for different components/features make the core (i.e. original) branch not clean. They might have some bugs and the bugs might impact other features and increase the complexity and risk in the project/service cycle management (like bug fix, release etc.). Last but not least, it is also about the trust or social group. Usually, different people will have their own groups, the developers they trust, the individual they cooperated with, they could work together very well. They kind of automaticlaly be clustered into different groups, and act as a feature v-team.

The fork feature perfectly resolves all these issues. With cloning, the development around the repository is more like a graph with all nodes almost similar (in term of complexity) to others and might have large amount of nodes when the project is large. With the forking, from the original repository perspective, it will has less pull requests (branching node in the commit graph), most of the small works/pull requests for specific feature already gracefully handled by the forked repository. The original repository off-load the works to the forked repository, which could greatly reduce the complexity of work and the productivity is also very efficient.

## Further thinking
Given that we have the fork feature for the repository which enables great flexibility for the project development. There are basically two ways to do the project management: fork-based repositories and component-based repositories. Those are totally two different philosophies, each has its own advantages and application scenarios. We might have the quesion: ***when should we use which approach?***

> TODO: in the future, I will find the time to discuss the trade-off between them and some suggestions in the real project development.

**References**:
* [git clone vs fork](http://bryanpendleton.blogspot.com/2014/07/git-clone-vs-fork.html)
* [Stash 2.4: Forking in the Enterprise](https://www.atlassian.com/blog/archives/stash-git-forking-development-workflow)
* [Collaborative Github Workflow](http://www.eqqon.com/index.php/Collaborative_Github_Workflow)