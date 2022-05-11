---
layout: post
title: How to create pop-up with different parameters for generated elements?
categories: [Next.js]
author_github: negativename
author: Vladislav Kobenko
---

In case you have generated elements with different parameters and you want to create pop-up elements with these parameteres in `Next.js`, you can use following method.

## Creating pop-up

Let's create a simple pop-up in `Next.js`. Create `popup.js` file and paste there the following code:

```jsx
export default function Popup(props) {
    const data = props.data.test;

    return (
        <div className="popup">
            <div className="popup-text">
                <p>{data}</p>
            </div>
            <button title="Close" onClick={props.handleClose} className="popup-button">×</button>
        </div>
    )
}
```

Now we have the pop-up component with data and close button. 

## Adding pop-up into application

Open your main file, for example `index.js`. Import the pop-up component to this file.

```js
import Popup from '../components/popup'
```

Create a statement for opening and closing pop-up. We will use `useState` hook for changing the state. Set initial value as `false` to keep the pop-up closed as default:

```js
const [isOpen, setIsOpen] = useState(false);
```

Also create a statement for changing the data. You can set initial value to `0`, as we will change it upon click later:

```js
const [data, setData] = useState(0);
```

Now create a handler to toggle pop-up and update the data.

```js
const togglePopup = (key) => {
    setIsOpen(!isOpen);
    data = setData(data[key]);
  }
```

In the end we need to call this handler on click. For example, we have an array `data` with different dynamically generated objects and want to see a pop-up with unique data for each element.

```jsx
export default function Test({data}) {

const [isOpen, setIsOpen] = useState(false);
const [data, setData] = useState(0);

const togglePopup = (key) => {
    setIsOpen(!isOpen);
    data = setData(data[key]);
  }

return (
    <>
        {isOpen && <Popup
                    data= {test : data}
                    handleClose={togglePopup}
                    />
        }

        <div className="test">
            {data.map((object, i)=> {
                return(
                    <div className="col" key={i}>
                        <span onClick={() => {togglePopup(i)}} />
                        <div className="data-text">
                            <span>{object.text}</span>
                        </div>
                    </div>
                )
            })}
        </div>
    </>
```

It would be nice if we could make our code a bit more concise.