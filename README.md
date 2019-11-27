# Notebook

View demo [here at Codepen](https://codepen.io/Moroshan/full/RpRRYp)

This app saves user's notes in browser local storage. User able adds a note by click plus (orange round button in right corner). A user can edit note by click on the pencil or delete a note. Pretty simple.

*Some code explanation:* 

* Here one parent Component 'Main' and two children. 

* Form and Note components lift their state up, to the Main component by using this.props.functions.

* In Form and Note components was used conditional rendering (marked with comments).

* So much boilerplate code, because it's a controlled components (e.g. handling user's interactions on forms). 

* Also localStorage we should set Item as array, for this we need JSON parse and stringify functions. 

I didn't include links to libraries and frameworks. As you can see on demo, we need Babel as transpiler, React and ReactDom, SCSS as preprocessor for style sheet, and Bootstrap 4


