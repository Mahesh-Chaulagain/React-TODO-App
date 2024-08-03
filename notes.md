# To create react app with Next.js
-> Navigate to your preferred directory and run the following command:
    >> npx create-next-app@latest todo
-> once the installation is complete, navigate into your newly created app directory and start the development server by running:
    >> cd toto
    >> npm run dev
    
    ## or with yarn
    >> cd todo
    >> yarn run dev

# How to build components

1. The Header Component
-> Component: javascript functions that return HTML
-> serves to display the title of our app
-> remains static throughout our app
-> start by creating a directory for components:
    # in the root directory of your project, create a new directory
    >> mkdir "src/components"
    # navigate into the directory
    >> cd src/components
    # create a new file for the Header component
    touch Header.jsx
-> code: 
    function Header(){
    return (
        <>
            <svg>
                <path d="" />
            </svg>
            <h1>TODO</h1>
        </>
    );
}

export default Header;

2. The TODOHero component
-> serves as a selection where we provide an overview of total number of todos and the number of completed tasks
-> is dynamic i.e continuously updates based on the number of completed todos and the total number of todos
-> we identify dynamic parts by passing arguments called props to our component
-> create the TODOHero component
    >> cd src/components
    >> touch TODOHero.jsx
-> In TODOHero.jsx, define a function that takes props as arguments:
    function TODOHero({ todos_completed, total_todos }){
    return(
        <section>
            <div>
                <p>Task Done</p>
                <p>Keep it up</p>
            </div>
            <div>
                {todos_completed}/{total_todos}
            </div>
        </section>
    );
}

export default TODOHero;

3. The Form component
-> has simple input with a submit button 
-> add "use client" on top of the file where you are using handleClick because all components in Next 13 by default are server components, 
    therefore for client side interactivity you need to use "use client".
-> code:
'use client';
import React from "react";

function Form(){
    const handleSubmit = (event) => {
        // prevents the form from submitting and reloading the entire app
        event.preventDefault();
        // reset the form
        event.target.reset();
    };

    return (
        <form className="form" onSubmit={handleSubmit}>
            <label htmlFor="todo">
                <input
                    type="text"
                    name="todo"
                    id="todo"
                    placeholder="write your next task"
                    />
            </label>
            <button>
                <span className="visually-hidden">Submit</span>
                <svg>
                    <path d="" />
                </svg>
            </button>
        </form>
    );
}

export default Form;

4. The TODOList Component
-> start by creating a new component file called TODOList.jsx
-> list will be generated dynamically from the todo data
-> code:
function TODOList(){
    return <ol className="todo_list">
        {/* {list goes herre} */}
    </ol>
}

export default TODOList;

-> create separate component for the list item i.e create "Item" component alongside the "TODOList" component
-> code:
function Item({ item }){
    return(
        <li id={item?.id} className="todo_item">
            <button className="todo_items_left">
                <svg>
                    <circle cx="11.998" cy="11.998" fillRule="nonzero" r="9.998" />
                </svg>
                <p>{item?.title}</p>
            </button>
            <div className="todo_item_right">
                <button>
                    <span className="viually-hidden">Edit</span>
                    <svg>
                        <path d="" />
                    </svg>
                </button>
                <button>
                    <span className="visually-hidden">Delete</span>
                    <svg>
                        <path d="" />
                    </svg>
                    </button>
            </div>
        </li>
    )
}

-> use the "Item" component within our list:
-> code:
function TODOList({ todos }){
    return (<ol className="todo_list">
            {todos && todos.length > 0 ? (
            todos?.map((item, index) => <Item key={index} item={item} />)
        ):(
            <p>Seems lonely in here, what are you up to?</p>
        )}
    </ol>
    );
}

# Putting it all together
-> render the above components in our index page
-> in Next.js pages are located inside the "src/app" directory and the index page is typically named page.js
-> first empty the contents of the file with:
    >> echo -n > src/app/page.js
-> import all the components created and utilize them inside the page.js as:
    import React from "react";
    import Form from "@/components/Form";
    import Header from "@/components/Header";
    import TODOHero from "@/components/TODOHero";
    import TODOList from "@/components/TODOList";

    function Home(){
    return (
        <div className="wrapper">
        <Header />
        <TODOHero todos_completed={0} total_todos={0} />
        <Form />
        <TODOList todos={[]} />
        </div>
    )
    }

    export default Home;

# The Styling
-> create a "styles.css" file to hold our styles
    >> touch src/app/styles.css
-> delete all the CSS files that came with installing Next.js 
    >> rm src/app/page.module.css && src/app/globals.css
