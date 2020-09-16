# 🔁 data-mocker

## Introduction

* 🎮   Live demo : https://meyerka.github.io/data-mocker/

data-mocker is an easy way to mock flat files for your mocking and testing purposes.
It allows you to choose between a range of different fields, user-defined list, or even randomized data sets.


## How to set-up your extraction

* Click on the live demo's link up there ☝

* Name your fields and select the type of data.
** Range type: Range: Select the min and max values 
** Range type: List: Select a pool of values to choose from
** Range type: Random: It will randomly fetch from a dataset based on faker api

* Unicity: The higher the value for the unicity, the more unique values will be present for this field. For example, if maxed out, there will be only one value for this field, for all lines.

* Group: You can group 2 or more fields to "pair" their values. It will ensure the set of values will be coherent throughout the file rows. For example, pair first name, name, and employee number to make sure the trio will always be the same for a given name. 


## Licensing and acknowledgment
* ©     MIT Licensed.


## Project setup (if you want run data-mocker locally)
```
npm install
```
### Compiles and hot-reloads for development
```
npm run serve
```
### Compiles and minifies for production
```
npm run build
```