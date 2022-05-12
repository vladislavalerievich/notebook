# Notebook

View demo [here at Codepen](https://codepen.io/Moroshan/full/RpRRYp). Please *don't open link in incognito mode*! Incognito mode in some browsers produces unexpected behavior for the Note app on Codepen.  

## Description 

This app saves user's notes in browser local storage. User able adds a note by click plus (orange round button in right corner). A user can edit note by click on the pencil or delete a note. Pretty simple.

*Code summary:* 

* One parent Component 'Main' and two children. 

* Form and Note components lift their state up, to the Main component by using this.props.functions.

* In Form and Note components was used conditional rendering (marked with comments).

* So much boilerplate code, because it's a controlled components (e.g. handling user's interactions on forms). 

* In localStorage Item saved as array, for this JSON parse and stringify functions are needed. 

Used libraries and frameworks: React, ReactDom, SCSS as preprocessor for style sheet, Bootstrap 4.