-> add your CSS rules in the styles.css file
*,
*::after,
*::before{
    padding: 0;
    margin: 0;
    font-family: inherit;
    box-sizing: border-box;
}

html, 
body{
    font-family: sans-serif;
    background-color: #0d0d0d;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100vw;
}

button{
    cursor: pointer;
}

.visually-hidden{
    position: absolute !important;
    clip: rect(1px, 1px, 1px, 1px);
    padding: 0 !important;
    border: 0 !important;
    height: 1px !important;
    width: 1px !important;
    overflow: hidden;
    white-space: nowrap;
}

.text_large {
    font-size: 32px;
}

.text_small {
    font-size: 24px;
}

.wrapper{
    display: flex;
    flex-direction: column;
    width: 70%;
}

@media(max-width: 510px){
    .wrapper{
        width: 100%;
    }
    header{
        justify-content: center;
    }
}

header{
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: 12px;
    padding: 42px;
}

.todohero_section{
    border: 1px solid #c2b39a;
    display: flex;
    align-items: center;
    justify-content: space-around;
    align-self: center;
    width: 90%;
    max-width: 455px;
    padding: 12px;
    border-radius: 11px;
}

.todohero_section div:last-child{
    background-color: #88ab33;
    width: 150px;
    height: 150px;
    border-radius: 75px;
    font-size: 48px;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
}

.form{
    align-self: center;
    width: 97%;
    max-width: 455px;
    display: flex;
    align-items: center;
    gap: 12px;
    margin-top: 38px;
}

.form label{
    width: 90%;
}

.form input{
    background-color: #1f2937;
    color: #fff;
    width: 100%;
    height: 50px;
    outline: none;
    border: none;
    border-radius: 11px;
    padding: 12px;
}

.form button{
    width: 10%;
    height: 50px;
    border-radius: 11px;
    background-color: #88ab33;
    border: none;
}

.todo_list {
    align-self: center;
    width: 97%;
    max-width: 455px;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 27px;
    margin-bottom: 27px;
    gap: 27px;
}

.todo_item,
.edit-form input{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 70px;
    width: 100%;
    max-width: 455px;
    border: 1px solid #c2b39a;
    font-size: 16px;
    background-color: #0d0d0d;
    color: #fff;
    padding: 12px;
}

.edit-form input{
    outline: transparent;
    width: calc(100% - 14px);
    height: calc(100% - 12px);
    border: transparent;
}

.todo_items_left,
.todo_items_right{
    display: flex;
    align-items: center;
}

.todo_items_left {
    background-color: transparent;
    border: none;
    color: #fff;
    gap: 12px;
    font-size: 16px;
}

.todo_items_right{
    gap: 4px;
}

.todo_items_right button{
    background-color: transparent;
    color: #fff;
    border: none;
}

.todo_items_right button svg{
    fill: #c2b39a;
}

-> import the css file to the layout.js file as:
    import "./styles.css";


## Build the Functionality: How to add todos

# A way to store the TODO data
-> we need a method to store our todo data, this is accomplished using state - a javascript object that holds information about a component's state
-> React provide a hook called "useState()", which enables us to manage state in our React apps
-> But in Next.js, before utilizing "useState" you need to specify that the component is a client component by adding the following code to the top of your component file
    "use client";

-> use the "useState" hook to create a state for our todo data
    "use client";
    import React from "react";
    import Form from "@/components/Form";
    import Header from "@/components/Header";
    import TODOHero from "@/components/TODOHero";
    import TODOList from "@/components/TODOList";

    function Home(){
    const [todos, setTodos] = React.useState([]);
    return (
        <div className="wrapper">
        <Header />
        <TODOHero todos_completed={0} total_todos={0} />
        <Form />
        <TODOList todos={[]} />
        </div>
    )
    }

    export default Home;

-> in above code snippet, the useState initally holds a empty array
-> it's important to understand that "useState" returns two values:"todos" and "setTodos"(can name these anything we prefer)
-> the first value "todos" holds the current value of the state, while "setTodos" is a function used to update the state

# What kind of data do we want to store?
-> define the type of data we intend to store, essentially it will be an array of objects, where each object holds the necessary information to render our lists of todos i.e 
    const [todos, setTodos] = React.useState([
        { /* Object */ },
        { /* Object */ },
        { /* Object */ },
   ]);

-> each object in the array will have the following structure:
    {
        title: "Some task", // string
        id: self.crypto.randomUUID(), // string
        is_completed: false // boolean
    }
where,
    self.crypto.randomUUID() is a method that allows the browser to generate unique IDs for each todo item.

