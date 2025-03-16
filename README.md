# Technical Writing Assignment

For guidance on setting up and submitting this assignment, refer to the Marcy lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/fullstack-curriculum/how-tos/working-with-assignments#how-to-work-on-assignments).

## Prompt 1

In your own words, explain why React is a popular choice for building user interfaces. Make sure to mention at least one benefit, such as how it simplifies development, supports reusable components, or helps optimize performance. Feel free to include any specific features you find particularly helpful.

### Response 1

React simplifies the process of creating dynamic and interactive user interfaces; the foundation of React lies in its component-based architecture. This approach allows developers to build reusable UI components that can be combined to create complex user interfaces. One of the key benefits of using React is reusability; once you create a component, you can use it across different parts of your application without duplicating code. This allows for your application to be more consistent. One particular feature that I find helpful is that React also optimizes performance through its Virtual DOM, which decreases the number of direct updates to the actual DOM. Instead of re-rendering the entire page when something changes, React efficiently updates only the necessary parts, improving the speed and responsiveness of the application. 

## Prompt 2

Explain how the useState hook is used in React to manage state within functional components. In your response, include an example of how useState might be used in a simple application and why managing state is important in building interactive user interfaces.

### Response 2

The useState hook in React is used to manage state within functional components, allowing components to store and update data that affects their rendering. It takes an initial state as an argument and returns an array with two elements: the current state value and a function to update the state. 
```
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // Declare state with an initial value of 0

  return (
    <div>
      <p>Current Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
      <button onClick={() => setCount(count - 1)}>Decrease</button>
    </div>
  );
}

export default Counter;
```
One of the reasons managing state is important in building is interactivity, components need to react to user actions, like button clicks or form inputs.

Another reason is that Dynamic UI Updates State changes trigger re-renders, ensuring that the UI stays up-to-date.

Another reason is Encapsulation. Each component manages its state independently, making the app modular and easier to maintain.
Using useState, developers can build interactive and responsive user interfaces efficiently without relying on complex class-based components.

## Prompt 3

Describe the different ways the useEffect hook can be triggered in a React component. Include an explanation of how the dependency array influences its behavior. If possible, provide a code example for each scenario to illustrate your explanation.

### Response 3

The useEffect hook in React is a tool that lets you run some code after your component has rendered. It’s like saying, "After the component is on the screen, do this." How often that code runs depends on what you put in the dependency array. Here are the different ways you can use it:

If you don't provide a dependency array, useEffect will execute after every render of the component. This means that any time your component updates—whether due to state changes, prop changes, or parent component re-renders—the effect will run again. This approach can be useful for debugging but may lead to performance issues if overused.
jsx
useEffect(() => {
  console.log("Effect runs after every render");
});

By passing an empty array ([]) as the dependency array, useEffect runs only once, right after the component mounts. This is ideal for tasks like fetching data or setting up subscriptions, as it ensures the effect runs a single time when the component is first rendered.
jsx
useEffect(() => {
  console.log("Effect runs only once on mount");
}, []);

When you include specific variables (state or props) in the dependency array, useEffect runs only when those variables change. This allows you to control when the effect should re-run based on changes to particular pieces of data.
jsx
useEffect(() => {
  console.log("Effect runs because 'count' changed");
}, [count]);

useEffect can return a cleanup function that runs before the component unmounts or before the effect re-runs due to a dependency change. This is useful for cleaning up resources like timers or event listeners to prevent memory leaks.
jsx
useEffect(() => {
  console.log(`Effect runs for count: ${count}`);

  return () => {
    console.log(`Cleanup for previous count: ${count}`);
  };
}, [count]);
*Basically:*
- *No dependency array:* Runs after every render.
- *Empty array ([]):* Runs only once when the component mounts.
- *Specific dependencies:* Runs when those dependencies change.
- *Cleanup function:* Executes before unmounting or before the effect re-runs.
Understanding these patterns helps you control when side effects occur in your React components, ensuring they behave as intended.

## Prompt 4

The component below makes a mistake when using useEffect. When running this code, we will get an error from React! Please fix this code.

```js
const DogDisplay = () => {
  const [imgSrc, setImgSrc] = useState('https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg');

  useEffect(async () => {
    try {
      const response = await fetch('https://dog.ceo/api/breeds/image/random');
      if (!response.ok) throw new Error(`Error: ${response.status}`)
      const data = await response.json();
      setImgSrc(data.message);
    } catch (error) {
      console.error(error);
    }
  }, []);

  return <img src={imgSrc} />
}
```

After fixing the code provide and explanation to what you fixed and why it needed to be fixed.

### Response 4

The issue with the original code is that you can’t directly use an async function inside useEffect. React expects the function passed to useEffect to be synchronous, but when you make it async, it returns a promise, which causes an error.
To fix this, I moved the async function inside the useEffect and called it from there, so everything works properly.
Here’s the fixed code:
js
const DogDisplay = () => {
  const [imgSrc, setImgSrc] = useState(
    "https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg"
  );

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch("https://dog.ceo/api/breeds/image/random");
        if (!response.ok) throw new Error(`Error: ${response.status}`);
        const data = await response.json();
        setImgSrc(data.message);
      } catch (error) {
        console.error(error);
      }
    };
    fetchData();
  }, []);

  return <img src={imgSrc} alt="Random Dog" />;
};

Instead of making the useEffect callback itself async, I created an inner function called fetchData that's async and then called it. This fix ensures that React handles the effect correctly and doesn't run into issues with promises.
