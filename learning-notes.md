# Javascript Promise Learning Notes 

Table of Contents
  * [How to Upload Exoplanet Explorer Project from Local Machine to Remote Repository](#how)
  * [Lesson 1: Creating Promises](#lesson-1)
  * [Lesson 1.8: Quiz: Write Your First Promise](#lesson-1.8)
  * [Lesson 1.11 Quiz: Wrap an XHR](#lesson-1.11)
  * [Lesson 1.13 Quiz: Fetch API](#lesson-1.13)

## [Udacity Javascript Promises Course](https://www.udacity.com/course/javascript-promises--ud898)<a id="how"></a>

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


### Lesson 1: Creating Promises<a id="lesson-1"></a>

#### Lesson 1.8 Quiz: Write Your First Promise<a id="lesson-1.8"></a>
> Print 'Resolve' after a certain milliseconds.
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

* The setTimeOut function return promise object after milliseconds. After that, the thenable finish is executed.

#### Lesson 1.8 Quiz: Wrapping readystate<a id="lesson-1.8"></a>
> Create a function to show "Resolved" text on the web page before the huge image finish loading.

```javascript
function ready() {
			/*
			Your code goes here!

			Instructions:
			(1) Set network throttling.
			(2) Wrap an event listener for readystatechange in a Promise.
			(3) If document.readyState is not 'loading', resolve().
			(4) Test by chaining wrapperResolved(). If all goes well, you should see "Resolved" on the page!

			Make sure you return the Promise!
			 */

			  return new Promise(function(resolve) { 
					document.addEventListener('readystatechange', function() {
						/*
						Your code here too!
						 */
						if (document.readyState !== 'loading') {
							console.log('inside listener:' + document.readyState);
							resolve();
						}
					});	
					if (document.readyState !== 'loading') {
							console.log(document.readyState);
							resolve();	
					};		
				});

		};

		/*
		Don't forget to test your code!
		Call wrapperResolved when the DOM is ready.
		 */
		ready().then(wrapperResolved);
		
		// Just here for your testing
		function wrapperResolved() {
			var completion = document.querySelector('.completion');
			completion.innerHTML = "Resolved!";
		};
```

* There are two same conditions for checking the document state, which are before and after creating the promise. This is necessary to catch the document state before the event listener even get the chance to 'listen' the event. 

* Since the two condition are the same, the code can be simplify by creating the **checkState() function** as the instructor's solution as below:
```javascript
return new Promise(function(resolve) {
    function checkState() {
      if (document.readyState !== 'loading') {
        console.log(document.readyState);
        resolve();
      }
    }
    document.addEventListener('readystatechange', checkState);
    checkState();
  });
```
#### Lesson 1.11 Quiz: Wrap an XHR<a id="lesson-1.11"></a>
> Wrap XMLHttpRequest in Promise. 

```javascript
/**
 * XHR wrapped in a promise.
 * @param  {String} url - The URL to fetch.
 * @return {Promise}    - A Promise that resolves when the XHR succeeds and fails otherwise.
 */
function get(url) {
  /*
  This code needs to get wrapped in a Promise!
   */
  return new Promise(function(resolve, reject) {
    var req = new XMLHttpRequest();
    req.open('GET', url);
    req.onload = function() {
      if (req.status === 200) {
        resolve(req.response);
      } else {
        reject(Error(req.statusText));
      };        
    }
    req.onerror = function() {
      // It failed :(
      // Pass a 'Network Error' to reject
      reject(Error('Network Error'));
    };
    req.send();      
  });
}
```
* Notice that the **onerror()** and **send()** method are not wrapped in Promise in order to keep sending the request even before the response is ready.
* Adding the .then and .catch when calling get function.
```javascript
get('../data/earth-like-results.json')
.then(function(response) {
  addSearchHeader(response);
})
.catch(function() {
  addSearchHeader('unknown');
  console.log('Unknown error');
});
```
#### Lesson 1.13 Quiz: Fetch API<a id="lesson-1.13"></a>
> Fetch return promise. The Promise returned from fetch() won’t reject on HTTP error status even if the response is an HTTP 404 or 500. Instead, it will resolve normally (with ok status set to false), and it will only reject on network failure or if anything prevented the request from completing.

```javascript
  * XHR wrapped in a Promise.
   * @param  {String} url - The URL to fetch.
   * @return {Promise}    - A Promise that resolves when the XHR succeeds and fails otherwise.
   */
  //MyNote: Then and catch callback can be ommitted if it just return the response/error. 
  function get(url) {
    return fetch(url)
    .then(response => response)
    .catch(err => err);
  }

  /**
   * Performs an XHR for a JSON and returns a parsed JSON response.
   * @param  {String} url - The JSON URL to fetch.
   * @return {Promise}    - A promise that passes the parsed JSON response.
   */
  function getJSON(url) {
    return get(url).then(function(response) {
      // Handle network errors
      if (!response.ok) {
        throw Error(response.statusText ? response.statusText : 'Unknown network error')
      }
      return response.json();
    });
  }

  window.addEventListener('WebComponentsReady', function() {
    home = document.querySelector('section[data-route="home"]');
    getJSON('../data/earth-like-results.json')
    .then(function(response) {
      addSearchHeader(response.query);
      console.log(response);
    })
    .catch(function(error) {
      addSearchHeader('unknown');
      console.log(error);
    });
  });
```