# How to pass the todo data to our component
-> concept called state sharing allows children components to access the state of their parent component which means that the todo state we created earlier can be shared among all our components
-> pass the state to the List component as :
"use client";
import React from "react";
import Form from "@/components/Form";
import Header from "@/components/Header";
import TODOHero from "@/components/TODOHero";
import TODOList from "@/components/TODOList";

function Home(){
  const [todos, setTodos] = React.useState([
    {
      title: "Some task", // string
      id: self.crypto.randomUUID(), // string
      is_completed: false // boolean
  },
  {
    title: "Some other task", // string
    id: self.crypto.randomUUID(), // string
    is_completed: true // boolean
},
{
  title: "Last task", // string
  id: self.crypto.randomUUID(), // string
  is_completed: false // boolean
}
  ]);

  return (
    <div className="wrapper">
      <Header />
      <TODOHero todos_completed={0} total_todos={0} />
      <Form />
      <TODOList todos={todos} />
    </div>
  )
}

export default Home;

-> since we already made provisions in our List component to receive a "todo" prop, the todo prop will be populated by the data from our state
-> in the "TODOHero" component we don't need all of the data in that component - we just need to count the total number of todos and the number of completed todos
"use client";
import React from "react";
import Form from "@/components/Form";
import Header from "@/components/Header";
import TODOHero from "@/components/TODOHero";
import TODOList from "@/components/TODOList";

function Home(){
  const [todos, setTodos] = React.useState([
    {
      title: "Some task", // string
      id: self.crypto.randomUUID(), // string
      is_completed: false // boolean
  },
  {
    title: "Some other task", // string
    id: self.crypto.randomUUID(), // string
    is_completed: true // boolean
},
{
  title: "Last task", // string
  id: self.crypto.randomUUID(), // string
  is_completed: false // boolean
}
]);

const todos_completed = todos.filter((todo) => todo.is_completed === true).length;
const total_todos = todos.length;

return (
    <div className="wrapper">
      <Header />
      <TODOHero todos_completed={todos_completed} total_todos={total_todos} />
      <Form />
      <TODOList todos={todos} />
    </div>
  )
}

export default Home;

# Adding more data to our state
-> purpose of creating a Form component was to enable us to create new todos ourselves, not rely on dummy data
-> since we have access to the todo state data, we can also update the state of a parent from a children component i.e we can pass the function used to update the state(setTodos) to our Form component
    <Form setTodos={setTodos} />
-> with access to the "setTodos" function in our Form component, we can now add new todos to our state when we submit the form
     // add todos from the form  
      const value = event.target.todo.value;
      setTodos((prevTodos) => [
        ...prevTodos,
        {title: value, id: self.crypto.randomUUID(), is_completed: false},
      ])
which is equivalent of doing the following in plain javascript
    let prevTodos = [];

    prevTodos.push({
        title: value,
        id: self.crypto.randomUUID(),
        is_completed: false
    })

# How to mark Todos as complete
-> List component contains <li> element with buttons. here we are going to attach the "onClick" event handler to the first button
      function Item({ item }) {

        const completeTodo = () => {
            // perform some action
        };

        return (
          <li id={item?.id} className="todo_item" onClick={completeTodo}>
-> when we click on this button and the "completeTodo" handler is invoked, our objective is to:
    1. Filter the data to find the todo that was clicked
    2. Modify the data and set the "is_completed" value to true
-> Before we can proceed with the data modification, we need access to the "setTodo" function in our <Item /> component
-> we can pass the "setTodo" function from the "<List /> component to our <Item /> component 
    <TODOList todos={todos} setTodos={setTodos} />
-> Then within our <List /> component, we pass the setTodo function to our <Item /> component
        return (
        <ol className="todo_list">
          {todos && todos.length > 0 ? (
            todos?.map((item, index) => <Item key={index} item={item} setTodo={setTodo}/>)
          ) : (
            <p>Seems lonely in here, what are you up to?</p>
          )}
        </ol>
      );
-> now, within the <Item /> component, we can use the "setTodos" function to update the todo's "is_completed" status when the button is clicked:
    function Item({ item, setTodos }){
        const completeTodo = () => {
            setTodos((prevTodos) => prevTodos.map((todo) => todo.id === item.id 
            ?{...todo, is_completed:!todo.is_completed}
                :todo
            )
        );
    };
    }
-> when todo is marked as completed, enhance its visual representation as:
    <li id={item?.id} className="todo_item">
            <button className="todo_items_left" onClick={completeTodo}>
              <svg fill={item.is_completed ? "#22c55e" : "#0d0d0d"}>
                <circle cx="11.998" cy="11.998" fillRule="nonzero" r="9.998" />
              </svg>
              <p style={item.is_completed?{ textDecoration:"line-through" } : {} }>{item?.title}</p>
            </button>

# How to edit todos
-> when editing todos we want to have a form in which we can edit the title of the todo. when the button is clicked we want to swap out everything in the <li> and have a form instead
-> first thing to do was create a state:
    const [editing, setEditing] = React.useState(false);
-> when the edit button is clicked we set the value of our editing state to true which will render our form:
    const handleEdit = () => {
        setEditing(true);
    }
-> when we submit the edit todo form by pressing enter, we also want to set the variable back to false so we can get back our list
    const handleInpuSubmit= (event) => {
        event.preventDefault();
        setEditing(false)
    } 
-> when we mouse out of the edit form, we also want to set the state back to false:
    const handleInputBlur = () => {
        setEditing(false);
    }
-> Another thing to do is to focus the input once editing is set to true:
    React.useEffect(() => {
        if(editing && inputRef.current){
            inputRef.current.focus();
            inputRef.current.setSelectionRange(
                inputRef.current.value.length,
                inputRef.current.value.length
            )
        }
    },[editing]);

-> the edit todo itself has a single input field with an "onChange" event. As we edit the title in the input field, we want to modify the current todo with the updated title
    const handleInputChange = (e) => {
        setTodos((prevTodos) =>
        prevTodos.map((todo) =>
        todo.id === item.id ? {...todo, title: e.target.value} : todo)
        )
    }
    - javasccript "array.map()" method is perfect for this because it returns a new array with the same number of elements after modifying the title

# How to delete todos
-> when delete button is clicked, we filter out the todo that triggered the delete event from the todo list as
    const handleDelete = () => {
        setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== item.id))
    }
