根权限，不要改动。

但是我们必须了解访问控制，以便其子权限仓库可以通过修改进行覆盖
[project]
	description = Access inherited by all other projects.
[receive]
	requireContributorAgreement = false
	requireSignedOffBy = false
	requireChangeId = true
	enableSignedPush = false
[submit]
	mergeContent = true
[access "refs/*"]
	read = group Administrators
	read = group Anonymous Users
	revert = group Registered Users
[access "refs/for/*"]
	addPatchSet = group Registered Users
[access "refs/for/refs/*"]
	push = group Registered Users
	pushMerge = group Registered Users
[access "refs/heads/*"]
	create = group Administrators
	create = group Project Owners
	editTopicName = +force group Administrators
	editTopicName = +force group Project Owners
	forgeAuthor = group Registered Users
	forgeCommitter = group Administrators
	forgeCommitter = group Project Owners
	label-Code-Review = -2..+2 group Administrators
	label-Code-Review = -2..+2 group Project Owners
	label-Code-Review = -1..+1 group Registered Users
	push = group Administrators
	push = group Project Owners
	submit = group Administrators
	submit = group Project Owners
[access "refs/meta/config"]
	exclusiveGroupPermissions = read
	create = group Administrators
	create = group Project Owners
	label-Code-Review = -2..+2 group Administrators
	label-Code-Review = -2..+2 group Project Owners
	push = group Administrators
	push = group Project Owners
	read = group Administrators
	read = group Project Owners
	submit = group Administrators
	submit = group Project Owners
[access "refs/tags/*"]
	create = group Administrators
	create = group Project Owners
	createSignedTag = group Administrators
	createSignedTag = group Project Owners
	createTag = group Administrators
	createTag = group Project Owners
[label "Code-Review"]
	function = MaxWithBlock
	defaultValue = 0
	copyMinScore = true
	copyAllScoresOnTrivialRebase = true
	value = -2 This shall not be merged
	value = -1 I would prefer this is not merged as is
	value = 0 No score
	value = +1 Looks good to me, but someone else must approve
	value = +2 Looks good to me, approved
[capability]
	administrateServer = group Administrators
	priority = batch group Non-Interactive Users
	streamEvents = group Non-Interactive Users


3 All-Users


4 all-myprojects inherited from All-Projects

这里有一个访问控制的参考配置。
[access]
	inheritFrom = All-Projects
[submit]
	action = inherit
[project]
	description = This is a top configuration repository for all xvisio repository
[access "refs/for/*"]
	abandon = group reviewers
	abandon = group scm
	addPatchSet = group scm
	label-Code-Review = -2..+2 group reviewers
	push = group developers
	push = group reviewers
	push = group scm
	pushMerge = group developers
	pushMerge = group reviewers
	pushMerge = group scm
	read = group developers
	read = group reviewers
	read = group scm
	rebase = group reviewers
	rebase = group scm
	removeReviewer = group developers
	removeReviewer = group reviewers
	removeReviewer = group scm
	revert = group reviewers
	revert = group scm
	submit = group developers
	submit = group reviewers
	submit = group scm
[access "refs/tag/*"]
	forgeCommitter = group reviewers
	forgeCommitter = group scm
	create = group scm
[access "refs/head/master"]
	addPatchSet = group reviewers
	addPatchSet = group scm
	create = group reviewers
	create = group scm
	createSignedTag = group reviewers
	createSignedTag = group scm
	createTag = group scm
	delete = group scm
	forgeCommitter = group scm
	label-Code-Review = -2..+2 group scm
	push = group scm
	pushMerge = group scm
	read = group developers
	read = group reviewers
	read = group scm
	rebase = group reviewers
	rebase = group scm
	revert = group scm
[access "refs/head/dev"]
	abandon = group developers
	addPatchSet = group developers
	create = group developers
	createSignedTag = group scm
	createTag = group scm
	delete = group scm
	forgeCommitter = group scm
	label-Code-Review = -2..+2 group reviewers
	label-Code-Review = -2..+2 group scm
	label-Code-Review = -1..+1 group developers
	push = group developers
	pushMerge = group developers
	read = group developers
	rebase = group developers
	removeReviewer = group developers
	revert = group developers
	submit = group developers



## 修改

git clone ssh://admin-xv@192.168.20.131:29418/all-xprojects && scp -p -P 29418 admin-xv@192.168.20.131:hooks/commit-msg all-xprojects/.git/hooks/

#refs/meta/config 是All-Projects的引用
git fetch origin refs/meta/config:refs/remotes/origin/meta/config
git checkout meta/config
#现在目录下有groups project.config两个文件
#提交修改
git add . && git commit -m “modify config”
git push origin meta/config:meta/config