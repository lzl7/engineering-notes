There are basically two ways to manage the projects:

    1. One repo + forking
    2. Multiple repos

Please note that we will not discuss the detail of concrete feature development via cloning. Here we will only focus on the project managment.

## 1. One Repo + Forking
In this approach, it will only have one repository to hold all the source code for the projects.

***How to manage the source code?***

The owner will hold the original repository in his/her account, and control the write/merge permission. 

The contributor will contribute/implement the features via forking or cloning. If the developers does not in the contributor list (say they is in another org), then they would fork the original repo into their own account where they has fully control. After folking, they could do the development acts as they really own the whole code base. They could even do some new creative stuff in their forked code base (See [Git - The revolution of collaboration](git-collaboration-revolution.md)) and create differnt products/services.

Via forking, different teams/orgs could contribute their works and in the end push back to the original repository. With this way, it could fully distribute the works to any one or any team or org without sacrificing the control. What is more, it also significantly reduces the permission or tasks management effort.

In terms of the organization for sub-projects, in this method it usually uses the folders to seperate the projects. In practice, it does work well in most of the case.

***When should use this approach?***

This method is extremely useful in the open source projects and works very well in the community.

If the code base is huge, this method might not be a good solution. Because it has all stuff together which makes the code base very large and also increases the complexity of the code managemnet.

There are several characteristics:
* The code could be open access to others/or specific teams (i.e. not a confidential project)
* Code base is not gigantean
* The source code would be logically high cohesion

There might have a question: *How big is too big?* It depends, depends on your org/ your team size. There is no deterministic guide there. If the management work is overhead, then there is a sign of too big.

## 2. Multiple Repositories
In a system, it might contain a bunch of projects or sub-systems and the code base will be tremendous. Take search engine as an example, it contains the crawer, indexing, serving, ranking etc. Each one is a huge system. Besides, there are other underlying infrastructures like big data storage system, distirbuted processing system etc. There is no way to put them together into one repo. In this case, different projects/sub-system will have their own repos.

***How to manage source code?***
Each system/project (please note it might contains sub-projects/sub-systems) will have its own repo. And then different projects/services will be developed, deployed and managed independently. They will orchestrate together to provide the final service, like search engine.

The system will have several ways to cooperate with each others:
* Service directly APIs calls
* Message/event based trigger

The service APIs call method would be instant and low latency. But it will be more cost and services  will be coupled. What is more, it will be more complicated and have lower reliablity and availability.

The message/event method could dramtically decouple different systems, and make the whole system more flexible. It usually have better resource utilization. However, it might have larger latency.

***When should use this approach?***

Obviously, when the project is too big, we definitely need to separate it into several repos. And different teams would just focus on each repo.

Microservices is also an scenario fit in this method. Different micro-service will be located into different repos and developed by different small teams.

## Which method should be choosed?

As mentioned above, each approach has its own scenarios. Actually, the way we manage the code base is mostly reflecting the team shape/management. If the team culture is more like start-up, more agile, then the multiple repositories would be a good fit.

In practice, we usually use both methods. With multiple repos method, we have a cleaner organization of the code/projects, and have better agility. And the forking approach enables the flexibility of contibution. As you can see, the large open source project also use this way, like docker, it has bunch of project repos for different tools/services.