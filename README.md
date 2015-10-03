## CSC591 - DevOps project Milestone 1
### Team and Contribution
* Chetan Pawar - Triggered Builds script, Multiple Branches Multiple Jobs script, Video recording
* Payal Soman - Maven and Jenkins set up, Post-Build success task, Documentantion
* Viralkumar Sanghavi - Maven and Jenkins set up, Post-Build failure task , Documentation

### Build

##### Task 1 - Trigger build in response to git commit
* Install Git Plugin in jenkins
* Create build a new job on jenkins. Take care of following parameter

```
	Repository URL:  Enter local path to git repository
	Branch to build: Enter the branch name to which you want to trigger the build
```
* Create hooks post-commit file with following content
```
	#!/bin/sh
	curl http://localhost:8080/job/MavenTestAppLocal-master/build
```
* Change one of the local file and commit it
* Go to Jenkins project where it will show new build has been triggered automatically.

##### Task 2- Ability to execute a build job via a build manager 
* We have used maven which takes care of dependency management and running clean build every time. Maven manages dependency by adding packages into pom.xml file in dependencies section. It is also capable performing build and creating package from the source code.

##### Task 3: Build Status + External Post-Build Job Triggers
* The build status is displayed in the Jenkins console output.
* On an unsuccessful and unstable build, we have set up an email trigger using the Email notification option in Jenkins.
* On a successful build, we executed a script that writes the Build Number, Build Time and the Build Url into a temporary build log file. We used the Post-Build Script Plugin in Jenkins for this. Following are the contents of post-build script.

```
	BUILDTEXT1="Build number "$BUILD_NUMBER" successful for dev branch.\n"
	BUILDTEXT2="Time: $(date)\nFind details of build at "
	echo "$BUILDTEXT1$BUILDTEXT2$BUILD_URL\n-----------------------------------\n" >> /tmp/buildLog
```

##### Task 4: Multiple Branches, Multiple Jobs

* We are executing each job corresponding to each branch using the git post-commit hook.
* In the post-commit hook, we wrote a script to get the name of the branch committed and then trigger the build for that particular branch.
* The script for this in post-commit file is as follows:
```
	#!/bin/sh
	BRANCHNAME=$(git rev-parse --abbrev-ref HEAD)
	MASTERBRANCH="master"

	if [ $BRANCHNAME = $MASTERBRANCH ]; then
		curl http://localhost:8080/job/MavenTestAppLocal-master/build
	else
	        curl http://localhost:8080/job/MavenTestAppLocal-devBranch/build
	fi
	
	echo "Build triggered successfully on branch: $BRANCHNAME"

```
##### Task 5: Build history

* Jenkins maintains the history of all the builds executed for a job and shows it on its UI.
* The build history can also be obtained from the available XML or JSON API in Jenkins which contains the urls for all the builds that have been executed for that particular job.


### Screencast
This [screencast](https://youtu.be/piNrxWM9XH8) demonstrates how we achieved above 5 tasks.



