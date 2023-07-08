---
title: Using VScode for Arduino sketches
date: 2023-05-28 12:00:00 -0200
categories: [devops]
tags: [ardiuno, vscode]
---
## Using VScode for Arduino sketches

My steps to sdtart using VScode for ardiuno sketches, after I reinstalled my PC. 

My PC at home (for personal use, not my work laptop) is running ubuntu linux and now with a clean install of 22.04.2 LTS. I started with an install of vscode from the repository (which is a snap package) and added some of my regular used extentions.

Because I did not wanted to invent the wheel again, I found [instructions](https://www.circuitstate.com/tutorials/how-to-use-vs-code-for-creating-and-uploading-arduino-sketches/ "it is not that old"), some of which I already did, and some new pointers.

but first, make sure git is operastional
``` sh
sudo apt install git
git config --global user.name "Roelof"
git config --global user.email "roelof@roheve.nl
```

Then install vscode. 
``` sh
sudo snap install code
```
After it is installed start it and add the arduino extention (which brings the arduino-cli).
As I want to use it for the [LilyGO T-display-S3](https://github.com/Xinyuan-LilyGO/T-Display-S3) board. I added the board URL for that. The libraries for Expresiff where downloaded, but they where not recognized, so needed to be added.

To use the arduino-cli that is integrated in the vscode extention more easy (for debugging my configuration), I added the following alias (I did not want to fiddle with the path). I used locate to find where it was hidden (somewhere in the .vscode folder). Note that it has a hardcoded version, that will likely change.
``` sh
alias arduino-cli="${HOME}/.vscode/extensions/vsciot-vscode.vscode-arduino-0.6.0-linux-x64/assets/platform/linux-x64/arduino-cli/arduino-cli.app"
```

Now commands like 'arduino-cli config dump' will work as in the documentation, to explore more functionality. I did not want a separate install, as that would make troubleshooting worse

to ve continued...

...