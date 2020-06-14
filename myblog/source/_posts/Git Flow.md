# Git Flow工作流总结

## Git Flow 简介

​		在使用Git的过程中如果没有清晰流程和规划，否则,每个人都提交一堆杂乱无章的commit,项目很快就会变得难以协调和维护。Git版本管理同样需要一个清晰的流程和规范。Git Flow定义了一个项目发布的分支模型，为管理具有预定发布周期的大型项目提供了一个健壮的框架。

## Git Flow 的常用分支

### Master主分支：

​		线上分支（线上跑的代码），我们经常使用的Master分支，这个分支最近发布到生产环境的代码，最近发布的Release， 这个分支只能从其他分支合并，不能在这个分支直接修改。

### Develop 分支：

​		这个分支是我们是我们的主开发分支，包含所有要发布到下一个Release的代码，这个主要合并与其他分支，比如Feature分支。

### Feature 分支

​		功能分支，从develop分支拉取，开发完成以后合并到develop分支，这个分支主要是用来开发一个新的功能，一旦开发完成，我们合并回Develop分支进入下一个Release

### Release分支

​		预上线分支上线前的测试，当你需要一个发布一个新Release的时候，我们基于Develop分支创建一个Release分支，完成Release后，我们合并到Master和Develop分支

### Hotfix分支

​		热补丁分支（负责线上问题紧急修复），当我们在Production发现新的Bug时候，我们需要创建一个Hotfix, 完成Hotfix后，我们合并回Master和Develop分支，所以Hotfix的改动会进入下一个Release

## Git Flow 如何使用

### Master/Devlop 分支

​		所有在Master分支上的Commit应该打上Tag，一般情况下Master不存在Commit，Devlop分支基于Master分支创建。

### Feature 分支

​		Feature分支做完后，必须合并回Develop分支, 合并完分支后一般会删点这个Feature分支，毕竟保留下来意义也不大。

### Release 分支

​		Release分支基于Develop分支创建，打完Release分支之后，我们可以在这个Release分支上测试，修改Bug等。同时，其它开发人员可以基于Develop分支新建Feature (记住：一旦打了Release分支之后不要从Develop分支上合并新的改动到Release分支)发布Release分支时，合并Release到Master和Develop， 同时在Master分支上打个Tag记住Release版本号，然后可以删除Release分支了。

### Hotfix 分支

​		hotfix分支基于Master分支创建，开发完后需要合并回Master和Develop分支，同时在Master上打一个tag。

## GitHub flow

[		GitHub flow](http://scottchacon.com/2011/08/31/github-flow.html) 是 GitHub 所使用的一种简单的流程。该流程只使用两类分支，并依托于 GitHub 的 pull request 功能。在 GitHub flow 中，master 分支中包含稳定的代码。该分支已经或即将被部署到生产环境。master 分支的作用是提供一个稳定可靠的代码基础。任何开发人员都不允许把未测试或未审查的代码直接提交到 master 分支。 对代码的任何修改，包括 bug 修复、hotfix、新功能开发等都在单独的分支中进行。不管是一行代码的小改动，还是需要几个星期开发的新功能，都采用同样的方式来管理。当需要进行修改时，从 master 分支创建一个新的分支。新分支的名称应该简单清晰地描述该分支的作用。所有相关的代码修改都在新分支中进行。开发人员可以自由地提交代码和 push 到远程仓库。 当新分支中的代码全部完成之后，通过 GitHub 提交一个新的 pull request。团队中的其他人员会对代码进行审查，提出相关的修改意见。由持续集成服务器（如 Jenkins）对新分支进行自动化测试。当代码通过自动化测试和代码审查之后，该分支的代码被合并到 master 分支。再从 master 分支部署到生产环境。

​		GitHub flow 的好处在于非常简单实用。开发人员需要注意的事项非常少，很容易形成习惯。当需要进行任何修改时，总是从 master 分支创建新分支。完成之后通过 pull request 和相关的代码审查来合并回 master 分支。GitHub flow 要求项目有完善的自动化测试、持续集成和部署等相关的基础设施。每个新分支都需要测试和部署，如果这些不能自动化进行，会增加开发人员的工作量，导致无法有效地实施该流程。这种分支实践也要求团队有代码审查的相应流程。

## git-flow

​		应该是目前流传最广的 Git 分支管理实践。git-flow 围绕的核心概念是版本发布（release）。因此 git-flow 适用于有较长版本发布周期的项目。虽然目前推崇的做法是持续集成和随时发布。有的项目甚至可以一天发布很多次。随时发布对于 SaaS 服务类的项目来说是很适合的。不过仍然有很大数量的项目的发布周期是几个星期甚至几个月。较长的发布周期可能是由于非技术相关的因素造的。 		

​		git-flow 流程中包含 5 类分支，分别是 master、develop、新功能分支（feature）、发布分支（release）和 hotfix。这些分支的作用和生命周期各不相同。master 分支中包含的是可以部署到生产环境中的代码，这一点和 GitHub flow 是相同的。develop 分支中包含的是下个版本需要发布的内容。从某种意义上来说，develop 是一个进行代码集成的分支。当 develop 分支集成了足够的新功能和 bug 修复代码之后，通过一个发布流程来完成新版本的发布。发布完成之后，develop 分支的代码会被合并到 master 分支中。 

​		其余三类分支的描述如下表所示。这三类分支只在需要时从 develop 或 master 分支创建。在完成之后合并到 develop 或 master 分支。合并完成之后该分支被删除。这几类分支的名称应该遵循一定的命名规范，以方便开发人员识别。

| 分支类型 | 命名规范 | 创建自 | 合并到 | 说明 | | :------: | :-------: | :-----: | :---------------: | :-----------------------------: | | feature | feature/* | develop | develop | 新功能 | | release | release/* | develop | develop 和 master | 一次新版本的发布 | | hotfix | hotfix/* | master | develop 和 master | 生产环境中发现的紧急 bug 的修复 | 		对于开发过程中的不同任务，需要在对应的分支上进行工作并正确地进行合并。每个任务开始前需要按照指定的步骤完成分支的创建。例如当需要开发一个新的功能时，基本的流程如下：

​		从 develop 分支创建一个新的 feature 分支，如 feature/my-awesome-feature。

​		在该 feature 分支上进行开发，提交代码，push 到远端仓库。

​		当代码完成之后，合并到 develop 分支并删除当前 feature 分支。 

​		在进行版本发布和 hotfix 时也有类似的流程。当需要发布新版本时，采用的是如下的流程：

​		从 develop 分支创建一个新的 release 分支，如 release/1.4。

​		把 release 分支部署到持续集成服务器上进行测试。测试包括自动化集成测试和手动的用户接受测试。

​		对于测试中发现的问题，直接在 release 分支上提交修改。完成修改之后再次部署和测试。

​		当 release 分支中的代码通过测试之后，把 release 分支合并到 develop 和 master 分支，并在 master 分支上添加相应的 tag。

## Git-flow详解：

1.创建develop分支。

2.创建feature分支，并进行开发。

3.feature分支开发完成以后进行合并到develop分支。

4.创建release版本，并进行测试。

5.release版本测试通过以后进行合并到develop分支与master分支。