# Latex Related Questions

## Introduction
We usually use Latex to generate documents.  
Here are some record of the problems I encountered.

## Problem and Solutions

### FreeSerif not found
Recently I need to compile my sphinx based document into PDF, it complains:
```
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!
! fontspec error: "font-not-found"
!
! The font "FreeSerif" cannot be found.
!
! See the fontspec documentation for further information.
!
! For immediate help type H <return>.
```
**Solution:**  
The `FreeFont` fonts in Ubuntu xenial are provided by package `fonts-freefont-otf`, and e.g. in Fedora 29 via package `texlive-gnu-freefont`, for my case:
```shell
sudo apt-get install fonts-freefont-otf
```

### Fandol not found
Recently I need to compile my sphinx based document into PDF, but my ubuntu system do not have Chinese font `Fandol`:
```
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!
! fontspec error: "font-not-found"
!
! The font "FandolSong-Regular" cannot be found.
!
! See the fontspec documentation for further information.
!
! For immediate help type H <return>.
```
**Solution:**  
1. Try to find the font using:  
	```shell
	fc-list :lang=zh
	```
	Here `:lang=zh` is used because `Fandol` is a Chinese font.  
	If you can not find the font you are looking for, proceed to step 2. Otherwise, something else is wrong.

2. Search for the web and download your font. For my case, font `fandol` is at: [CTAN](https://ctan.org/tex-archive/fonts/fandol)

3. Copy the `*.otf` files to system font folder and refresh the cache: 
	```shell
	sudo mv fandol/* /usr/share/fonts/fandol/
	sudo fc-cache -fsv
	```