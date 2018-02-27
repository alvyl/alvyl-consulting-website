# alvyl-consulting-website
alvyl-consulting-website

This contains the raw gohugo template data that will generate the website used by [the alvyl.github.io project](https://github.com/alvyl/alvyl.github.io).

## Steps to Generate the files

- Make changes in [this project](https://github.com/alvyl/alvyl-consulting-website).
- Run `hugo server -D` from this project directory to see the changes. Once you are satisfied with the changes,
- Remove all files from **public** folder by running `rm -rf public/`.
The contents of the public folder is not to be pushed as part of this repo. This is because, the contents of the public folder has the static content of the website, whcih is actually managed as part of the [alvyl.github.io project](https://github.com/alvyl/alvyl.github.io). 
- Raise PR for the changes made in this project repo. 
- To push the changes to the website (alvyl.com), we need to generate the public folder with all the contents under the **alvyl.github.io** fork of yours. To do that, 
- Run `hugo -d ../alvyl.github.io` assuming that the `alvyl.github.io` project and the alvyl-consulting-website projects are in the same parent folder and at same levels. 
If not, then run the command `hugo -d <<path_to_alvyl.github.io_fork>>`.
- Raise PR with the generated contents. On merging this, the changes must be visible in alvyl.com website.
- Please talk to goutham@alvyl.com and/or hari@alvyl.com for any queries/more info on this. 
 
 
  

