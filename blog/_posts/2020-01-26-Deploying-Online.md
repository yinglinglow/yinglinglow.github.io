---
layout: post
title: "Deploying Online"
date: 2020-01-26
---

### __Setting up your Git (in Python)__
A step-by-step guide to setting up Git repositories, and deploying to Heroku, using Python and Visual Studio Code! It was really painful for me the first few times I did it (with the help of my expert friend, no less), so I really hope the below will help you guys simplify it.

For clarification on the below instructions, when I say “Input `xyzzy`“ I mean type `xyz` into the terminal and hit return/enter. 

__In Visual Studio Code:__
1. Open the terminal. Drag up the bottom console if you can't see it, alternatively click 'View', click 'Integrated Terminal'.
2. Input `cd` - this brings you back to your Home folder
3. Input `ls` - this shows you all the files in the current directory you are in
4. Input `mkdir filename` - this creates a folder with the filename, which is going to be where all your repository files will be in. This filename should be similar to the name of your directory.
5. Input `cd filename` - replace filename with the name of your folder. This brings you into the folder

__In Github:__
1. Signup for a free account/ login
2. Click ‘New repository’
3. Fill in the repository name, select ‘add README.md’
4. Click ‘Create repository’

__In Visual Studio Code:__
1. Input `git init` - initialises a git repository locally in your computer
2. Input `git add README.md` - adds the readme file into the repository index. _*If you didn’t select add README previously, it will return an error. You will need to create a README file manually by inputing `touch README.md`, and then repeat step 2._
3. Input `git commit -m “first commit”` - basically this commits (aka mark as confirmed) your action of adding the readme file. The text in between the quotes “” is your comment tagged to this action.
4. Input `git remote add origin https://github.com/username/reponame.git` - this tells git which address to push it to. Replace username with your username and reponame with the name of your repository. Alternatively, earlier on when you created your repository in Github this link should have been provided to you. _*If you get an error (or you did a typo like me) and trying to redo the step gives you `fatal: remote origin already exists.`, you can remove the origin and put in a new one again, by inputing `git remote rm origin`. Repeat step 4 after that._
5. Input `git push -u origin master` - this pushes your commit online, to Github. 
6. If you have been successful, you can refresh your Github page online and you should be able to see your README.md file in the repository.
7. Tell Visual Studio Code which language you are using to code in - click ‘View’, ‘Command Palette’, ‘Select Workspace Interpreter’, ‘Python 3.6’


At this stage you have successfully set up your Github repository! You can now work locally on your computer, and push the code online to Github.

Next, we will host your work on the internet (via Heroku) so everyone can see it!


### __Setting up heroku (in Python)__

1. Signup for a free account/login
2. Click 'Create' on top right hand corner, and then 'Create new app'
3. Input app name; I kept the region as US. Click 'Create app’
4. Deployment method - click ’Connect to github’
5. Input repository name, press search. Click ‘Connect’.

   
__In Visual Studio Code:__
1. Input `pip3 install gunicorn` to install gunicorn.
2. Create files required for Heroku deployment. Input `touch requirements.txt` - this is a file that is required for all Python apps. It details all the versions of libraries used for the app, which Heroku will load so that the app can run properly on the internet.
3. Input `pip3 freeze` and search for the libraries you need (i.e. the stuff you import in your app), as well as gunicorn and Flask. For example, my app needs to import pandas and numpy to run. I will copy and paste them into requirements.txt by inputing the below:

```bash
echo “numpy==1.13.1
pandas==0.20.3
gunicorn==19.7.1
Flask==0.12.2
Flask-Compress==1.4.0”>>requirements.txt
```
What this echo command is doing is basically telling the program to take all the text in between the quotation marks, and put it into the file, requirements.txt.
Aka the lazy way to paste something into a file without opening the file.

4. Input `touch runtime.txt` - this is a file only required by Heroku. It tells Heroku which version of program to use. Input `python -V` _(that’s a capital V! I typo-ed and had to use exit())_ to see what version of Python you are on. Input `echo “python-3.6.2”>>runtime.txt` to write it in the runtime.txt file. _*If you are using Python 3 but the version on the computer shows up as 2, don’t worry - just type in the Python 3 version instead._

5. Input `touch app.py` to create your app file. Input the below, which is a very basic python program to instruct the server to go to an index.html file with Flask. You can just copy and paste the text within the quotation marks into the app.py file as well (I'm just lazy to open the file).

```bash
echo “from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def main():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)”>>app.py

```

Alternatively, if you already have your app written, just copy and paste your py file into the folder.

6. Input `mkdir templates` - this creates a folder called templates.
7. Input `cd templates` to go into the templates folder. Input `touch index.html` - this creates an index.html file. Input the below:

```bash
echo “<!DOCTYPE html>
<html>
<head>
    <title>My Homepage</title>
</head>
<body>
What’s up world!!!
</body>
</html>”>>index.html

```

Alternatively copy and paste all the text within quotation marks directly into index.html.

8. Input `cd ..` - this goes up one folder in the directory, bringing you back to your main folder.

9. Input `touch Procfile` (that’s a capital P!) which creates a Procfile. Input `echo “web: gunicorn app:app”>>Procfile`. Basically this file tells Heroku that this is a web app, using gunicorn (which reads Python), and the last bit is equivalent to `from app import app`. Meaning, from app.py, import the variable app (defined as app = Flask(__name__). If you saved your app.py in another folder, you can replace the app.py bit with your folder location, like bot/app.py

10. Your app should be able to run locally now. Input `python3 app.py` to try running it. It should give you the link, press command+click to open the link in a new page.

11. If everything works, go to the left side panel, there should be a blue bubble on a node icon detailing the number of changes you have made. Next to ‘Changes’, click the ‘+’ sign to stage all changes. Click the tick on top, input the message ‘added files for heroku deployment’. Press the three dots, click ‘Push to…’, click ‘origin’

__In Heroku:__
1. Press ‘Deploy Branch’ under Manual deploy. This is only required for the first time. *If you have troubles building it or running the application, click ‘More’ on the top right hand side, next to ‘Open app’, and click ‘View logs’. From there you can try to troubleshoot a little.

2. Once your app is successfully built, at the bottom it should provide the link to your website, saying deployed to Heroku. Go to that link to access your website! Alternatively, scroll up and click open app.
3. Once done, click ‘Enable automatic deploys’. This automatically updates your app every time you push to Github.

---

__CONGRATULATIONS!!!__ You are officially DONE. With the deployment haha - the real work is yet to come ;)

---

_*Optional: Create a .gitignore file_ (this tells git which files to ignore so that they are not uploaded to Github)
1. Go to https://www.gitignore.io/
2. Search what programs you use to code with - for me, it’s ‘Python’, ‘MacOS’, ‘VisualStudioCode’ - so they can customise the code for you.
3. Copy all the text generated
In Visual Studio Code:
4. Input `touch .gitignore` to create the file
5. You can open the .gitignore file manually to paste it in. To do this, click ‘File’, ‘Open’, and open the entire folder. Do not open just the individual file. You want to the see the entire folder content on the left of the screen. Double-click on .gitignore, and paste all the text in, and save it. Alternatively, you can input `echo “text”>>.gitignore` - replace text with the text you copied. This writes all the text into the file.
7. If there is a particular file in the directory you want to ignore (for e.g. testing.txt), type the filename in right at the very top ’testing.txt’. You should see that even if you make changes to this file, VSC will not ask you to commit changes on it.