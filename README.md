# Readme

osp-documentation is a meta documentation around OpenSourcePolitics:
[osp-documentation](https://osp-documentation.herokuapp.com/)

You must be a member of the github documentation team to access this website.

## How to contribute
You can contribute by creating files directly on github, this is currently the easiest method since it doesn't require configuration / dependencies installation.

### File Structure
The following folders contains the documents displayed on website :
* Docs
* Posts

### To create a new document
Let's say you wanted to create a new file in the commercial category.

* Click on `_docs`.
* Click on `Commercial`.
* Click on `Create new file`.

* Fill the filename with your desired name :  e.g. `filename.md` (the .md is crucial)

-> Your newly creatd document should appear.

Since we use *Jekyll* to render our documentation you should copy add the following to the top of your document : 

```
---
title: Exemple #1
category: Commercial
order: 1
---

```
Where `Exemple #1` is of course the title of your document.

* Below this cartridge, fill the document with your desired text.
* At the footer of the page, look for the `commit changes` section, enter an explanatory message e.g. : `Add documentation on commercial process`.
* click on `commit changes`
-> The documentation website should update in 20-30 seconds.

## How to install
You will need to install `git`, `ruby`, and `jekyll`.

```bash
git clone https://github.com/OpenSourcePolitics/osp-documenation.git
gem install bundler
bundle install
```

* Launch local server :
```
bundle exec jekyll serve
```

* You will find a markdown editor on `localhost:4000/admin`
