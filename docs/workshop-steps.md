# Onboard like a PRO with Shepherd JS, React, and [Kintone Web Database](https://kintone.dev/en/)

## Outline <!-- omit in toc --> <!-- markdownlint-disable MD007 -->

- [Onboard like a PRO with Shepherd JS, React, and Kintone Web Database](#onboard-like-a-pro-with-shepherd-js-react-and-kintone-web-database)
  - [Get Started](#get-started)
  - [Create a Kintone Web Database App](#create-a-kintone-web-database-app)
  - [Create a `.env` file](#create-a-env-file)
  - [Edit content.js](#edit-contentjs)
    - [Add some fun HTML Elements](#add-some-fun-html-elements)
      - [Fruits](#fruits)
      - [Number Clicker](#number-clicker)
      - [Radio Button](#radio-button)
      - [Display a treasure chest](#display-a-treasure-chest)
  - [Edit App.js](#edit-appjs)
    - [Build a Shepherd JS Tour](#build-a-shepherd-js-tour)
      - [Steps](#steps)
      - [Button Config](#button-config)
  - [Refactor](#refactor)
  - [Check Your Work](#check-your-work)
<!-- markdownlint-enable MD007 -->


## Get Started

First, let's download the [sean-kintone/shepherd-react-tutorial](https://github.com/sean-kintone/shepherd-react-tutorial) Repo and go inside the folder.

Once you are inside the folder, let's install the dependencies! Once for our backend, and again for our frontend!

```shell
git clone https://github.com/sean-kintone/shepherd-react-tutorial

cd shepherd-react-tutorial

cd backend

npm install

npm start
```

Make a new tab in your console and:

```shell
cd ../frontend

npm install

npm start
```

## Create a Kintone Web Database App

Let's create a **ShepherdJS Onboarding** Kintone App!  
This will keep track of two metrics for visitors to our site:

1) Did Users complete the task?
2) Did Users finished our onboarding?

![images/kintone-app-setup.gif](images/kintone-app-setup.gif)

Here are the required fields & their configurations for our workshop:

| Field Type   | Field Name        | Field Code  | Note                                           |
| ------------ | ----------------- | ----------- | ---------------------------------------------- |
| Radio Button | Task Completed    | `completed` | Options: 'Yes' and 'No'                        |
| Number       | Percent Completed | `percent`   | This will show onboarding tour completion rate |

Be sure to click the **Save** and **Activate App** buttons! 💪

Confused? 🤔 → Check out the [How to Create a Kintone Database App](https://youtu.be/pRtfn-8cf_I) video 📺

## Create a `.env` file

Using the [.env.example](./../.env.example) file as a template, create a `.env` file that will contain your login credentials and API Token.

Here is what your `.env` might look like:

```txt
SUBDOMAIN = "example"
APPID = "1"
APITOKEN = "fj3iaoJ1IFJ#*@(328jf3m3lwf"
```

⚠️ DO NOT DELETE THE [.env.example](./../.env.example) FILE!  
[.env.example](./../.env.example) is used by env-cmd to verify that `.env` file is correctly configured.

## Edit content.js

Today, we're just working on our frontend logic, so we've set up our backend express server, and API Post logic for you. You can check out our video on how we set up our backend [here](https://www.youtube.com/watch?v=LF2ue7ePgyU).

For this project, just know that we've set up our API POST Logic here in [/frontend/src/requests/postRecord.js](frontend/src/requests/postRecord.js), and we can import and call the PostRecord function in our code like so:

```js
import PostRecord from './requests/postRecord.js';
...
    PostRecord({
      completed: 'Yes',
      percent: 100
    });
...
```

Also, we have set up a few React Hooks, in order to keep track of variables and set their state across our app:

```jsx
  const [count, setCount] = useState(0);
  const [radioValue, setRadioValue] = useState("");
  const [onboardingPercent, setOnboardingPercent] = useState(0);
  const [fruits, setFruits] = useState("");
  const [stepsComplete, setStepsComplete] = useState(false);
```

We will use the variables, `count`, `radioValue`, `fruits` etc, and set them across our app with their setter functions: `setCount`, `setRadioValue`, `setFruits`.

We have three goals for our coding today:

1. Add some fun HTML Elements to guide our users through

2. Create a ShepherdJS tour to onboard our users

3. Refactor our code to be a little bit neater.

### Add some fun HTML Elements

First, let's look at [content.js](../frontend/src/content.js).
We have set up a 25 x 25 grid, using CSS grid.

```jsx
  return (
    <div className="main-grid">
      <div className="grid-item"></div>
      <div className="grid-item">
        <div className="fruits">
        </div>
      </div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item">
        <div className="button-clicker">
        </div>
      </div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item">
      </div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      {treasureClicked ? (
        <div className="grid-item"></div>
      ) : (
        <div className="grid-item"></div>
      )}
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
      <div className="grid-item"></div>
    </div>
  );
```

Let's add a selector in the fruits area, a clickable div, a radio button, and some treasure chests.

#### Fruits

In reality, this can be anything you want, we are just creating content to guide our users through later. I chose a simple HTML selector, with some options for fruits:

```jsx
        <div className="fruits">
          <label htmlFor="fruits">Choose a fruit:</label>
          <select id="fruits" value={fruits} name="fruits" onChange={(e) => {
            setFruits(e.target.value);
          }}>
            <option value="pineapple">Pineapple</option>
            <option value="blueberry">Blueberry</option>
            <option value="pomegranate">Pomegranate</option>
            <option value="fig">Fig</option>
          </select>
        </div>
```

As long as you bind the value of your selector to the `fruits` hook via `setFruits()`, anything is fine.

#### Number Clicker

Our Number Clicker is a clickable div, in which an `onClick` event listener fires, which adds +1 to our `count` variable. Notice we use the setter function `setCount()`:

```jsx
        <div className="button-clicker">
          <button onClick={() => {
            setCount(count + 1);
          }}>
            {count}
          </button>
        </div>
```
On our flat, invisible button, we display the current `count` variable using `{count}`. Due to React keeping track of the state of our variable, this changes in real time, as you click the div and add to the count. Cool!

#### Radio Button

Our radio buttons are part of a form, which updates the `radioValue` variable in real time.

```jsx
      <div className="grid-item">
        <form onChange={(e) => {
          setRadioValue(e.target.value);
        }}>
          <div className="radio-buttons">
            <input type="radio" name="radio-buttons" id="one" value="one" />
            <label htmlFor="one">One</label>
            <input type="radio" name="radio-buttons" id="two" value="two" />
            <label htmlFor="two">Two</label>
          </div>
        </form>
      </div>
```

In the form's `onChange`, we set `radioValue` to whatever was clicked, via our `setRadioValue()` hook.


#### Display a treasure chest

Lastly, we need to display our treasure chests. If we have met the criteria, we should be able to open the treasure chest `onClick`. Otherwise, we should display a closed treasure chest.

```jsx
      {treasureClicked ? (
        <div className="grid-item"><OpenChest /></div>
      ) : (
        <div className="grid-item" onClick={openTreasure}><Chest /></div>
      )}
```

Here we use a `ternary` operator, the `?` seen above. This is just a fancy way of writing an `if` statement in React, and is commonly used to conditionally display content. We could also re-write the above code like so:

```jsx
      {if (treasureClicked) {
        <div className="grid-item"><OpenChest /></div>
      } else {
        <div className="grid-item" onClick={openTreasure}><Chest /></div>
      }}
```


## Edit App.js

Save your work and run kintone-customize-uploader by entering `npm run start` in your terminal!  
Navigate to your app, create a record, write some Markdown, and click the publish button!

See the [slides.pdf](../slides.pdf) for more info!

### Build a Shepherd JS Tour

#### Steps

#### Button Config

## Refactor

## Check Your Work

Is your code not working?

Compare your [./src/main.ts](../src/main.ts) with the [completed-code.md](./completed-code.md) to see if it is all written correctly.

Still got a problem?

Check out README's [Debugging](../README.md#debugging---lets-fix-those-problems) section!

And finally, post your Kintone customization questions over at our community forum:  
[forum.kintone.dev](https://forum.kintone.dev/)

Good luck coding! 💪
