# Notebook-React
TODO app saves notes on your browser's local storage

This app saves user's notes in browser local storage. User able adds a note by click plus (orange round button in right corner). A user can edit note by click on the pencil or delete a note. Pretty simple.

*Some code explanation:* 

* Here one parent Component 'Main' and two children. 

* Form and Note components lift their state up, to the Main component by using this.props.functions.

* In Form and Note components was used conditional rendering (marked with comments).

* So much boilerplate code, because it's a controlled components (e.g. handling user's interactions on forms). 

* Also localStorage we should set Item as array, for this we need JSON parse and stringify functions. 

I don't include links to libraries and frameworks. As you can see on demo, we need Babel as transpiler, React and ReactDom, SCSS as preprocessor for style sheet, Bootstrap 4 and Query


Javascript code
===
```javascript 

class Main extends React.Component {
   constructor(props) {
    super(props);     
    this.newNote = this.newNote.bind(this); //when save form 
    this.saveNote = this.saveNote.bind(this); //localStorage & this.state
    this.removeNote = this.removeNote.bind(this); 
    this.updateNote = this.updateNote.bind(this); //whan clicked save-icon in a note
    
    this.state = {notes:[]};
  }
  
  componentDidMount() {
    let notes = localStorage.getItem("notes");
      if (notes)
         this.setState({notes: JSON.parse(notes)});
  }    
  
  newNote(title, text) { //prepend new object   
    let notes = [{title: title, text: text}].concat(this.state.notes);   
    this.saveNote(notes);
  } 
  
  saveNote(notes) {
    localStorage.setItem('notes', JSON.stringify(notes)); 
    this.setState({notes: notes});
  } 
  
  removeNote(index) {
    let notes = this.state.notes;
    notes.splice(index, 1);
    this.saveNote(notes);
  }
  
  updateNote(index, title, text) {
    let notes = this.state.notes;
    notes[index].title = title;
    notes[index].text = text;
    this.saveNote(notes);
  } 
       
  render() { //Conditional rendering: Ternary operator shows saved notes from local storage. Or plain text
   const instruction = <div className="col-xs-12 col-sm-6 offset-sm-3">The app uses local sotarage to keep changes in the client's                    cache. Click orange round button &#x02197; to add a new note. You can edit or delete notes. 
                         If you delete your browser's temporary files then you will lose your notes. </div>
          
   let notes = this.state.notes.map((obj, i) =>  
               <Note key={i} index={i} title={obj.title} text={obj.text} onUpdate={this.updateNote} onRemove={this.removeNote} />
               );      
    
    return ( <div className="container-fluid">
        
              <div className="row header">
                <h1 className="col-2">Notepad</h1>
                <div className="col-1 offset-7 offset-md-9">
                  <button  type="button" className="btn btn-warning" data-toggle="collapse" data-target="#form">+</button>
                </div>
              </div>
              
              <Form onSend={this.newNote}/>
        
              <div className="container-fluid">  
                  {this.state.notes.length > 0 ? notes : instruction }         
              </div> 
        
              <div className="row footer">
                <h5 className="about">Made by <a href="https://github.com/VladislavMoroshan" target="_blank"> this</a> guy</h5>                           </div>
          </div> 
    )
  }
}
 
class Form extends React.Component {
  constructor(props) {
    super(props);    //controlled component, so it needs so many handlers
    this.changeTitle = this.changeTitle.bind(this);
    this.changeText = this.changeText.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.handleCancel = this.handleCancel.bind(this);
    
    this.state = {title: '', text: ''};
  }
  
  changeTitle(e) {
    this.setState({title: e.target.value});
  }
  
  changeText(e) {
    this.setState({text: e.target.value});
  }
  
  handleSubmit(e) { //here is a "Lift state up" on the parent component
    this.props.onSend(this.state.title, this.state.text); 
    this.handleCancel(e); //reset form text and cancel reload of the page
  }
  
  handleCancel(e) {
    this.setState({title: '', text: ''});
    e.preventDefault();
  }
  
  render() {
    return ( <form className="collapse" id="form">            
                  <div className="form-group">
                     <label htmlFor="exampleInputEmail1">Title</label>
                     <input type="text" value={this.state.title} onChange={this.changeTitle} 
                            className="form-control" placeholder="Enter title" />    
                  </div>

                  <div className="form-group">
                     <textarea  name="text" value={this.state.text} onChange={this.changeText} 
                                placeholder="Enter message" className="form-control" rows="4"/>
                  </div>

                  <button onClick={this.handleSubmit} className="btn">Save</button>
                  <button onClick={this.handleCancel} className="btn">Cancel</button> 
              </form>  )
  }
}

class Note extends React.Component {
  constructor(props) {
    super(props); 
    this.changeTitle = this.changeTitle.bind(this);
    this.changeText = this.changeText.bind(this);
    
    this.edit = this.edit.bind(this); //to the parent
    this.delete = this.delete.bind(this); //to the parent
    
    this.state = {title: this.props.title, text: this.props.text, editing: false}; //by default render as text
  }
      
  edit() { //lift state up to the parent 
    this.props.onUpdate(this.props.index, this.state.title, this.state.text);
    this.setState({editing: !this.state.editing});
  }
  
  delete() { //lift state up to the parent    
    this.props.onRemove(this.props.index);
  }
  
  changeTitle(e) {
    this.setState({title: e.target.value});
  }
  
  changeText(e) {
    this.setState({text: e.target.value});
  }
  
  renderNoteOrEdit() { //when clicks edit button pencil-icon toggle between input and div
    if(this.state.editing) {
      return (<div className="inner">   
                <div className="title">
                     <span>Title</span>
                     <button type="button" className="btn del" onClick={this.delete}><i className="fa fa-trash"></i></button>  
                     <button type="button" className="btn save" onClick={this.edit}><i className="fa fa-floppy-o"></i></button>
                </div>
                          
                <div className="form-group">
                    <input type="text" value={this.state.title} onChange={this.changeTitle} className="form-control" />  
                </div>                    
                <div className="form-group">
                    <textarea  name="text" value={this.state.text} onChange={this.changeText} className="form-control" rows="4"/>
                </div>
          
             </div>)
    } else {
       return (<div className="inner">
                 <div className="title">
                     <h2>{this.props.title}</h2>
                     <button type="button" className="btn del" onClick={this.delete}><i className="fa fa-trash"></i></button>
                     <button type="button" className="btn" onClick={this.edit}><i className="fa fa-pencil"></i></button>
                 </div>
                 <div className="text"> 
                   <p>{this.props.text}</p>
                 </div>
               </div>)
    }
  }
  render() { //render function based on value {this.state.editing}
    return ( <div className="note col-sx-10 col-sm-6 col-md-4">
                {this.renderNoteOrEdit()}
             </div>
    )
  }
}

ReactDOM.render(<Main/>, document.getElementById('app'));

```
