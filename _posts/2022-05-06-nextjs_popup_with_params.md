---
layout: post
title: How to create pop-up with different parameters for generated elements?
categories: [Next.js]
author_github: negativename
author: Vladislav Kobenko
---

In case you have generated elements with different parameters and you want to create pop-up elements with these parameteres in `Next.js`, you can use following method.

## Creating pop-up

Let's create simple pop-up in `Next.js`. Create `popup.js` file and paste there following code:

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

Now we have pop-up component, with data and close button. 

## Adding pop-up into application

Open your main file, for example `index.js`. Import pop-up component to this file.

```js
import Popup from '../components/popup'
```

Create statement for opening or closing pop-up. Let's use `useState` hook for changing state. Set initial value as `false` to keep pop-up closed as default:

```js
const [isOpen, setIsOpen] = useState(false);
```

Also create statement for different data. You can set initial value as `0`, because we will change it after click below:

```js
const [data, setData] = useState(0);
```

Now create handler for changing pop-up and data statement.

```js
const togglePopup = (key) => {
    setIsOpen(!isOpen);
    data = setData(data[key]);
  }
```

In the end we need to call this handler on click. For example, if we have array `data` with different objects, which are generated dynamically, you will see pop-up with unique data for every element.

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

It would be nice if we could keep our code a bit more concise.