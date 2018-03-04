# Javascript Promise Learning Notes 

## [Udacity Javascript Promises Course](https://www.udacity.com/course/javascript-promises--ud898)

### How to Upload Exoplanet Explorer Project from Local Machine to Remote Repository
1. Create a new repository on GitHub 
To avoid errors, do not initialize the new repository with README, license, or `gitignore` files. You can add these files after your project has been pushed to GitHub.
2. Open Terminal
3. Change the current working directory to your local project.
4. Initialize the local directory as a Git repository. 
```sh
$ git init
```
5. Add the files in your new local repository. This stages them for the first commit.
```sh
$ git add .
# Adds the files in the local repository and stages them for commit. 
# To unstage a file, use 'git reset HEAD YOUR-FILE'.
```
6. Commit the files that you've staged in your local repository
```sh
$ git commit -m "First commit"
# Commits the tracked changes and prepares them to be pushed to a remote repository. 
# To remove this commit and modify the file, use 'git reset --soft HEAD~1' and commit and add the file again. 
```
7. At the top of your GitHub repository's Quick Setup page, click to copy the remote repository URL.
8. In Terminal, add the URL for the remote repository where the local repository will be pushed. But before that, remove the existing origin and add a new one.
```sh
$ git remote remove the origin
# Remove the existing origin
$ git remote add origin https://username@github.com/username/js-learning-promises.git
# Add new origin
```
9. [Push the changes](https://help.github.com/articles/pushing-to-a-remote/) in your local repository to GitHub.
```sh
$ git push origin master
# Push local repository
Password for https://username@github.com/username/js-learning-promises.git:
# Fill in the password
```
> If there is an error as below:
> error: src refspec master does not match any.
> error: failed to push some refs to 'https://username@github.com/username/js-learning-promises.git'
```sh
$ git show-ref
$ git push origin HEAD:master
```


### Lesson 1: Creating Promises
#### Lesson 1.8 Quiz: Write Your First Promise
```javascript
function wait(ms) {
  /*
  Your code goes here!

  Instructions:
  (1) Wrap this setTimeout in a Promise. resolve() in setTimeout's callback.
  (2) console.log(this) inside the Promise and observe the results.
  (3) Make sure wait returns the Promise too!
   */			
         return new Promise(function(resolve) {
            console.log(this);
            window.setTimeout(function() {                                
               resolve();
            }, ms);                
         });
     }

/*
Uncomment these lines when you want to test!
You'll know you've done it right when the message on the page changes.
 */
 var milliseconds = 2000;
 wait(milliseconds).then(finish);


// This is just here to help you test.
function finish() {
  var completion = document.querySelector('.completion');
  completion.innerHTML = "Complete after " + milliseconds + "ms.";
};
```

The setTimeOut function produce promise object after milliseconds. After that, the thenable finish is executed.

#### Lesson 1.8 Quiz: Wrapping readystate