-> also add an onClick event to the delete button
    <button onClick={handleDelete}>
        <span className="visually-hidden">Delete</span>

# How to persist our todo data
-> upto this point our todo data has been stored solely in the application's state i.e:
    const [todos, setTodos] = React.useState([])
-> while this approach works, it present a challenge, when the app is reloaded all todo data is lost
-> when it comes to persisting data, we typically think of database
-> storing our data in database offer several advantages, such as easy access from any device but there's and alternative: localStorage
-> localStorage is a browser-based storage system with some limitations like a 5MB storage cap and data accessibility restricted to the browser where it's stored
-> we can only store strings in localStorage and can't store array or objects

# How to persist the todo data to localStorage
-> to add the data to localStorage
    const newTodo = {
        title: value,
        id: self.crypto.randomUUID(),
        is_completed: false,
      };

      // update todo state
      setTodos((prevTodos) => [...prevTodos, newTodo]);

      // convert the array of data to a string
      const updatedTodoList = JSON.stringify([...todos, newTodo]);
      // store the updated todo list in local storage
      localStorage.setItem("todos", updatedTodoList);
-> pass the todo state to the "Form" component
     <Form todos={todos} setTodos={setTodos}/>

-> also pass the todos data to <Item /> component
    <ol className="todo_list">
          {todos && todos.length > 0 ? (
            todos?.map((item, index) => (
            //pass the todos to <Item />
            <Item key={index} item={item} todos={todos} setTodos={setTodos}/>))

-> now that we have access to the todo data in out <Item /> component, we can persist data to localStorage after marking todo as completed:
     const completeTodo = () => {
        setTodos((prevTodos) => prevTodos.map((todo) => todo.id === item.id 
        ?{...todo, is_completed:!todo.is_completed}
        :todo
        )
    );

    // update localStorage after marking todos as completed
    const updatedTodos = JSON.stringify(todos);
    localStorage.setItem("todos", updatedTodos);
    };

-> also persist the data to localStorage after editing 
    const handleInputBlur = () => {
        // update localStorage after editing todos
    const updatedTodos = JSON.stringify(todos);
    localStorage.setItem("todos", updatedTodos);
    setEditing(false);
}
and deleting a todo:
    const handleDelete = () => {
        setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== item.id));
        // update localStorage after deleting todo
        const updatedTodos = JSON.stringify(
            todos.filter((todo) => todo.id !== item.id)
        );
        localStorage.setItem("todos", updatedTodos)
    }

-> now when we create new todos, they'll be persisted in localStorage even after reloading the app

# How to read the todo data from localStorage
-> in src/app/page.js, read the data from the localStorage and store it in todos state
    //Retrieve data from localStorage when component mounts
    React.useEffect(() => {
        const storedTodos = localStorage.getItem("todos");
        if (storedTodos){
        setTodos(JSON.parse(storedTodos));
        }
    },[])
-> the part that reads the data from localStorage:
    const storedTodos = localStorage.getItem("todos");
-> since data stored in localStorage is a string, convert it back to array of objects before we can use it by:
    JSON.parse(storedTodos)