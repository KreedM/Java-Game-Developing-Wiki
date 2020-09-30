Saving and loading is a relatively simple process.

# Saving

* Go to *File Save As...*
* Choose a file path to save to
* Click *Save*

# Loading

* Go to *File Open...*
* Choose an SCMP file to open
* Click *Open*

# Keep Resources Relative

Typically, Skin Composer uses absolute file paths to resources it uses. If you plan on sharing a save file with others, it would be a good practice to enable *Keep Resources Relative*. This makes a duplicate copy of any asset you import and places it in a data folder relative to your save file. This adds the potential complexity of having multiple folders with resources that you would need to update if you need to make a change. Go to *Project >> Settings* and check *Keep resources relative* to activate this feature per project.

# Missing Files on Loading a Project

Skin Composer depends on asset files to build skins and show previews. If these files, such as images and fonts, are moved or deleted, Skin Composer would not be able to operate. To resolve this, a missing files dialog will appear when opening a project with missing assets. You will be asked to locate these assets before proceeding. This works well when moving from one computer to another.