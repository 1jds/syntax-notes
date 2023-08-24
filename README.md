# Syntax-Notes
Some Notes in Markdown about elements of Syntax for JavaScript and React

## Variable Naming Conventions and Capitalisation

### In Javascript
Javascript generally uses camelCase, but does have some exceptions:
-   camelCase for normal variable names and function names
-   start boolean variable names with 'is' or 'has' but still use camelCase
-   constants can be UPPERCASE or SCREAMING_SNAKE_CASE. These would be used with `const`
-   use PascalCase for the names of classes
-   in React use PascalCase when naming components
-   for more detail and examples, see [this helpful article](https://www.syncfusion.com/blogs/post/10-javascript-naming-conventions-every-developer-should-know.aspx)

### In Python
Python uses different conventions from Javascript
- for general variables, use snake_case

### In Bash


### Environment Variables
For environment variable names the use of UPPERCASE is customary. For example, when storing API keys locally that will be gitignored, the variable name is in screaming snake case.  

`// for Create React App applications
REACT_APP_API_KEY=A858372575838929292175243D

// for Vite applications
VITE_SOME_KEY=823849BHRJS582105657DBSEPQIJ3N`


## When to use brackets of different types or parentheses in JavaScript and React

### In JavaScript
- Use parentheses `( )` for creating or invoking functions
  - The arguments being passed to a function go in here, unless you have an arrow function with just one parameter, in which case the parentheses can be omitted
  
        // Normal function:
            function funky() {/*...function body...*/}
            
        // Normal arrow: 
            const arrowFuncA = () => {/*...function body...*/}
            
        // One arg arrow - `()` can be omitted: 
            const arrowFuncB = arg => {/*...function body...*/}
            
        // Normal arrow as callback:
            const array = [1, 2, 3, 4]
            array.map((item) => {return item * 2})
            
        // One arg arrow as callback - `()` can be omitted:
            const array = [1, 2, 3, 4]
            array.map(item => {return item * 2})
            
        // One arg arrow as callback with implicit return - `()` omitted, `{}` omitted
            const array = [1, 2, 3, 4]
            array.map(item => item * 2)
  - For IIFEs (immediately invoked function expressions), there are additional parentheses. The whole function is written as normal, but then wrapped in `(...)` and another `( )` is attached (to invoke it?).
  
        (function () {
          /*...function body...*/
        })();

        (() => {
          /*...function body...*/
        })();

        (async () => {
          /*...function body...*/
        })();                                       //Source: MDN docs

- When using the implicit return feature in arrow functions and trying to return an object literal, then the value you're trying to return needs to be wrapped in parentheses, so that JS doesn't interpret it as the start of your function body, as this example illustrates:
        // You might want to take this and make it more concise using the implicit return as below:
        
            const objsArray = [{name: "Yuko"}, {name: "Steve"}, {name: "Yuvraj"}]
            const usersAllNamedMax = objsArray.map(item => {
                return {name: "Max"}
            });     // Works
        
            const usersAllNamedMax = objsArray.map(item => {name: "Max"});   // Won't work
        
        // However, since `{}` are used for both object literals and function bodies,
           you can't just write have an implicit return of {name: "Max"} as it will be treated 
           as the function body. Wrapping the object you want to return within `()` solves 
           this problem, since a `()` chunk must be an expression.
        
            const usersAllNamedMax = objsArray.map(item => ({name: "Max"}));   // Works


### In React
- Inside JSX, when wanting to insert regular JavaScript this is done by placing the 'interpolation' inside braces as below. The 'interpolation' in this example is `{username}`:

        <>
            <Header />
            <main>
                <h1>Your name is {username}</h1>
            </main>
            <Footer />
        </>
- When invoking a function or method inside of JSX, then the normal parentheses are omitted. For example, the invocation `handleFavourite` inside `onClick={handleFavourite}` doesn't have parentheses after it such as it would in normal JS `handleFavourite()`:

        function favouriteStar(props) {
            const [ isStarred, setIsStarred ] = React.useState(true);
            
            function handleFavourite() {
                setIsStarred(prevState => !prevState)
            }
            
            return (
                <Star onClick={handleFavourite} />
            )
        };
- Parentheses are used to group a multi-line return statement, especially when using a slab of JSX that is being returned from a component, so as to prevent troublesome automatic semicolon insertion, e.g.:

        return (
        <>
            <form onSubmit={handleSubmit}>
                <input 
                    placeholder="First Name"
                    name="firstName" 
                    value={inputData.firstName}
                    onChange={handleChange}
                />
                <input 
                    placeholder="Last Name"
                    name="lastName" 
                    value={inputData.lastName}
                    onChange={handleChange}
                />
                <br />
                <button>Add contact</button>
            </form>
        </>                                           //Source: Scrimba
        )

- When using in-line styles inside JSX, we need to use **doubled** `{ }` one set inside the other. This is because we need to first escape the JSX into JS, then we need to insert an object with our styles inside of it. This is not exactly the same as writing CSS styling, or normal in-line HTML styling, because we need to use camelCase. Also, if we give bare numbers and omit 'px' then React assumes that unit of measurement. An example of in-line styling:

            function MyComponent(){
                return <div style={{ color: 'blue', lineHeight : 10, padding: 20 }}>
                    Inline Styled Component
                </div>
            }
        
        // This can then be made more readable by moving the styles object literal
           outside of the JSX and inserting it via a variable as below:
        
            const myComponentStyle = {
               color: 'blue',
               lineHeight: 10,
               padding: 20,
            }
            
            function MyComponent(){
                return <div style={myComponentStyle}>
                    Inline Styled Component
                </div>
            }                               // Source: Pluralsight


### In Redux
- In his advanced React course on Scrimba, Bob Ziroll mentions that `const` is scoped to the nearest set of curly braces, and a lot of things can be enclosed in braces in JS. As such, when working with things like `switch` statements for reducers in Redux, it can be helpful to put braces around individual cases within the switch body so that the same `const` variable name can be reused. For example, in the following reducer `const arrCopy =` can be used for multiple switch cases as long as they are scoped with the curly braces. This is instead of doing something like `const arrCopy1`, `const arrCopy2`, &c. *(The reason for making a copy of the array, of course, is to maintain a pure function that doesn't mutate the original array that was passed to the reducer.)*

        function reducer(state = initialState, action) {
            switch(action.type) {
                case "CHANGE_COUNT":
                    return {
                        ...state,
                        count: state.count + action.payload
                    }
                case "ADD_FAVORITE_THING":
                    return {
                        ...state,
                        favoriteThings: [...state.favoriteThings, action.payload]
                    }
                case "REMOVE_FAVORITE_THING": {  <-- HERE
                    const arrCopy = [...state.favoriteThings]  <-- IF NOT USING THE FILTER METHOD AS IN THE LINE BELOW, 
                                                                   ONE COULD MAKE A COPY OF THE ARRAY BEFORE PLAYING AROUND WITH IT

                    const updatedArr = state.favoriteThings.filter(thing => thing.toLowerCase() !== action.payload.toLowerCase())
                    return {
                        ...state,
                        favoriteThings: updatedArr
                    }
                }  <-- AND HERE
                default:
                    return state
            }
        }
