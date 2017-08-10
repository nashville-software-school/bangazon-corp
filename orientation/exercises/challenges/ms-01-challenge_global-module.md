# Make your own global node module

A Node module doesn't just have to be a small part of a larger program. Developers often write [Bash scripts](http://ryanstutorials.net/bash-scripting-tutorial/bash-script.php) to do any number of repetitive tasks to speed up their productivity, or just to impress their friends and families. You can do the same thing with Node. Check out this [tutorial](https://codeforgeek.com/2015/09/command-line-application-nodejs/) for how to turn any helpful chunk of code into a tool that's at your beck and call.
  

There's no trick to it; it's just a simple trick. 

## Requirements
Create a module called `project` that automates the setup of a project directory and adds the basic folders and files you need to start a new project. Also include these tasks:  

1. intializea git repo  
2. create a `.gitignore` file   
3. create a `package.json` file  
4. create a `README.md` and add the string `# Project Template` to it  
5. run `git add README.md` and `git commit -m "Initital Commit"`  
 
If you think your new tool is something other devs might like to use, go ahead and publish it to the [npm registry](https://docs.npmjs.com/getting-started/publishing-npm-packages)!
