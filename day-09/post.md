---
page_id: 30-days-of-react/day-9
series: 30-days-of-react
permalink: day-9
title: An Introduction to Styling React Components
description: >-
  No application is complete without style. We'll look at the different methods
  we can use to style our components, from traditional CSS to inline styling.
hero_image: /assets/images/series/30-days-of-react/headings/9.jpg
imageUrl: /assets/images/series/30-days-of-react/headings/9.jpg
dayDir: 09
day: 9
introBannerUrl: /assets/images/series/30-days-of-react/headings/9_wide.jpg
date: 'Wed Oct 12 2016 21:29:42 GMT-0700 (PDT)'
imagesDir: /assets/images/series/30-days-of-react/day-9
includeFile: ./../_params.yaml
---

Through this point, we haven't touched the styling of our components beyond attaching Cascading StyleSheet (CSS) class names to components.

Today, we'll spend time working through a few ways how to style our React components to make them look great, yet still keeping our sanity. We'll even work through making working with CSS a bit easier too!

Let's look at a few of the different ways we can style a component.

1. Cascasding StyleSheets (CSS)
2. Inline styles
3. Styling libraries

## CSS

Using CSS to style our web applications is a practice we're already familiar with and is nothing new. If you've ever written a web application before, you most likely have used/written CSS. In short, CSS is a way for us to add style to a DOM component outside of the actual markup itself.

Using CSS alongside React isn't novel. We'll use CSS in React just like we use CSS when _not_ using React. We'll assign ids/classes to components and use CSS selectors to target those elements on the page and let the browser handle the styling.

As an example, let's style our `Header` component we've been working with a bit.

<div class="demo" id="demo1"></div>


Let's say we wanted to turn the header color orange using CSS. We can easily handle this by adding a stylesheet to our page and targeting the CSS class of `.header` in a CSS class.

Recall, the render function of our `Header` component currently looks like this:

```javascript
class Header extends React.Component {
  render() {
    return (
      <div className="header">
        <div className="menuIcon">
          <div className="dashTop"></div>
          <div className="dashBottom"></div>
          <div className="circle"></div>
        </div>

        <span className="title">
          {this.props.title}
        </span>

        <input
          type="text"
          className="searchInput"
          placeholder="Search ..." />

        <div className="fa fa-search searchIcon"></div>
      </div>
    )
  }
}
```


We can target the `header` by defining the styles for a `.header` class in a regular css file. As per-usual, we'll need to make sure we use a `<link />` tag to include the CSS class in our HTML page. Supposing we define our styles in a file called `styles.css` in the same directory as the `index.html` file, this `<link />` tag will look like the following:

```html
<link rel="stylesheet" type="text/css" href="styles.css">
```

Let's fill in the styles for the `Header` class names:

```html
.demo .notificationsFrame .header {
  background: rgba(251, 202, 43, 1);
}
.demo .notificationsFrame .header .searchIcon,
.demo .notificationsFrame .header .title {
  color: #333333;
}

.demo .notificationsFrame .header .menuIcon .dashTop,
.demo .notificationsFrame .header .menuIcon .dashBottom,
.demo .notificationsFrame .header .menuIcon .circle {
  background-color: #333333;
}

```

<div class="demo" id="demo2"></div>

One of the most common complaints about CSS in the first place is the cascading feature itself. The way CSS works is that it _cascades_ (hence the name) parent styles to it's children. This is often a cause for bugs as classes often have common names and it's easy to overwrite class styles for non-specific classes.

Using our example, the class name of `.header` isn't very specific. Not only could the page itself have a header, but content boxes on the page might, articles, even ads we place on the page might have a class name of `.header`.

> One way we can avoid this problem is to use something like [css modules](https://glenmaddern.com/articles/css-modules) to define custom, very unique CSS class names for us.
> There is nothing magical about CSS modules other than it forces our build-tool to define custom CSS class names for us so we can work with less unique names.
> We'll look into using CSS modules a bit later in our workflow.

React provides a not-so-new method for avoiding this problem entirely by allowing us to define styles inline along with our JSX.

## Inline styles

Adding styles to our actual components not only allow us to define the styles inside our components, but allow us to dynamically define styles based upon different states of the app.

React gives us a way to define styles using a JavaScript object rather than a separate CSS file. Let's take our `Header` component one more time and instead of using css classes to define the style, let's move it to inline styles.

Defining styles inside a component is easy using the `style` prop. All DOM elements inside React accept a `style` property, which is expected to be an object with camel-cased keys defining a style name and values which map to their value.

For example, to add a `color` style to a `<div />` element in JSX, this might look like:

```javascript
<div style={{ color: 'blue' }}>
  This text will have the color blue
</div>
```

<div class="demo" id="blueTextDemo"></div>

> Notice that we defined the styles with two braces surrounding it. As we are passing a JavaScript object within a template tag, the inner brace is the JS object and the outer is the template tag.
>
> Another example to possibly make this clearer would be to pass a JavaScript object defined outside of the JSX, i.e.
>
```javascript
render() {
  const divStyle = { color: 'blue' }
  return (<div style={divStyle}>
    This text will have the color blue
  </div>);
}
```

In any case, as these are JS-defined styles, so we can't use just any ole' css style name (as `background-color` would be invalid in JavaScript). Instead, React requires us to camel-case the style name.

> [camelCase](https://en.wikipedia.org/wiki/CamelCase) is writing compound words using a capital letter for every word with a capital letter except for the first word, like `backgroundColor` and `linearGradient`.

To update our header component to use these styles instead of depending on a CSS class definition, we can add the `className` prop along with a `style` prop:

```javascript
class Header extends React.Component {
  render() {
    const wrapperStyle = {
      backgroundColor: "rgba(251, 202, 43, 1)"
    };

    const titleStyle = {
      color: "#111111"
    };

    const menuColor = {
      backgroundColor: "#111111"
    };

    return (
      <div style={wrapperStyle} className="header">
        <div className="menuIcon">
          <div className="dashTop" style={menuColor}></div>
          <div className="dashBottom" style={menuColor}></div>
          <div className="circle" style={menuColor}></div>
        </div>

        <span style={titleStyle} className="title">
          {this.props.title}
        </span>

        <input
          type="text"
          className="searchInput"
          placeholder="Search ..."
        />

        <div style={titleStyle} className="fa fa-search searchIcon"></div>
      </div>
    );
  }
}
```

Our header will be orange again.

<div class="demo" id="demo3"></div>

## Styling libraries

The React community is a pretty vibrant place (which is one of the reasons it is a fantastic library to work with). There are a lot of styling libraries we can use to help us build our styles, such as [Radium](https://formidable.com/open-source/radium/) by Formidable labs.

Most of these libraries are based upon workflows defined by React developers through working with React.

Radium allows us to define common styles outside of the React component itself, it auto-vendor prefixes, supports media queries (like `:hover` and `:active`), simplifies inline styling, and kind of a lot more.

We won't dive into Radium in this post as it's more outside the scope of this series, but knowing other libraries are good to be aware of, especially if you're looking to extend the definitions of your inline styles.

Now that we know how to style our components, we can make some good looking ones without too much trouble. In the next section, we'll get right back to adding user interactivity to our components.

