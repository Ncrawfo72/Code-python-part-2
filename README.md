# Codepython part 2
The mastery of code python in a series of five different projects
This repo is a template VS code project for CircuitPython projects that automatically uploads your code to the board when you press F5. Requires F5Anything extension.
## Use
### Description


### To commit from VS Code:
1. Go to the little branch icon in the left bar of VS Code.
2. Click the + icon next  to the files you want to commit.
3. Write a message that descibes your changes in the "Message" box and hit commit.
4. If you get an error about user.name and user.email, see the next section.
5. Click the "Sync changes" button.
### If you get an error about user.name and user.email
1. Open Git Bash from the Windows Search Bar.
2. FIlling in your actual information, run the following commands one line at a time. The paste shortcut is `Shift+Insert` or you can right click then hit paste. Spelling must match exactly:
```
git config --global user.name YOURUSERNAME
git config --global user.email YOURSCHOOLEMAIL
```
3. Return to step 3 of the previous section.
