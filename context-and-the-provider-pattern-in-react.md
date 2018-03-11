# Context and the Provider Pattern in React

---

### Context

Note:

* React’s context is not highly advertised. It is even discouraged to use it.

Sometimes we have to pass down props down the component tree, a several layers

![](/assets/passing_props.png)

This could be cumbersome for each component. However, we can make it better with Context

![](/assets/context.png)

With Context , we do not need to pass down props and C can access context defined in Root Component A .

**Example**:

We define the context as follows:

```
class A extends React.Component {
  getChildContext() {
    return {
      coloredTheme: "green"
    };
  }

  render() {
    return <D />;
  }
}

A.childContextTypes = {
  coloredTheme: PropTypes.string
};
```

Then, we can consume it:

```
class C extends React.Component {
  render() {
    return (
      <div style={{ color: this.context.coloredTheme }}>
        {this.children}
      </div>
    );
  }
}

C.contextTypes = {
  coloredTheme: PropTypes.string
};
```

**Note**:

* Component A did not need to pass down context to it's child component D

### Provider Pattern

For a Provider Pattern, we need a HOC to pass down context, and then  child to access the contexts.

**Provider HOC :**

```
class ThemeProvider extends React.Component {
  getChildContext() {
    return {
      coloredTheme: this.props.coloredTheme
    };
  }

  render() {
    return <div>{this.props.children}</div>;
  }
}

ThemeProvider.childContextTypes = {
  coloredTheme: PropTypes.string
};
```

The Provider component only sets the colored theme from the incoming props. In addition, it only renders its children and doesn’t add anything to the JSX.

After declaring the Provider component, you can use it anywhere in your React component tree. It makes most sense to use it at the top of your component hierarchy to make the context, the colored theme, accessible to everyone.

**Then we use the Provider HOC :**

```
const coloredTheme = "green";
// hardcoded theme
// however, imagine the configuration is located somewhere else
// and would be different for every user of your application
// it would need to be fetched first
// and varies depending on the app user

ReactDOM.render(
  <ThemeProvider coloredTheme={coloredTheme}>
    <App />
  </ThemeProvider>,
  document.getElementById('app')
);
```

Now, every child component can consume the colored theme provided by the`ThemeProvider`component. It doesn’t need to be the direct child, in this case the`App`component, but any component down the component tree.

```
class App extends React.Component {
  render() {
    return (
      <div>
        <Paragraph>
          That's how you would use children in React
        </Paragraph>
      </div>
    );
  }
}

class Paragraph extends React.Component {
  render() {
    const { coloredTheme } = this.context;
    return (
      <p style={{ color: coloredTheme }}>
        {this.props.children}
      </p>
    );
  }
}

Paragraph.contextTypes = {
  coloredTheme: PropTypes.string
};
```

A stateless functional component would have access to React’s context too, but only if `contextTypes`is defined as property of the function.

```
class App extends React.Component {
  render() {
    return (
      <div>
        <Headline>Hello React</Headline>

        <Paragraph>
          That's how you would use children in React
        </Paragraph>
      </div>
    );
  }
}

...

function Headline(props, context) {
  const { coloredTheme } = context;
  return (
    <h1 style={{ color: coloredTheme }}>
      {props.children}
    </h1>
  );
}

Headline.contextTypes = {
  coloredTheme: PropTypes.string
};
```



**Provider Patterns in state management libraries \(e.g. Redux, MobX\): **

We rarely use `this.context`down in the component tree when using a state management library. That's because there is always a HOC that comes with library that does that for us  \(e.g. connect in react-redux library\).

**Here's a contrived example of how the HOC may look like**

```
const getContext = contextTypes => Component => {
  class GetContext extends React.Component {
    render() {
      return <Component { ...this.props } { ...this.context } />
    }
  }

  GetContext.contextTypes = contextTypes;

  return GetContext;
};

// usage

class App extends React.Component {
  render() {
    return (
      <div>
        <Headline>Hello React</Headline>

        <Paragraph>
          That's how you would use children in React
        </Paragraph>

        <SubHeadlineWithContext>
          Children and Context
        </SubHeadlineWithContext>
      </div>
    );
  }
}

...

function SubHeadline(props) {
  return (
    <h2 style={{ color: props.coloredTheme }}>
      {props.children}
    </h2>
  );
}

const contextTypes = {
  coloredTheme: PropTypes.string
};

const SubHeadlineWithContext = getContext(contextTypes)(SubHeadline);
```

Now you could wrap any component into the`getContext`higher order component and expose it to the colored theme property. The property would end up in the props and not in the context object anymore.

  






