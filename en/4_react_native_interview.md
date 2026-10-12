# React Native — Interview

Interview answers in this note are filled.


### Related docs

- [React Native — Interview](./4_react_native_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/).

### React Native Fundamentals

<details>
<summary>What is React Native and how does it differ from React?</summary>

### What is React Native and how does it differ from React?

**Definition:**

React Native extends the award-winning React library, making it possible to build native mobile applications using familiar web technologies.

**How it works:**

Differences from React

- **Platform Scope**: React is tailored for web development, while React Native is exclusive to building iOS and Android applications.
- **Rendering Engine**: React uses the browser's DOM for visualization, whereas React Native achieves a parallel outcome through native platform rendering.
- **Component Style**: While most of the component-building strategies and lifecycles between React and React Native are analogous, the controls manage the considerable difference in rendering and event handling. For instance, React uses simple buttons and divs, whereas React Native leverages platform-compliant components like Button, View, and Text.
- **Integration with APIs**: While React targets web APIs, React Native consolidates connectivity with native mobile device features and APIs. This extension makes it feasible to tap into mechanism such as Camera, GPS, and Fingerprint sensors.

**Examples:**

Code Example: React vs React Native

Here is the React code:

```jsx
import React, { useState } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(count+1)}>
      Clicked {count} times
    </button>
  );
};
```

And here is the equivalent React Native code:

```jsx
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  
  return (
    <View>
      <Button title={`Clicked ${count} times`} onPress={() => setCount(count+1)} />
    </View>
  );
};
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>Can you explain the concept of "Learn once, write anywhere" in the context of React Native?</summary>

### Can you explain the concept of "Learn once, write anywhere" in the context of React Native?

**Definition:**

The main idea of "Learn Once, Write Anywhere" in the context of React Native is "code reusability". Libraries and components written in React can be utilized across multiple platforms, while platform-specific code sections adapt as needed.

This approach offers significant efficiency in both development and maintenance as it ensures consistent behavior across platforms.

**How it works:**

Key Concepts

- **Abstraction**: Developers can focus on business logic without being overly concerned about low-level platform intricacies.
- **Adaptation**: Platform-specific visual and user experience elements can be incorporated when necessary.
- **Reusable Components**: Leveraging shared components across platforms reduces redundancy and simplifies code maintenance.

Core Mechanisms

- **Platform Selectors**: Logical checks based on the running platform, such as Platform.OS === 'ios', provide branching capabilities.

- **Platform-related Directories**: Platform-specific files can be organized into dedicated directories (e.g., ios and android), ensuring distinct settings and behaviors.

- **Platform-specific Extensions**: File naming conventions utilizing platform-specific extensions (e.g., filename.ios.js) enable tailored module imports.

- **Conditional Styles**: Styles imported or applied differently on distinct platforms ensure visual consistency.

Caveats and Best Practices

- **Balance**: Strive for a harmonious blend of shared and platform-specific code, avoiding gridlock due to over customization.

- **Aim at Consistency**: Use platform-agnostic libraries as much as possible to ensure a uniform look and feel.

- **Maintenance Awareness**: Background differences in platform conventions, potential updates, and evolving third-party libraries underscore the need for periodic reviews of platform-specific modules.

**When to use:**

Application Scenarios

- **Authenticators**: Different authentication workflows often exist for iOS and Android. The shared codebase can adapt gracefully using platform-specific file imports and methods.

- **UI/UX Adaptations**: When the look and feel of certain components deviate across platforms, such as navigational elements, shared business logic with platform-specific UI components is key.

- **Push Notifications**: Configurations and handling strategies for push notifications might necessitate platform-specific implementations.

- **Permissions**: The way permissions like location or camera are requested and handled can differ between iOS and Android, requiring adaptation within the common React Native codebase.

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>How do you create a basic React Native application?</summary>

### How do you create a basic React Native application?

**Definition:**

Creating a basic React Native application involves a number of steps, such as setting up your development environment, installing required software, and running a boilerplate application.

**How it works:**

Setting Up Your Environment

First, ensure Node.js and npm are installed on your computer. To check for existing installations:

For Node.js, use node -v or npm -v. If not installed, get the latest version from Node.js website.
For npm, use npm -v.

Setting Up Expo

Expo offers a quicker setup for a lightweight app. However, for large or complex applications, direct use of React Native might be more suitable. To set up Expo, install it globally via npm:

```shell
npm install -g expo-cli
```

Creating Your Project

With Expo (Recommended for Beginners)

- **Choose a Development Tool**: You can select Expo Go, simulator/emulator, or physical device for testing.
- **Create a New Project**: Use the expo init command and choose a template. Common choices include types like "blank," "tabs," and "from existing git repo."
- **Test Your Setup**: Run "hello world" with expo start.

Without Expo

If you're not using Expo, create a new project with npm or Yarn:

With npm:

Looking at Your Project

app.json: This file holds configuration settings.
package.json and yarn.lock (if using Yarn): These files manage project dependencies.
- **node_modules/**: This directory stores your project's dependencies. This directory and its contents should never be pushed to your version control system.

App Entry Point

The default path for the app's entry point and main code is:

For Android: index.js or App.js (generated by Expo)

For iOS: AppDelegate.m and main.m

Directory Structure

The initial setup often includes these directories:

- **android/**: Contains the Android project.
- **ios/**: Contains the iOS project.
- **node_modules/**: Modules fetched and managed via npm or Yarn.
package.json: Lists the app's metadata, as well as its dependencies.

**Examples:**

```shell
npx react-native init MyApp
cd MyApp
npx react-native run-android
# OR
npx react-native run-ios
With Yarn:

npx react-native init MyApp
cd MyApp
yarn android
# OR
yarn ios
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>What are components in React Native?</summary>

### What are components in React Native?

**Definition:**

In React Native, components are building blocks that encapsulate UI and logic, making app development modular and efficient. There are two types of components: Base Components and Custom Components.

**How it works:**

Base Components

These are core UI elements provided by React Native, directly corresponding to native views or controls. They are optimized for performance and interactive consistency.

- **Text**: Displays readable text.
- **View**: A container that supports layout with styles, such as flexbox.
- **Image**: Displays images.

Custom Components

These are created by developers and can be composed of both base and custom components, offering a higher level of abstraction. Custom components are reusable, promote a consistent design, and streamline UI updates.

Text_Example.jsx

Component Nesting and Tree Structure

React Native applications are tree-structured with multiple components nested within one another. This composition allows for consistent and quick alterations across the app. Whether it's a Text, View, or custom component, each is a node in the component tree, visually impacting the app. Devs split the UI into smaller, self-contained parts to simplify maintenance and testing.

Core Principles of Building Components

- **Reusability**: Both base and custom components are designed for reuse in different parts of the application, further expanding the idea of modular development.
- **Autonomy**: Each component should be self-sufficient, not heavily reliant on external data or functionality. This promotes easier maintenance and testing.
- **UI Focus**: Components should either cater to UI or some specific functionality, but never both. This separation ensures a better code structure and maintainability.
- **Loose Prop Types**: Custom components should generally avoid having too many mandatory props to allow for flexibility in their usage. They can, instead, rely on sensible defaults.
- **Integrative Mindset**: When designing components, developers must have a holistic approach, keeping in mind how everything will come together in the UI.

Managing Component State

State management in components revolves around keeping track of changing data within that component. It's common in interactive UIs and involves data binding and conditional rendering. In React Native, components invoke a useState hook to integrate reactive state management.

CustomText with State

**Examples:**

Here is the React Native code:

```jsx
import React from 'react';
import { Text } from 'react-native';

const CustomText = ({ children }) => (
  <Text style={{ fontFamily: 'Roboto-Bold', color: 'darkslategray' }}>{children}</Text>
);

export default CustomText;
```

Here is the modified CustomText component:

```jsx
import React, { useState } from 'react';
import { Text, TouchableOpacity } from 'react-native';

const CustomText = ({ children }) => {
  const [isBold, setIsBold] = useState(false);

  const toggleBold = () => setIsBold(prevState => !prevState);

  return (
    <TouchableOpacity onPress={toggleBold}>
      <Text style={{ fontFamily: isBold ? 'Roboto-Bold' : 'Roboto-Regular', color: 'darkslategray' }}>{children}</Text>
    </TouchableOpacity>
  );
};

export default CustomText;
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>Explain the purpose of the `render()` function in a React Native component.</summary>

### Explain the purpose of the `render()` function in a React Native component.

**Definition:**

The render() function, which is mandatory for all React and React Native components, is a gateway for JSX, receiving, processing, and returning the JSX layout. This function is like a workbench where the developer prepares the visual representation.

**How it works:**

JSX: Visual Blueprint

JSX is HTML-like markup within JavaScript that provides a structured description of the visual layout. It's like a visual blueprint for the component.

The render() function leverages this blueprint, converting the JSX elements into the actual visual UI components.

Code Maintenance

Having the render() function separates declarative structure from actual evaluatives and provides a clear workflow, making the code easier to maintain and understand.

Virtual DOM Interaction

React Native employs a virtual DOM to optimize and streamline UI updates. When the state or props of a component change, render() is called to ensure the virtual DOM is in sync. The virtual DOM then identifies and applies only the necessary updates to the actual UI, reducing redundancy and rendering time.

Performance Optimization: Conditional Rendering

Conditional rendering, controlled by if-else, is facilitated within the render() method, allowing for context-aware UI updates that ensure sensible resource and display utilization.

Side-Effects Handling: Lifecycle Methods

The render() method is just one of several lifecycle methods. Accurate handling of these methods through render() and controlled component updates ensures proper data-fetching and side-effect management.

UI Interactivity: Integrating JSX with Methods

JSX elements link visual representation with the logic behind user interactions—this is powered by methods like onPress, which, again, correspond to changes in state or prop triggers, leading back to, you guessed it, the trustworthy render() function.

**Examples:**

Here's a simple example:

```jsx
// JSX Blueprint
let myJSX = (
  <View>
    <Text>Hello, World!</Text>
  </View>
);

// 'render()' Function
let render = () => {
  let uiComponent = (
    <View>
      <Text>Hello, World!</Text>
    </View>
  );

  // Visual representation
  return uiComponent;
};
```

Code Example: Using JSX and render()

Here is the React Native code:

```python
import React, { Component } from 'react';
import { View, Text, TouchableOpacity, ScrollView } from 'react-native';

class MyComponent extends Component {
  state = {
    data: ['Item 1', 'Item 2', 'Item 3'],
    showList: true,
  };

  renderList() {
    if (!this.state.showList) {
      return null;
    }

    return (
      <ScrollView>
        {this.state.data.map((item, index) => (
          <Text key={index}>{item}</Text>
        ))}
      </ScrollView>
    );
  }

  toggleList = () => {
    this.setState({ showList: !this.state.showList });
  };

  render() {
    return (
      <View>
        <TouchableOpacity onPress={this.toggleList}>
          <Text>{this.state.showList ? 'Hide' : 'Show'} List</Text>
        </TouchableOpacity>
        {this.renderList()}
      </View>
    );
  }
}
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>What is JSX and how is it used in React Native?</summary>

### What is JSX and how is it used in React Native?

**Definition:**

JSX is a syntax extension for JavaScript, especially popular in React and React Native for expressing your UI components concisely.

It effectively lets you write XML-style code directly in your JavaScript files, making component definition and nesting visually intuitive.

**How it works:**

Key JSX Features in React Native

- **Component Definition**: Use .JSX to visually group styling and component structure.
- **Component Nesting**: Implement parent-child relationships via clear indentation in your JSX tree, enhancing visual hierarchy.
- **Event Handling**: Attach event listeners to UI events easily within the JSX structure.

JSX Transpiling

The Babel transpiler lies at the heart of JSX functionality, converting JSX into regular JavaScript for compatibility with web and mobile platforms.

Babel transpiles it into the following JavaScript:

return React.createElement(View, { style: styles.container }, React.createElement(Text, null, "Hello, React Native!"));

**Examples:**

```jsx
const MyComponent = () => {
  return (
    <View style={styles.container}>
      <Text>Hello, React Native!</Text>
    </View>
  );
};
```

Regular JS (No JSX):

```jsx
const MyComponent = () => {
  return React.createElement(View, { style: styles.container }, React.createElement(Text, null, "Hello, React Native!"));
};
```

```jsx
return (
  <View>
    <Text>Parent</Text>
    <View>
      <Text>Child 1</Text>
      <Text>Child 2</Text>
    </View>
  </View>
);
```

```jsx
<TouchableOpacity onPress={handlePress}>
  <Text style={styles.buttonText}>Press Me</Text>
</TouchableOpacity>
Equivalent Without JSX:
React.createElement(TouchableOpacity, { onPress: handlePress }, React.createElement(Text, { style: styles.buttonText }, "Press Me"));
```

For instance, when you author this React Native code:

```jsx
return <View style={styles.container}><Text>Hello, React Native!</Text></View>;
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>Can you list some of the core components in React Native?</summary>

### Can you list some of the core components in React Native?

**Definition:**

React Native has several fundamental components.

**How it works:**

Core Components

- **View**: The basic container that supports layout with Flexbox.
- **Text**: For displaying text.
- **Image**: For displaying images either from the local file system or the network.

Specialized Components

- **ScrollView**: For displaying a scrollable list of components.
Listview (deprecated): A high-performance, cross-platform list view.
- **TextInput**: An input component with optional prompts, as well as a variety of keyboard types, enabling text input.

User Interface

- **Button**: A UI component that enables a user to interact with the application.
- **Picker**: A dropdown list that displays a picker interface.

Basic Functionality Components

- **ActivityIndicator**: Displays a rotating circle, indicating that the app is busy performing an operation.
- **Slider**: Lets the user select a value by sliding the thumb on the bar.
- **Switch**: Used for the on/off state.
iOS and Android Platform Integration
- **SegmentedControlIOS**: Renders a UISegmentedControl on iOS.
- **TabBarIOS**: Renders a tab bar with tabs that can be swiped.
TabBarIOS.Item: Represents an item in a TabBarIOS component.
- **ToolbarAndroid**: A toolbar for use with the CoordinatorLayout, offering additional features, such as controls for children of the Views.
- **ToolbarAndroid**: Represents a standard Android toolbar.

List Views

- **FlatList**: A core virtualized list component supporting both vertical and horizontal scrolls. It's memory-efficient and only renders the elements on-screen. It also supports dynamic loading.
- **SectionList**: Much like FlatList, but also allows you to section your data.

Other Components

- **ActionSheetIOS**: Provides a pre-designed action sheet to display the list of options.
- **Alert**: For displaying an alert dialog.
- **AncestryManager**: Manages the relationships of Views. Developed initially for aiding the animation framework.
- **Animated**: A consolidated toolset to perform animations.
- **Appearance**: Gains insights into the unique appearance traits of respective iOS components if the device is running on iOS 13 or later.
- **DevSettings**: Owns a group of system settings tailored to the development phase.
- **Dimensions**: Assists in determining the dimensions of the viewport.
- **DormantStack**: Regulates the arrangement of Views. These views remain dormant and stop updating when they are not visible.
- **DrawerLayoutAndroid**: A unique drawer layout design developed for Android.
- **InputAccessoryView**: Accommodates customized views for input access.
- **KeyboardAvoidingView**: Augments the scalability of the UI in circumstances like keyboard being opened.
- **MaskedViewIOS**: Allows developers to create custom views with a specified shape.
- **Picker**: A seeming replica of the Picker that shows a native picker interface after a click.
- **Platform**: A utility to single out the present platform.
- **ProgressViewIOS**: For when one needs to display a standard iOS progress bar.
- **RefreshControl**: A tool used to integrate swipe-down-the-screen functionalities.
- **SafeAreaView**: A safety net component for iOS, regulating the area displays that would have compromised otherwise.
- **StatusBar**: A utility to handle the app's status bar control.
- **StyleSheet**: A collection of type-safe approaches to the integration of styles in your app.
- **TouchableHighlight**: A view that illustrates a sub-view while being tapped.
- **TouchableNativeFeedback**: An optimized version of the TouchableHighlight toward Android platforms.
- **TouchableOpacity**: Reduces the opacity of the view for conveying the touch response.
- **TouchableWithoutFeedback**: A view reacting to touches and signals exclusively without providing any feedback.
- **Vibration**: Triggers the device's vibration mechanism.
- **ViewPropTypes**: Manages the prop-types for the View component.

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>How do you handle state management in React Native?</summary>

### How do you handle state management in React Native?

**Definition:**

State management in React Native involves tracking and updating the state of components.

**How it works:**

Local State Management

- **State Declaration**: Use useState from the React library.

- **State Update**: Invoke the state-modifying function (in this case, setCount).

Global State Management

- **State Declaration**: Utilize the "Context" API via Provider to make the state globally available.

- **Selecting**: Components can subscribe to specific parts of the global state using useContext.

- **Updating**: To modify global state, make use of a "reducer" or actions that the dispatch function sends to the state management system.

External State Management

- **State Declaration**: Use external libraries like Redux.

- **Selecting**: Components can subscribe to parts of the global state via connect or useSelector.

- **Updating**: Trigger changes using dispatch or action creators.

Shared State Management

- **State Declaration**: Also facilitated by the "Context" API or libraries like Redux.

- **Updating**: Components interact with the shared state as per the framework's methodology, be it by using dispatch in Redux or other means.

- **Keeping Components in Sync**: This is automatically handled by the framework.

Best Practices

- **Granularity of State**: Prefer local state whenever state is limited to a single component. Global and external state management should be reserved for state shared across multiple components.

- **Consistency**: Adhere to a consistent state management approach throughout the application to minimize complexity.

- **Minimizing Storage**: Avoid duplicative storage of state where local state can serve the purpose.

**Examples:**

```jsx
import React, { useState } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
};
```

```jsx
import { useReducer, useContext } from 'react';

// Define an action
const INCREMENT = 'increment';

// Define a reducer
const reducer = (state, action) => {
  switch (action.type) {
    case INCREMENT:
      return { ...state, count: state.count + 1 };
    default:
      return state;
  }
};

// Within component
const MyComponent = () => {
  const { state, dispatch } = useContext(MyContext);
  dispatch({ type: INCREMENT });
};
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>What are props in React Native and how are they used?</summary>

### What are props in React Native and how are they used?

**Definition:**

Props (short for "properties") enable unidirectional data flow in React and are essential for building reusable React Native components.

Props provide a mechanism for passing data from a parent to a child, allowing for customization and dynamic behavior. They are immutable and help to keep components self-contained, making it easier to manage and maintain a React Native application.

**How it works:**

Key Concepts

- **Unidirectional Data Flow**: Props serve as a one-way street for data, originating in a parent component and flowing down to child components.
- **Read-Only**: Once defined, props are not meant to be modified by the receiving component.
- **Default Values**: You can define default values for props to ensure smooth handling.

Guiding Principles

- **Single Source of Truth**: Emphasizes a centralized role for data, making it clearer to identify the information's origination point.
- **Separation of Concerns**: By restricting the scope in which components can influence one another, application logic becomes more compartmentalized and easier to manage.

Prop Types

- **Required Props**: Ensures that specific props are provided. If a required prop is missing, React will issue a warning (in development mode).
- **Data Types**: Prop types can be specialized (e.g., string, number, or function) to enforce type coherency, contributing to better code reliability and predictability.
- **Shape and Arrays**: For more complex data structures, such as objects with specific shapes and arrays, you can define even more intricate prop type requirements.
- **In the above example, WeatherCard accepts three props**: temperature, humidity, and icon. Each of these props is essential for the card to accurately depict current weather conditions. Each prop has a designated prop type, ensuring that the received values match specific criteria.

**Examples:**

Code Example: Simple "Weather Card" Component

```jsx
// WeatherCard.js
import React from 'react';
import PropTypes from 'prop-types';
import { Text, View, Image } from 'react-native';

const WeatherCard = ({ temperature, humidity, icon }) => {
  return (
    <View>
      <Text>{`Temperature: ${temperature}`}</Text>
      <Text>{`Humidity: ${humidity}`}</Text>
      <Image source={icon} />
    </View>
  );
};

WeatherCard.propTypes = {
  temperature: PropTypes.number.isRequired,
  humidity: PropTypes.number.isRequired,
  icon: PropTypes.shape({
    uri: PropTypes.string,
  }).isRequired,
};

export default WeatherCard;
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>What is the significance of the Flexbox layout in React Native?</summary>

### What is the significance of the Flexbox layout in React Native?

**Definition:**

Let's look at the unique implementations and advantages of Flexbox within React Native.

**How it works:**

Components Designed for Flexbox

React Native provides specific components empowered by Flexbox, including:

- **Container Components**: These are Views and Touchables that house inner elements being placed using Flexbox.

- **Content Components**: These are core layout components that handle the arrangement of inner items. Examples include the Text component.

Core Flexbox Components

- **View**: The foundation of Flexbox layout in React Native

- **Text**: A specialized View primarily for text-related elements that supports Flexbox

Image: A Flexbox-capable View for image elements

- **ScrollView**: A container for components that are larger than its size, offering various scrolling methods. It uses Flexbox to arrange these scrollable components.

- **FlatList and SectionList**: These specialized components efficiently render large lists and areas as per the screen's dimensions, leveraging the innate performance of the native interface.

- **VirtualizedList**: A low-level, high-performance list. Both FlatList and SectionList are built on top of VirtualizedList.

Flexbox Properties in React Native

- **Direction**: Establishes the principal axis of the layout. Options are row and column, with the latter being the default.

- **Alignment**: Determines the position of items along the secondary axis. Common settings include flex-start, center, and flex-end. The setting stretch is also available, which extends the components to fill the empty space.

- **Order**: Flexbox allows for reordering of elements. This property defines the display order, with the default being 0.

- **Proportional Sizing**: Rather than specifying explicit dimensions, items can be sized proportionally to the remaining space, making the layout adaptable to various screen sizes.

- **Gutters**: Flexbox in React Native can handle gutters between items effortlessly.

In this example, the three boxes are displayed side by side due to flexDirection: 'row'. Their alignment is managed by justifyContent and alignItems.

React Native primarily focuses on the core concepts and properties of Flexbox, offering a simplified and intuitive experience for developers.

**Examples:**

Code Example: Flex Direction

Here is the JavaScript code:

```java
export default function App() {
  return (
    <View style={styles.container}>
      <View style={styles.box} />
      <View style={styles.box} />
      <View style={styles.box} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'row',
    justifyContent: 'space-evenly',
    alignItems: 'center',
  },
  box: {
    width: 50,
    height: 50,
    backgroundColor: 'lightgrey',
  },
});
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>How do you debug a React Native application?</summary>

### How do you debug a React Native application?

**Definition:**

Debugging in React Native involves both traditional debugging methods and those specific to the platform. Here are the strategies and tools to streamline the debugging process.

**How it works:**

Debugging Strategies

- **Use DevTools**: These browser-based tools let you inspect and modify the DOM, examine network activity, and monitor console logs, among other features.

- **Leverage Emulators/Simulators**: Virtual device environments can be invaluable for replicating and debugging issues across a range of device types.

- **Physical Testing**: Testing on actual devices is vital for identifying device-specific bugs. Use USB debugging for a direct connection to the development machine.

- **Watch Out for Undefined Behaviors**: JavaScript doesn't throw errors when accessing properties or methods of null or undefined. Check for unexpected undefined values using defensive patterns, like short-circuit evaluation or optional chaining.

- **Separate and Simplify Issues**: If you're dealing with a complex issue, break it down into bite-sized problems. Simplify the problem domain to understand the root cause. Use binary search principles to isolate issues methodically.

- **Test Isolation**: Confirm whether the bug is reproducible in a fresh environment. Temporarily remove certain components or functionality to see how it affects the bug.

- **Incremental Building**: After making a change, observe if the problem resolves or worsens.

- **Versatile Console Logging**: Consider using console.assert for conditional logs, console.info for informational messages, console.warn for potential issues, and console.error for errors.

- **Component-Specific Debugging**: Use ReactDOM.findDOMNode to fetch a component's DOM representation.

- **Debug in Multiple Environments**: It might be helpful to test your app in both dev and production modes.

Tools for Debugging

- **React DevTools**: Browser extensions like Chrome DevTools or standalone applications facilitate inspection of React component trees, context, and hooks.

- **Flipper**: This is a debugging tool for mobile apps on iOS, Android, and React Native. It offers a pluggable architecture that allows developers to build custom integrations.

- **Expo**: The expo-dev-client package provides a powerful testing environment. It's useful when working in managed Expo projects.

- **Redux DevTools**: For an app integrated with Redux, this tool offers essential features such as time-travel debugging.

- **Network Inspectors**: Both Chrome DevTools and Safari's Web Inspector offer detailed network activity insights.

- **Profiling and Performance Tools**: Use React DevTools for profiling and diving deep into performance metrics.

- **Remote Debugging**: Chrome and Safari offer remote debugging tools. For example, with Chrome, you can inspect the WebView used by your app.

- **React Native Debugger**: This application bundles Chrome DevTools, the React DevTools, and a JavaScript VM.

- **Code Linters**: Integrate linters like ESLint or TypeScript with your editor or development environment to catch potential source code issues.

HMR (Hot Module Replacement): HMR ensures that your app remains in the current state whenever you make changes, reducing the need for constant refreshes.

- **Third-Party Services**: Incorporate third-party services for bug tracking, crash reporting, and in-app user feedback.

Continuous Integration and Continuous Deployment (CI/CD) Tools: Platforms like Bitrise or CircleCI offer extensive pipelines for automation, testing, and deployment.

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>Explain the concept of hot reloading in React Native.</summary>

### Explain the concept of hot reloading in React Native.

**Definition:**

Hot Reloading innovatively speeds up the development process by rendering updates to the running app in real-time. While preserving the app's current state, it allows developers to observe changes in layout, live.

**How it works:**

Key Benefits

- **Enhanced Productivity**: Avoiding repetitive recompilations and reinstalls.
- **Quick Updates**: View real-time changes with contextual data.
- **Error Localization**: Identify issues when and where they occur.

Mechanism

- **State Retention**: Unlike a full reload, hot reloading keeps the app's state intact.
- **Partial Update**: Only files that have been changed are updated, reducing the time needed.

Code Example: Enabling Hot Reloading

In App.js, activate hot reloading with a few simple changes:

if (__DEV__) {

**Examples:**

```text
    const { activateKeepAwake } = require('expo-keep-awake');
    activateKeepAwake();
}
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>How do you handle user input in React Native?</summary>

### How do you handle user input in React Native?

**Definition:**

React Native comprises several components tailored to user interaction and input processing for both iOS and Android platforms.

**How it works:**

Core Input Components

Text Input

The most basic input component, TextInput, caters to single- and multi-line text input needs. It provides options for simple text, password fields, and more.

Example: Basic TextInput

Input Fields with Auto-Complete

TextInput leverages autoCompleteType to blend with both platforms' keyboard suggestions.

If a user has previously entered data in the same field, the keyboard might suggest this data. On some platforms, this will include email address suggestions.

Choose one of the following:

- **off**: No auto-completion.
- **username**: Auto-completion for usernames.
- **password**: Secure auto-completion for passwords.
- **email**: Auto-completion for email addresses.
/>

Discrete options with Picker

For small sets of options, Picker macOS. Picker is the component that iOS makes available that allows for single option selection (much like a dropdown).

- **selectedValue**: The currently selected value.
- **onValueChange**: The event triggered when a different value is selected.
>
>

Text Areas for Multi-Line Input

TextArea offers a multi-line text input field.

**Examples:**

Here is the React Native code:

```jsx
<TextInput placeholder="Enter your name" />
```

For example:

```jsx
<TextInput
  placeholder="Enter your email"
  autoCompleteType="email"
```

The iOS code is as follows:

```jsx
<Picker
  selectedValue={selectedValue}
  onValueChange={(itemValue, itemIndex) =>
    setSelectedValue(itemValue)
  }
```

```java
  <Picker.Item label="Java" value="java" />
  <Picker.Item label="JavaScript" value="js" />
</Picker>
```

The android code is like:

```jsx
<Picker
  selectedValue={selectedValue}
  onValueChange={(itemValue, itemIndex) =>
    setSelectedValue(itemValue)
  }
```

```java
  <Picker.Item label="Java" value="java" />
  <Picker.Item label="JavaScript" value="js" />
</Picker>
```

Here is the React Native code:

```jsx
<TextArea placeholder="Type your message" />
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>What is a `TouchableHighlight` in React Native?</summary>

### What is a `TouchableHighlight` in React Native?

**Definition:**

TouchableHighlight is a React Native component optimized for touch interactions. It uses the platform's native feedback effect when touched, making it ideal for buttons, tabs, or other interactive elements.

**How it works:**

Key Features

- **Accessibility**: It automatically handles accessibility states such as focus and press, adhering to WAI-ARIA and native mobile accessibility guidelines.

- **Visual Feedback**: Upon touch, it provides a visual indication like opacity changes or highlighting, depending on the platform.

- **On Press Event**: Executes a function when the component is pressed or activated using keyboard or assistive devices.

>

**Examples:**

Code Example: TouchableHighlight

Here is the React Native code:

```jsx
import { TouchableHighlight, Text, View } from 'react-native';

const CustomButton = ({ label }) => (
  <TouchableHighlight
    style={{ backgroundColor: 'green', padding: 10, margin: 10, borderRadius: 5 }}
    underlayColor="lime"
    onPress={() => alert('Button pressed!')}
```

```jsx
    <Text style={{ color: 'white' }}>{label}</Text>
  </TouchableHighlight>
);

const App = () => (
  <View>
    <CustomButton label="Press me!" />
  </View>
);
```

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>Describe the View component and its purpose in React Native.</summary>

### Describe the View component and its purpose in React Native.

**Definition:**

View in React Native functions similarly to a <div> in web development and allows for style definitions.

**How it works:**

Core Benefits

- **Layout Flexibility**: Optimizes for different screen sizes and orientations.
- **Style Configuration**: Streamlines style organization and replication.

Common Misconceptions

1. View Doesn't Represent Visual Elements
While View forms the basic unit for layout and styling, it doesn't intrinsically display any content, nor is it scrollable. Within View, you incorporate Text and other components for visual representation.

2. View Doesn't Guarantee a Unique End-Component
Despite being fundamental for rendering, View might not directly translate to a distinct UI component. The internal view system structures UI arrangements. Accessibility and debug tools in React Native App recognize and isolate View components. This distinction is crucial for device adaptation and adopting universal design principles.

Best Practices

- **Organized Nesting**: Maintain clarity by thoughtfully nesting components.
- **Styling Economics**: Leverage shared styles to minimize redundancy and ensure a consistent look throughout the app.
- **Accessibility**: Maintain good accessibility by including visual aid for differently-abled users.
- **Refactoring**: Regularly reevaluate your design choices and refactor as needed.

**When to use:**

Primary Use-Cases

- **Structural Containers**: Serve as building blocks for organizing UI elements.

- **Standalone Components**: When you need an element that can be styled or requires touch interaction.

- **Adaptive Design**: Design dynamic layouts that accommodate various device types and orientations.

- **Styling**: Apply shared styles to multiple child components.

**Sources:**

- [Devinterview.io — React Native](https://devinterview.io/questions/web-and-mobile-development/react-native-interview-questions/)
</details>

<details>
<summary>How would you navigate between screens in a React Native app?</summary>

### How would you navigate between screens in a React Native app?

**Definition:**

React Native does not ship a navigator. Screen transitions are handled by a navigator library. The React Native docs recommend React Navigation for new apps, and `react-native-navigation` when navigation is already owned by native code.

**How it works:**

Install `@react-navigation/native` and `@react-navigation/native-stack`, plus `react-native-screens` and `react-native-safe-area-context`. `createNativeStackNavigator` registers screens. `createStaticNavigation` renders that tree. `useNavigation().navigate(name, params)` pushes a screen. A native stack uses `UINavigationController` on iOS and a `Fragment` on Android, so the transition runs on the native UI thread.

**When to use:**

Use React Navigation for a JavaScript-defined stack, tabs, or drawer. Use `react-native-navigation` when an existing native app already owns the navigation stack.

**Examples:**

```tsx
import {createStaticNavigation, useNavigation} from '@react-navigation/native';
import {createNativeStackNavigator} from '@react-navigation/native-stack';

function HomeScreen() {
  const navigation = useNavigation();
  return (
    <Button
      title="Open profile"
      onPress={() => navigation.navigate('Profile', {name: 'Jane'})}
    />
  );
}

const RootStack = createNativeStackNavigator({
  screens: {
    Home: {screen: HomeScreen, options: {title: 'Welcome'}},
    Profile: {screen: ProfileScreen},
  },
});

export default createStaticNavigation(RootStack);
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
</details>

<details>
<summary>Can you explain the use of the useState hook in React Native?</summary>

### Can you explain the use of the useState hook in React Native?

**Definition:**

`useState` keeps a value that belongs to one function component and asks React to render that component again when the value changes. React Native screens use the same React hook as the web.

**How it works:**

The call returns the current value and a setter. The setter should receive the next value or an updater function. Do not mutate the previous object or array in place. State updates from native event handlers are batched under the New Architecture's React 18 renderer.

**Examples:**

```tsx
import {useState} from 'react';
import {Button, Text, View} from 'react-native';

export function Counter() {
  const [count, setCount] = useState(0);
  return (
    <View>
      <Text>{count}</Text>
      <Button title="Add" onPress={() => setCount(value => value + 1)} />
    </View>
  );
}
```

**Sources:**

- [State](https://reactnative.dev/docs/state)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
</details>

<details>
<summary>How do you apply styles in React Native?</summary>

### How do you apply styles in React Native?

**Definition:**

Styles are plain JavaScript objects passed to the `style` prop. There is no CSS file and no CSS cascade. Yoga lays the views out with Flexbox.

**How it works:**

`StyleSheet.create` names those objects and type-checks them against `ViewStyle`, `TextStyle`, and `ImageStyle`. An array of styles merges left to right; the later object wins. `StyleSheet.compose` does the same merge and skips allocating an array when one side is empty. `StyleSheet.flatten` collapses an array into one object.

**Examples:**

```tsx
import {StyleSheet, Text, View} from 'react-native';

export function LotsOfStyles() {
  return (
    <View style={styles.container}>
      <Text style={[styles.bigBlue, styles.red]}>big, then red</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {marginTop: 50},
  bigBlue: {color: 'blue', fontWeight: 'bold', fontSize: 30},
  red: {color: 'red'},
});
```

**Sources:**

- [Style](https://reactnative.dev/docs/style)
- [StyleSheet](https://reactnative.dev/docs/stylesheet)
</details>

<details>
<summary>What is the difference between `state` and `props` in React Native?</summary>

### What is the difference between `state` and `props` in React Native?

**Definition:**

Props are inputs a parent passes in. State is data the component owns and can change. Both are React concepts used unchanged in React Native.

**How it works:**

A render reads props and state to describe the next native view tree. The parent owns props, so the child treats them as read-only. Calling the `useState` setter replaces state and schedules another render of that component. Props change only when the parent renders with new values.

**Examples:**

```tsx
function Greeting({name}: {name: string}) {
  const [taps, setTaps] = useState(0);
  return (
    <Button
      title={`${name} tapped ${taps}`}
      onPress={() => setTaps(taps + 1)}
    />
  );
}
```

**Sources:**

- [Props](https://reactnative.dev/docs/props)
- [State](https://reactnative.dev/docs/state)
</details>

<details>
<summary>How do you fetch and render a list of data using React Native?</summary>

### How do you fetch and render a list of data using React Native?

**Definition:**

Load JSON with the built-in `fetch` API, keep the result in state, and render it with `FlatList`. `FlatList` virtualizes rows so off-screen items are not all mounted.

**How it works:**

`fetch` returns a promise. Parse `response.json()`, then `setState` the array. `FlatList` needs `data` and `renderItem`. By default it uses each item's `key` field; otherwise pass `keyExtractor`. It is a `PureComponent`: if `renderItem` depends on state outside `data`, pass that value as `extraData`.

**Examples:**

```tsx
const [movies, setMovies] = useState<Movie[]>([]);

useEffect(() => {
  fetch('https://reactnative.dev/movies.json')
    .then(response => response.json())
    .then(json => setMovies(json.movies))
    .catch(console.error);
}, []);

<FlatList
  data={movies}
  keyExtractor={item => item.id}
  renderItem={({item}) => <Text>{item.title}</Text>}
/>
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [FlatList](https://reactnative.dev/docs/flatlist)
- [Using a List View](https://reactnative.dev/docs/using-a-listview)
</details>

### React Native Intermediate Concepts

<details>
<summary>What are the lifecycle methods of a React Native component?</summary>

### What are the lifecycle methods of a React Native component?

**Definition:**

Class components still have React's mount, update, and unmount methods. Current React Native code uses function components, so the same moments are expressed with hooks.

**How it works:**

`constructor` and `render` run while creating the class instance. `componentDidMount` runs after the first commit, which is where a class component starts a request. `componentDidUpdate` runs after later commits. `componentWillUnmount` runs before the native view is removed and is the place to cancel work. In a function component, `useEffect` covers mount, update, and cleanup. The New Architecture also lets `useLayoutEffect` read layout in the same commit.

**Examples:**

```tsx
useEffect(() => {
  const task = loadProfile();
  return () => task.cancel();
}, []);
```

**Sources:**

- [State](https://reactnative.dev/docs/state)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
</details>

<details>
<summary>Explain the use of the useEffect hook in React Native.</summary>

### Explain the use of the useEffect hook in React Native.

**Definition:**

`useEffect` runs a side effect after React commits the render. In React Native that is where you fetch data, subscribe to `AppState` or `Linking`, and start or stop timers.

**How it works:**

The effect runs after paint. The optional cleanup function runs before the next effect and on unmount. The dependency array lists values the effect reads. An empty array means “after the first commit.” Fast Refresh re-runs effects even when the dependency list is empty, so the effect must be safe to run again.

**Examples:**

```tsx
useEffect(() => {
  const subscription = AppState.addEventListener('change', setAppState);
  return () => subscription.remove();
}, []);
```

**Sources:**

- [AppState](https://reactnative.dev/docs/appstate)
- [Fast Refresh](https://reactnative.dev/docs/fast-refresh)
</details>

<details>
<summary>What are higher-order components in React Native?</summary>

### What are higher-order components in React Native?

**Definition:**

A higher-order component is a function that takes a component and returns a new component with extra behavior. React Native uses the same React pattern.

**How it works:**

The wrapper renders the inner component and passes through props, adding data or handlers. Fast Refresh resets state when the file exports the result of a higher-order call whose returned component is a class. New code usually replaces a wrapper with a custom hook, which keeps the component tree easier to trace.

**When to use:**

Use a higher-order component when many screens must share the same wrapper. Prefer a hook when the extra behavior is just data or a function.

**Examples:**

```tsx
function withOnline<P extends object>(Screen: React.ComponentType<P>) {
  return function OnlineScreen(props: P) {
    const online = useOnline();
    return <Screen {...props} online={online} />;
  };
}
```

**Sources:**

- [Fast Refresh](https://reactnative.dev/docs/fast-refresh)
- [Introducing React Native](https://reactnative.dev/docs/intro-react)
</details>

<details>
<summary>How do you use the Context API in React Native?</summary>

### How do you use the Context API in React Native?

**Definition:**

Context sends a value to any descendant without passing it through every intermediate component. React Native uses React context for theme, locale, and signed-in user.

**How it works:**

`createContext` defines the default. A provider component sets the current value. `useContext` reads the nearest provider. When the provider value changes, consumers render again. Keep the value stable: a new object on every parent render makes every consumer render too.

**Examples:**

```tsx
const ThemeContext = createContext<'light' | 'dark'>('light');

export function App() {
  return (
    <ThemeContext.Provider value="dark">
      <HomeScreen />
    </ThemeContext.Provider>
  );
}

function HomeScreen() {
  const theme = useContext(ThemeContext);
  return <Text>{theme}</Text>;
}
```

**Sources:**

- [Introducing React Native](https://reactnative.dev/docs/intro-react)
- [Appearance](https://reactnative.dev/docs/appearance)
</details>

<details>
<summary>Can you perform deep linking in React Native? If so, how?</summary>

### Can you perform deep linking in React Native? If so, how?

**Definition:**

Yes. The `Linking` module reads the URL that opened the app and listens for URLs while the app is open. A deep link looks like `app://products/1`.

**How it works:**

`Linking.getInitialURL()` returns the URL used at launch, or `null`. `Linking.addEventListener('url', handler)` receives links after launch. Parse the path and then navigate. Custom schemes are not a secure channel: another app can register the same scheme. Do not put tokens in the link. On iOS, universal links are the documented secure alternative.

**Examples:**

```tsx
const url = await Linking.getInitialURL();
const subscription = Linking.addEventListener('url', ({url}) => {
  openPath(url);
});
return () => subscription.remove();
```

**Sources:**

- [Linking](https://reactnative.dev/docs/linking)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>Explain the significance of keys in a list in React Native.</summary>

### Explain the significance of keys in a list in React Native.

**Definition:**

A key tells React which row is which across renders. `FlatList` and `SectionList` use it so a row keeps its identity when the data array changes.

**How it works:**

By default the list looks for a `key` field on each item. `keyExtractor` supplies another stable id. Index keys break when rows are inserted, removed, or reordered: React reuses the wrong child. Keys must be unique among siblings and stable for the lifetime of that item.

**Examples:**

```tsx
<FlatList
  data={users}
  keyExtractor={user => user.id}
  renderItem={({item}) => <Text>{item.name}</Text>}
/>
```

**Sources:**

- [FlatList](https://reactnative.dev/docs/flatlist)
- [VirtualizedList](https://reactnative.dev/docs/virtualizedlist)
</details>

<details>
<summary>How do you implement global state in a React Native app without using Redux or Context API?</summary>

### How do you implement global state in a React Native app without using Redux or Context API?

**Definition:**

A module-level store can hold app state outside the React tree. Components subscribe to it. The testing guide names this separation explicitly and lists Redux and MobX as examples, not as a requirement.

**How it works:**

Keep the store in its own module. Expose `getState`, `setState`, and `subscribe`. A small hook subscribes in `useEffect` and copies the slice into `useState`, so a write still causes a render. Persist only non-sensitive slices. The security guide says Async Storage is for non-sensitive data such as persisted state, not tokens.

**When to use:**

Use a module store when a few distant screens share one value and you do not want a provider. Use context when the value should follow a subtree.

**Examples:**

```tsx
type Listener = () => void;
let session: string | null = null;
const listeners = new Set<Listener>();

export function setSession(next: string | null) {
  session = next;
  listeners.forEach(listener => listener());
}

export function useSession() {
  const [value, setValue] = useState(session);
  useEffect(() => {
    const listener = () => setValue(session);
    listeners.add(listener);
    return () => listeners.delete(listener);
  }, []);
  return value;
}
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>How do you optimize performance in a React Native app?</summary>

### How do you optimize performance in a React Native app?

**Definition:**

A phone has about 16.67 ms to produce each of 60 frames. Dropped frames come from the JavaScript thread or the native UI thread. Measure a release build, then remove the specific stall.

**How it works:**

Dev mode and `console.log` slow the JavaScript thread, so profile a release bundle and strip `console.*` with `babel-plugin-transform-remove-console`. For long lists, implement `getItemLayout` so `FlatList` skips measuring rows. Keep animations off the JavaScript thread with `useNativeDriver: true` or `LayoutAnimation`. Prefer `transform: [{scale}]` over changing an image's width and height. Turn on `renderToHardwareTextureAndroid` only while a view is moving.

**Examples:**

```tsx
Animated.timing(opacity, {
  toValue: 1,
  duration: 200,
  useNativeDriver: true,
}).start();
```

**Sources:**

- [Performance](https://reactnative.dev/docs/performance)
- [Optimizing FlatList Configuration](https://reactnative.dev/docs/optimizing-flatlist-configuration)
</details>

<details>
<summary>What is the Virtual DOM and how does React Native utilize it?</summary>

### What is the Virtual DOM and how does React Native utilize it?

**Definition:**

React Native does not use the browser DOM. It uses React's element tree and reconciler, then commits host views such as `View` and `Text` through the native renderer.

**How it works:**

React describes the UI as elements. The reconciler compares the next tree with the previous one and updates the host components that changed. In the legacy architecture those updates crossed an asynchronous bridge that serialized messages. The New Architecture replaces that bridge with JSI: JavaScript holds a reference to a C++ object and calls it directly. Fabric is the renderer. Since React Native 0.76 the New Architecture is on by default and supports React's concurrent renderer, including automatic batching.

**Examples:**

```tsx
return (
  <View>
    <Text>{title}</Text>
  </View>
);
```

**Sources:**

- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
- [Performance](https://reactnative.dev/docs/performance)
</details>

<details>
<summary>Describe how to implement animations in React Native.</summary>

### Describe how to implement animations in React Native.

**Definition:**

The built-in `Animated` API drives values over time. The performance guide also documents `LayoutAnimation` for fire-and-forget layout changes.

**How it works:**

`Animated.timing`, `spring`, and `decay` update an `Animated.Value`. `useNativeDriver: true` runs the animation on the native UI thread, but only for properties the native driver supports, such as `transform` and `opacity`. Without it, each frame is calculated on the JavaScript thread and freezes if that thread is busy. `LayoutAnimation` uses Core Animation and is not interrupted by JavaScript frame drops. It is only for animations that are not interruptible.

**When to use:**

Use `Animated` with the native driver for opacity and transforms that the user might interrupt. Use `LayoutAnimation` for a one-shot layout change.

**Examples:**

```tsx
const fade = useRef(new Animated.Value(0)).current;
Animated.timing(fade, {toValue: 1, duration: 250, useNativeDriver: true}).start();
return <Animated.View style={{opacity: fade}} />;
```

**Sources:**

- [Animated](https://reactnative.dev/docs/animated)
- [Performance](https://reactnative.dev/docs/performance)
</details>

<details>
<summary>How does React Native handle layout for different device sizes and orientations?</summary>

### How does React Native handle layout for different device sizes and orientations?

**Definition:**

Flexbox stretches and shrinks children inside the parent instead of hard-coding every screen size. `useWindowDimensions` reports the current window so a layout can react when the window changes.

**How it works:**

Defaults differ from CSS on the web: `flexDirection` is `column`, `alignContent` is `flex-start`, `flexShrink` is `0`, and `flex` takes one number. `useWindowDimensions` subscribes to window changes, including rotation. `FlatList` will not re-render on a dimension change unless the new size is passed through `extraData`, because the list is a `PureComponent`. `PixelRatio` and `SafeAreaView` handle density and notches.

**Examples:**

```tsx
const {width, height} = useWindowDimensions();
const landscape = width > height;
return <View style={{flexDirection: landscape ? 'row' : 'column', flex: 1}} />;
```

**Sources:**

- [Flexbox](https://reactnative.dev/docs/flexbox)
- [useWindowDimensions](https://reactnative.dev/docs/usewindowdimensions)
- [FlatList](https://reactnative.dev/docs/flatlist)
</details>

<details>
<summary>What is the difference between `StyleSheet.create` and plain JavaScript objects for styling?</summary>

### What is the difference between `StyleSheet.create` and plain JavaScript objects for styling?

**Definition:**

`StyleSheet.create` is an identity function. The current docs say its practical benefit is static type checking against native style properties. A plain object is also a valid `style` value.

**How it works:**

Both end up as style objects on the host view. `StyleSheet.create` gives editors suggestions and checks `ViewStyle`, `TextStyle`, and `ImageStyle`. Moving styles out of render also names them. `StyleSheet.compose` and `flatten` help combine objects. `compose` avoids an array allocation when one style is empty and keeps reference equality for `PureComponent` checks. Creating a fresh object inside render still changes identity every time.

**When to use:**

Prefer `StyleSheet.create` for named, reusable, type-checked styles. Use an inline object for a value that truly depends on render, such as a measured width.

**Examples:**

```tsx
const styles = StyleSheet.create({
  box: {padding: 12},
});
<View style={styles.box} />
<View style={{padding: 12}} />
```

**Sources:**

- [StyleSheet](https://reactnative.dev/docs/stylesheet)
- [Style](https://reactnative.dev/docs/style)
</details>

<details>
<summary>How do you use Redux for state management in React Native?</summary>

### How do you use Redux for state management in React Native?

**Definition:**

Redux keeps one store outside the React tree. React Native's testing guide names it as one way to separate app state from components so logic can be tested without rendering UI.

**How it works:**

The store holds state and applies pure reducers to actions. React Native components read it with `useSelector` and send actions with `useDispatch` from React-Redux. A `Provider` wraps the navigation tree. Do not persist secrets in a Redux slice that is written to Async Storage. The security guide allows persisting non-sensitive Redux state there and forbids tokens and secrets.

**When to use:**

Use Redux when many screens share updates that you want as explicit actions. Local `useState` is enough when one screen owns the data.

**Examples:**

```tsx
const store = configureStore({reducer: {todos: todosReducer}});

export function App() {
  return (
    <Provider store={store}>
      <Navigation />
    </Provider>
  );
}
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>What is the `useCallback` hook and how can it be useful in React Native?</summary>

### What is the `useCallback` hook and how can it be useful in React Native?

**Definition:**

`useCallback` returns a function whose identity stays the same until its dependencies change. That matters for `FlatList`, which skips rendering when props are shallow-equal.

**How it works:**

`renderItem`, `Pressable` handlers, and children of `memo` re-render when they receive a new function. `useCallback` keeps the function stable. It does not make the function faster by itself. Fast Refresh still updates `useCallback` during an edit even if the dependencies did not change. The list docs say anything `renderItem` closes over must be passed as `extraData` when it changes.

**Examples:**

```tsx
const renderItem = useCallback(
  ({item}: {item: Todo}) => <Text>{item.title}</Text>,
  [],
);
<FlatList data={todos} renderItem={renderItem} />
```

**Sources:**

- [FlatList](https://reactnative.dev/docs/flatlist)
- [Fast Refresh](https://reactnative.dev/docs/fast-refresh)
</details>

<details>
<summary>Explain how to use refs in React Native.</summary>

### Explain how to use refs in React Native.

**Definition:**

A ref points at a host component or a mutable value that must not trigger a render. React Native host refs expose methods such as `focus` and `measureInWindow`.

**How it works:**

`useRef` creates the ref. Pass it to `TextInput`, `ScrollView`, or `View`. Read `.current` in an event or in `useLayoutEffect`, not during render to decide UI. The New Architecture page shows `measureInWindow` inside `useLayoutEffect` so a tooltip can position itself in the same commit. Fast Refresh preserves `useRef` values when hook order stays the same.

**Examples:**

```tsx
const input = useRef<TextInput>(null);
<TextInput ref={input} />
<Button title="Edit" onPress={() => input.current?.focus()} />
```

**Sources:**

- [TextInput](https://reactnative.dev/docs/textinput)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
- [Fast Refresh](https://reactnative.dev/docs/fast-refresh)
</details>

<details>
<summary>How would you create a custom hook in React Native?</summary>

### How would you create a custom hook in React Native?

**Definition:**

A custom hook is a function whose name starts with `use` and that calls other hooks. It shares stateful logic without a higher-order component.

**How it works:**

Call it only at the top level of a component or another hook. The React Native docs show this shape for `useInitialURL`, which combines `useState` and `useEffect` around `Linking.getInitialURL`. Fast Refresh preserves `useState` and `useRef` inside the hook when the call order does not change.

**Examples:**

```tsx
function useInitialURL() {
  const [url, setUrl] = useState<string | null>(null);
  useEffect(() => {
    Linking.getInitialURL().then(setUrl);
  }, []);
  return url;
}
```

**Sources:**

- [Linking](https://reactnative.dev/docs/linking)
- [Fast Refresh](https://reactnative.dev/docs/fast-refresh)
</details>

<details>
<summary>In what ways can you improve the memory usage of a React Native app?</summary>

### In what ways can you improve the memory usage of a React Native app?

**Definition:**

Hermes is the default engine and is documented to use less memory than JavaScriptCore for many apps. Lists and images are the other large sources.

**How it works:**

Hermes compiles to bytecode at build time and lowers startup memory. `FlatList` and `VirtualizedList` unmount rows that leave the window; the docs warn that scrolling faster than the fill rate can show blanks, which is the memory tradeoff. Do not leave `renderToHardwareTextureAndroid` on after a view stops moving, because the performance guide says memory use can jump. Release builds drop the dev tooling that inflates the JavaScript heap.

**Examples:**

```tsx
<FlatList
  data={photos}
  windowSize={5}
  keyExtractor={item => item.id}
  renderItem={({item}) => <Image source={{uri: item.uri}} />}
/>
```

**Sources:**

- [Using Hermes](https://reactnative.dev/docs/hermes)
- [Performance](https://reactnative.dev/docs/performance)
- [VirtualizedList](https://reactnative.dev/docs/virtualizedlist)
</details>

<details>
<summary>What is the role of the React Native Bridge?</summary>

### What is the role of the React Native Bridge?

**Definition:**

The legacy bridge was the asynchronous message channel between JavaScript and native code. The New Architecture removes that bridge.

**How it works:**

In the legacy design, JavaScript and native serialized messages to each other. That cost showed up for large payloads such as camera frames. JSI lets JavaScript hold a C++ object and call it without that serialization. Turbo Native Modules and the Fabric renderer sit on JSI. Since 0.76 this architecture is the default. `newArchEnabled=false` or `RCT_NEW_ARCH_ENABLED=0` opts out.

**Examples:**

```properties
# android/gradle.properties
newArchEnabled=true
```

**Sources:**

- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
- [Turbo Native Modules](https://reactnative.dev/docs/turbo-native-modules-introduction)
</details>

<details>
<summary>How can you integrate a React Native app with a backend service?</summary>

### How can you integrate a React Native app with a backend service?

**Definition:**

Call the backend with `fetch` over HTTPS. React Native also includes `XMLHttpRequest`, so libraries such as axios can run on top of it. There is no CORS model in native apps.

**How it works:**

Pass the URL and an optional init object (`method`, `headers`, `body`). `response.json()` parses the body. Always handle rejection: an uncaught `fetch` error is dropped. iOS App Transport Security and Android API 28 block cleartext HTTP by default. The security guide says secrets such as third-party API keys belong on the server, which forwards the call, not in the app bundle.

**Examples:**

```tsx
const response = await fetch('https://api.example.com/orders', {
  method: 'POST',
  headers: {Accept: 'application/json', 'Content-Type': 'application/json'},
  body: JSON.stringify({sku: 'pen'}),
});
const order = await response.json();
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>How do you secure sensitive data such as API keys in a React Native app?</summary>

### How do you secure sensitive data such as API keys in a React Native app?

**Definition:**

Anything shipped in the app bundle can be read as plain text. The security guide says not to store sensitive API keys in app code, including values injected by react-native-dotenv or react-native-config.

**How it works:**

Put the secret on a server or serverless function that the app calls. For user secrets on device, React Native has no built-in secure store. The guide points to iOS Keychain and Android Encrypted Shared Preferences or Keystore, wrapped by `expo-secure-store` or `react-native-keychain`. Async Storage is unencrypted and is for non-sensitive data only. Deep links must not carry tokens. APIs should use HTTPS.

**Examples:**

```tsx
import * as SecureStore from 'expo-secure-store';

await SecureStore.setItemAsync('session', token);
```

**Sources:**

- [Security](https://reactnative.dev/docs/security)
</details>

### React Native Styling and Layout

<details>
<summary>How do you use the `StyleSheet` object to create styles?</summary>

### How do you use the `StyleSheet` object to create styles?

**Definition:**

`StyleSheet` groups named style objects, similar to a CSS stylesheet, and type-checks them against native style props.

**How it works:**

`StyleSheet.create` returns the same style map. `absoluteFill` is the common overlay (`position: 'absolute'` with all edges at 0). `hairlineWidth` is a thin platform line in whole pixels. `compose` merges two styles with the second winning. `flatten` turns an array into one object. `setStyleAttributePreprocessor` is experimental and is used internally for color and transform.

**Examples:**

```tsx
const styles = StyleSheet.create({
  rule: {borderBottomWidth: StyleSheet.hairlineWidth},
});
<View style={[StyleSheet.absoluteFill, {backgroundColor: 'rgba(0,0,0,0.4)'}]} />
```

**Sources:**

- [StyleSheet](https://reactnative.dev/docs/stylesheet)
</details>

<details>
<summary>Can you explain how to use Flexbox for responsive layouts in React Native?</summary>

### Can you explain how to use Flexbox for responsive layouts in React Native?

**Definition:**

Flexbox in React Native distributes free space among children so the same component can fit different screens.

**How it works:**

The default direction is `column`, not `row`. `flex: 1` makes a child fill the parent's free space along the main axis. `justifyContent` places children on the main axis. `alignItems` places them on the cross axis. `flexShrink` defaults to `0`, so a child does not shrink unless you set it. Combine these three rather than absolute coordinates.

**Examples:**

```tsx
<View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
  <View style={{flexDirection: 'row'}}>
    <View style={{flex: 1}} />
    <View style={{flex: 2}} />
  </View>
</View>
```

**Sources:**

- [Flexbox](https://reactnative.dev/docs/flexbox)
- [Layout with Flexbox](https://reactnative.dev/docs/flexbox)
</details>

<details>
<summary>How do you implement theming in a React Native application?</summary>

### How do you implement theming in a React Native application?

**Definition:**

A theme is a set of colors and text styles passed through React context. `Appearance` reports whether the user asked for a light or dark interface.

**How it works:**

`Appearance.getColorScheme()` returns `'light'`, `'dark'`, or `null`. `Appearance.addChangeListener` updates the app when the setting changes. Store the palette in context and read it where styles are built. Prefer named colors from the palette over raw hex in each component.

**Examples:**

```tsx
const scheme = useColorScheme();
const background = scheme === 'dark' ? '#111' : '#fff';
return <View style={{flex: 1, backgroundColor: background}} />;
```

**Sources:**

- [Appearance](https://reactnative.dev/docs/appearance)
- [Style](https://reactnative.dev/docs/style)
</details>

<details>
<summary>What consideration should be taken when styling components for different platforms (iOS and Android)?</summary>

### What consideration should be taken when styling components for different platforms (iOS and Android)?

**Definition:**

The same style props run through Yoga, but platforms differ on fonts, shadows, hairlines, and safe areas. `Platform.select` picks the object for the OS you are running.

**How it works:**

`Platform.select` prefers `ios` or `android`, then `native`, then `default`. Shadows on iOS use `shadow*` props; Android elevation is a separate prop. `StyleSheet.hairlineWidth` is not a fixed physical size. `SafeAreaView` keeps content out of the notch and the home indicator. `StatusBar` is also platform-specific.

**Examples:**

```tsx
const styles = StyleSheet.create({
  card: {
    ...Platform.select({
      ios: {shadowOpacity: 0.2, shadowRadius: 4},
      android: {elevation: 3},
      default: {},
    }),
  },
});
```

**Sources:**

- [Platform](https://reactnative.dev/docs/platform)
- [Platform-Specific Code](https://reactnative.dev/docs/platform-specific-code)
- [SafeAreaView](https://reactnative.dev/docs/safeareaview)
</details>

<details>
<summary>How can you use third-party styling libraries like Styled Components in React Native?</summary>

### How can you use third-party styling libraries like Styled Components in React Native?

**Definition:**

Styled-components can target React Native host components. The styles it produces are still the JavaScript style objects that `View` and `Text` accept.

**How it works:**

Import from `styled-components/native` and tag `View` or `Text`. Props on the styled component can compute a style object. The result still has to use React Native style names (`marginTop`, not `margin-top`) and Flexbox defaults from Yoga. Theme values can come from that library's theme provider or from `Appearance`.

**When to use:**

Use it when a design system is already written as styled components. `StyleSheet.create` is enough when styles are static objects.

**Examples:**

```tsx
import styled from 'styled-components/native';

const Title = styled.Text`
  color: #222;
  font-size: 20px;
`;
```

**Sources:**

- [Style](https://reactnative.dev/docs/style)
- [Flexbox](https://reactnative.dev/docs/flexbox)
</details>

### React Native Data Management and Networking

<details>
<summary>How do you handle state persistence in React Native?</summary>

### How do you handle state persistence in React Native?

**Definition:**

Persisted data is written to disk and survives a restart. The security guide says to use Async Storage for non-sensitive state and a secure store for tokens.

**How it works:**

Async Storage is a community key-value store, unencrypted, sandboxed per app. The guide allows it for non-sensitive Redux or GraphQL state and app-wide settings. It forbids tokens and secrets. Those go to Keychain on iOS or Encrypted Shared Preferences / Keystore on Android. Do not persist a whole Redux tree if that tree contains a session token.

**When to use:**

Async Storage for drafts and preferences. Keychain or Encrypted Shared Preferences for credentials.

**Examples:**

```tsx
import AsyncStorage from '@react-native-async-storage/async-storage';

await AsyncStorage.setItem('draft', JSON.stringify(draft));
```

**Sources:**

- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>Explain how to make network requests using axios or fetch in React Native.</summary>

### Explain how to make network requests using axios or fetch in React Native.

**Definition:**

`fetch` is built in. Axios works because `XMLHttpRequest` is also built in. Native apps do not apply browser CORS.

**How it works:**

`fetch(url, init)` returns a promise. Check the HTTP status yourself: `fetch` only rejects on network failure. Axios rejects on non-2xx if you configure `validateStatus`, and it parses JSON by default. Cookie auth with `fetch` is documented as unstable, and `redirect: 'manual'` and `credentials: 'omit'` do not work. Use HTTPS.

**When to use:**

Use `fetch` for a small number of calls. Use axios when you want interceptors, timeouts, and a shared client.

**Examples:**

```tsx
const response = await fetch('https://reactnative.dev/movies.json');
if (!response.ok) throw new Error(String(response.status));
const json = await response.json();
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
</details>

<details>
<summary>What is GraphQL and how can it be used in a React Native app?</summary>

### What is GraphQL and how can it be used in a React Native app?

**Definition:**

GraphQL is a query language for an API. The client asks for the fields it needs. React Native sends that query with the same `fetch` API it uses for any HTTPS POST.

**How it works:**

The body is a JSON document with `query` and optional `variables`. The server returns JSON shaped like the query. The security guide lists persisted GraphQL state as a valid Async Storage use, and forbids putting secrets in that store. Cache and normalized state usually live in a client such as Apollo, not in React Native itself.

**Examples:**

```tsx
const response = await fetch('https://api.example.com/graphql', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({query: '{ viewer { name } }'}),
});
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>How do you manage caching of network requests in React Native?</summary>

### How do you manage caching of network requests in React Native?

**Definition:**

React Native does not cache `fetch` responses for you. You store the last successful payload in memory or, for non-sensitive data, in Async Storage, and you avoid refetching while that copy is fresh.

**How it works:**

Keep the response in state keyed by the URL. Record a timestamp and skip the network until it expires. Persist the non-sensitive copy so the next launch can paint immediately, then revalidate. Do not cache tokens or secrets in Async Storage. The list docs say row data must live in the item or an external store because virtualization drops off-screen component state.

**Examples:**

```tsx
const cached = await AsyncStorage.getItem(url);
if (cached) setData(JSON.parse(cached));
const fresh = await fetch(url).then(response => response.json());
await AsyncStorage.setItem(url, JSON.stringify(fresh));
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Security](https://reactnative.dev/docs/security)
- [VirtualizedList](https://reactnative.dev/docs/virtualizedlist)
</details>

<details>
<summary>What are the best practices for data validation in React Native forms?</summary>

### What are the best practices for data validation in React Native forms?

**Definition:**

Validate the value the user can see, show the error in the form, and validate again on the server. `TextInput` is the input host component.

**How it works:**

Keep each field in state. Check it on submit, and optionally as the text changes. Tie the error to the input with `accessibilityLabel` and `accessibilityLabelledBy` so VoiceOver and TalkBack announce the field. Do not trust the client: the backend repeats the check. Avoid storing sensitive form values in a persisted Redux tree.

**Examples:**

```tsx
function onSubmit() {
  if (!email.includes('@')) {
    setError('Enter an email address');
    return;
  }
  send({email});
}
```

**Sources:**

- [TextInput](https://reactnative.dev/docs/textinput)
- [Accessibility](https://reactnative.dev/docs/accessibility)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>How can you integrate a React Native application with Redux Saga?</summary>

### How can you integrate a React Native application with Redux Saga?

**Definition:**

Redux Saga runs long-running async work in response to Redux actions. The React Native app still renders from the store; the saga performs `fetch` and then dispatches the result.

**How it works:**

A saga watches an action with `takeLatest` or `takeEvery`, calls `fetch`, and `put`s a success or failure action. The reducer stores the data. Mount the saga middleware when the store is created, and wrap the tree in `Provider`. Keep secrets out of actions that you persist.

**When to use:**

Use a saga when many screens trigger cancellable request flows from actions. A `useEffect` plus `fetch` is enough for one screen.

**Examples:**

```tsx
function* loadMovies() {
  const response: Response = yield call(fetch, 'https://reactnative.dev/movies.json');
  const json: {movies: Movie[]} = yield call([response, response.json]);
  yield put({type: 'movies/loaded', payload: json.movies});
}
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Testing](https://reactnative.dev/docs/testing-overview)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>Discuss the different ways to manage application state in React Native.</summary>

### Discuss the different ways to manage application state in React Native.

**Definition:**

React Native uses the same React state options: local state, context, and an external store. The testing guide says to keep view components separate from business logic and app state.

**How it works:**

Screen-owned values use `useState`. A value needed by a subtree uses context. A value shared across distant screens, with explicit updates, can live in a module store, Redux, or MobX. Server data is often kept next to the request cache rather than copied into a global UI store. Persist only the non-sensitive part.

**When to use:**

Start with local state. Lift to context or a store only when another screen must read or change the same value.

**Examples:**

```tsx
const [query, setQuery] = useState('');
```

**Sources:**

- [State](https://reactnative.dev/docs/state)
- [Testing](https://reactnative.dev/docs/testing-overview)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>How do you handle offline data synchronization in React Native?</summary>

### How do you handle offline data synchronization in React Native?

**Definition:**

Show the last non-sensitive copy from disk, queue writes while `AppState` and the network say the device is offline, and replay them when a request succeeds again.

**How it works:**

Read the cache from Async Storage on launch. If `fetch` fails, keep the cached rows and append the mutation to a local queue. When the app becomes `active`, flush the queue in order. Do not queue secrets into Async Storage. The user should see that the list is stale.

**Examples:**

```tsx
useEffect(() => {
  const sub = AppState.addEventListener('change', state => {
    if (state === 'active') flushQueue();
  });
  return () => sub.remove();
}, []);
```

**Sources:**

- [AppState](https://reactnative.dev/docs/appstate)
- [Networking](https://reactnative.dev/docs/network)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>What are the benefits of using Apollo Client in React Native applications?</summary>

### What are the benefits of using Apollo Client in React Native applications?

**Definition:**

Apollo Client stores GraphQL results in a normalized cache and can render those results from a React Native screen. The network call is still HTTPS from the app.

**How it works:**

Components request a query. Apollo deduplicates in-flight work and can read the cache on the next screen that asks for the same object. The security guide allows persisting GraphQL state when it is not sensitive. Tokens stay in a secure store and are attached by an auth link, not written into Async Storage with the cache.

**When to use:**

Use Apollo when several screens share one GraphQL schema and you want a normalized cache. Use `fetch` when you call a few REST endpoints.

**Examples:**

```tsx
const {data} = useQuery(VIEWER);
return <Text>{data?.viewer.name}</Text>;
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Security](https://reactnative.dev/docs/security)
</details>

### React Native Platform Specifics

<details>
<summary>How does React Native handle platform-specific code?</summary>

### How does React Native handle platform-specific code?

**Definition:**

React Native loads `Component.ios.js` or `Component.android.js` when you import `./Component`. `Platform.select` and `Platform.OS` branch inside one file.

**How it works:**

The bundler picks the platform file from the extension. Callers import the base path. Small differences stay in `Platform.select`. Larger trees of native UI can be separate files. The New Architecture's Turbo Modules use one typed spec and then a Kotlin or Swift implementation.

**Examples:**

```tsx
import BigButton from './BigButton';
```

**Sources:**

- [Platform-Specific Code](https://reactnative.dev/docs/platform-specific-code)
- [Platform](https://reactnative.dev/docs/platform)
</details>

<details>
<summary>What are some common platform-specific components in React Native?</summary>

### What are some common platform-specific components in React Native?

**Definition:**

Some host components wrap a control that exists on one platform, or they map to different native controls. `Platform.OS` tells you which one is running.

**How it works:**

`StatusBar`, `Switch`, `Modal`, `ActivityIndicator`, and `SafeAreaView` are shared components whose native implementation differs. Android has `DrawerLayoutAndroid` and `TouchableNativeFeedback`. iOS has `ActionSheetIOS`. The docs for each component list props that exist on only one OS.

**Examples:**

```tsx
const label = Platform.OS === 'ios' ? 'iOS menu' : 'Android menu';
```

**Sources:**

- [Platform](https://reactnative.dev/docs/platform)
- [StatusBar](https://reactnative.dev/docs/statusbar)
- [Switch](https://reactnative.dev/docs/switch)
</details>

<details>
<summary>Can you explain how to use the Platform module in React Native?</summary>

### Can you explain how to use the Platform module in React Native?

**Definition:**

`Platform` tells you the OS and picks a value for it. `Platform.OS` is `'ios'`, `'android'`, or another target such as `'web'`.

**How it works:**

`Platform.select` returns the `ios` or `android` entry when you are on a phone. If that key is missing it uses `native`, then `default`. `Platform.Version` is the OS version. The same helper can choose a component, not only a style.

**Examples:**

```tsx
const color = Platform.select({ios: 'red', android: 'green', default: 'blue'});
```

**Sources:**

- [Platform](https://reactnative.dev/docs/platform)
</details>

<details>
<summary>How do you build a component that renders differently on iOS and Android?</summary>

### How do you build a component that renders differently on iOS and Android?

**Definition:**

Put the two trees in `Widget.ios.tsx` and `Widget.android.tsx`, or branch once with `Platform.select`. Importers use the name without the platform suffix.

**How it works:**

The platform file extension is resolved by the bundler, so `import Widget from './Widget'` loads the right file. Use separate files when the trees diverge. Use `Platform.select` when one prop differs. Keep behavior tests on the shared logic, not on the platform chrome.

**When to use:**

Separate files when the markup diverges. `Platform.select` when a style or one child differs.

**Examples:**

```tsx
const Widget = Platform.select({
  ios: () => require('./Widget.ios'),
  android: () => require('./Widget.android'),
})();
```

**Sources:**

- [Platform-Specific Code](https://reactnative.dev/docs/platform-specific-code)
- [Platform](https://reactnative.dev/docs/platform)
</details>

<details>
<summary>Describe a scenario where you might use `NativeModules` in React Native.</summary>

### Describe a scenario where you might use `NativeModules` in React Native.

**Definition:**

`NativeModules` is the legacy way to call a native API that React Native does not expose. New modules are Turbo Native Modules.

**How it works:**

A typical case is a device API with no React Native component, such as a vendor SDK. The legacy object hangs off `NativeModules.MyModule`. A Turbo Module starts from a TypeScript or Flow spec, Codegen writes the native interface, and you implement it in Kotlin and Swift. The spec is what application code imports.

**When to use:**

Maintain `NativeModules` only in a legacy bridge app. Write a Turbo Module for a new native API on the New Architecture.

**Examples:**

```tsx
import {NativeModules} from 'react-native';

await NativeModules.CalendarModule.createEvent('Lunch', 'Office');
```

**Sources:**

- [Legacy Native Modules](https://reactnative.dev/docs/legacy/native-modules-intro)
- [Turbo Native Modules](https://reactnative.dev/docs/turbo-native-modules-introduction)
</details>

<details>
<summary>How do you use `Linking` to handle external URLs in React Native?</summary>

### How do you use `Linking` to handle external URLs in React Native?

**Definition:**

`Linking` opens URLs in other apps and reads URLs that open yours. `openURL` hands a link to the system. `canOpenURL` checks first.

**How it works:**

`Linking.openURL('https://reactnative.dev')` asks the OS to open that URL. `canOpenURL` resolves to whether an app can handle it. Incoming links use `getInitialURL` and the `'url'` event. Custom schemes can be claimed by another app, so the security guide says not to put secrets in them.

**Examples:**

```tsx
const supported = await Linking.canOpenURL(url);
if (supported) await Linking.openURL(url);
```

**Sources:**

- [Linking](https://reactnative.dev/docs/linking)
- [Security](https://reactnative.dev/docs/security)
</details>

### React Native Performance Optimization

<details>
<summary>What strategies can you employ to optimize the performance of a React Native application?</summary>

### What strategies can you employ to optimize the performance of a React Native application?

**Definition:**

Stay inside the 16.67 ms frame budget on both the JavaScript thread and the native UI thread. Profile a release build, because dev mode is much slower.

**How it works:**

Remove `console.log` from the release bundle. Give `FlatList` a `getItemLayout`. Move animation to the native driver. Defer non-urgent work with `requestIdleCallback` or `startTransition` so input stays responsive. Avoid changing image width and height during an animation; scale with `transform`. Do not leave hardware textures enabled after motion stops.

**Examples:**

```tsx
<FlatList data={rows} getItemLayout={(_, index) => ({length: 44, offset: 44 * index, index})} renderItem={renderItem} />
```

**Sources:**

- [Performance](https://reactnative.dev/docs/performance)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
</details>

<details>
<summary>How would you reduce the app bundle size in a React Native project?</summary>

### How would you reduce the app bundle size in a React Native project?

**Definition:**

Hermes is on by default and is documented to produce a smaller app than JavaScriptCore for many apps, because JavaScript is compiled to Hermes bytecode at build time.

**How it works:**

Ship a release build so the bundle is minified and dev-only warnings are gone. Confirm you are loading the `.hbc` bytecode, not a fallback that only exposes `HermesInternal`. The performance guide also recommends `babel-plugin-transform-remove-console` so logging code is not in the release bundle. Split optional native features so they are not linked into every binary.

**Examples:**

```tsx
const isHermes = () => !!(global as {HermesInternal?: unknown}).HermesInternal;
```

**Sources:**

- [Using Hermes](https://reactnative.dev/docs/hermes)
- [Optimizing JavaScript loading](https://reactnative.dev/docs/optimizing-javascript-loading)
- [Performance](https://reactnative.dev/docs/performance)
</details>

<details>
<summary>Explain how to use the `PureComponent` class for performance gains in React Native.</summary>

### Explain how to use the `PureComponent` class for performance gains in React Native.

**Definition:**

`PureComponent` skips render when props and state are shallow-equal. `FlatList` is a `PureComponent`, which is why unchanged list props do not re-render rows.

**How it works:**

Shallow equality compares each prop with `===`. A new object, array, or function every render defeats the skip. The list docs say to pass changing outside data as `extraData`. `StyleSheet.compose` keeps reference equality when one style is empty so that check can succeed. Function components use `memo` for the same shallow compare.

**When to use:**

Use it for rows that render often with the same props. Skip it when props are always new objects.

**Examples:**

```tsx
class Row extends React.PureComponent<{title: string}> {
  render() {
    return <Text>{this.props.title}</Text>;
  }
}
```

**Sources:**

- [FlatList](https://reactnative.dev/docs/flatlist)
- [StyleSheet](https://reactnative.dev/docs/stylesheet)
</details>

<details>
<summary>Why is it important to avoid unnecessary renders in React Native, and how can you achieve this?</summary>

### Why is it important to avoid unnecessary renders in React Native, and how can you achieve this?

**Definition:**

A render that takes too long on the JavaScript thread drops frames. The performance guide's example is a root state update that takes 200 ms and drops about 12 frames, which freezes JavaScript-driven animation and delays touches.

**How it works:**

Keep state as local as possible. Batch updates; the New Architecture's React 18 renderer batches updates from native events. Mark non-urgent trees with `startTransition`. Memoize rows and pass stable callbacks. For lists, put extra dependencies in `extraData` instead of rendering the whole list from unrelated parent state.

**Examples:**

```tsx
startTransition(() => setTileCount(next));
```

**Sources:**

- [Performance](https://reactnative.dev/docs/performance)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
</details>

<details>
<summary>What is the Hermes engine and how does it benefit React Native performance?</summary>

### What is the Hermes engine and how does it benefit React Native performance?

**Definition:**

Hermes is the open-source JavaScript engine optimized for React Native. It is the default, and no extra setup is required.

**How it works:**

For many apps Hermes improves startup time, decreases memory use, and reduces app size compared with JavaScriptCore. React Native ships a bundled Hermes build matched to that React Native version. A release build compiles JavaScript to Hermes bytecode (`.hbc`). `global.HermesInternal` shows that the engine is present; you still need the bytecode file to get the precompiled path. JavaScriptCore remains available if you opt out.

**Examples:**

```tsx
const isHermes = () => !!global.HermesInternal;
```

**Sources:**

- [Using Hermes](https://reactnative.dev/docs/hermes)
</details>

### React Native Testing and Debugging

<details>
<summary>What are some popular testing frameworks for React Native applications?</summary>

### What are some popular testing frameworks for React Native applications?

**Definition:**

The default template ships Jest, ESLint, and TypeScript. Component tests use React Native Testing Library. End-to-end tests in the community often use Detox, Appium, or Maestro.

**How it works:**

Jest includes a React Native preset. ESLint and TypeScript catch mistakes without running the app. React Native Testing Library renders components in Node and adds `fireEvent` and queries. React's Test Renderer is documented as deprecated. Detox is called out as popular because it is tailored to React Native; Appium and Maestro are the other device-level options in the testing guide.

**Examples:**

```tsx
test('adds a row', () => {
  const {getByText} = render(<GroceryShoppingList />);
  fireEvent.press(getByText('Add the item to list'));
});
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
</details>

<details>
<summary>How do you write a unit test for a React Native component?</summary>

### How do you write a unit test for a React Native component?

**Definition:**

The testing guide asks you to test what the user sees, with React Native Testing Library, not the component's state or props.

**How it works:**

Render the component. `fireEvent.changeText` and `fireEvent.press` perform the interaction. Assert with `getByText` or `getByPlaceholderText`. One test should check one behavior and stay independent. Mock native modules that cannot run in Node. Component tests do not execute iOS or Android code, so they cannot prove the native side.

**Examples:**

```tsx
test('given empty list, user can add banana', () => {
  const {getByPlaceholderText, getByText, getAllByText} = render(<GroceryShoppingList />);
  fireEvent.changeText(getByPlaceholderText('Enter grocery item'), 'banana');
  fireEvent.press(getByText('Add the item to list'));
  expect(getAllByText('banana')).toHaveLength(1);
});
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
</details>

<details>
<summary>Describe the process for end-to-end testing in React Native.</summary>

### Describe the process for end-to-end testing in React Native.

**Definition:**

An end-to-end test builds the app in release mode and drives it on a device or simulator as a user. It does not call React components, Redux, or your business logic.

**How it works:**

The testing guide says to cover vital flows: sign-in, core tasks, payments. Detox, Appium, and Maestro can tap buttons and type into fields, then assert that text is on screen. These tests are slower and flakier than Jest. Use Jest for the rest. A flaky test passes and fails without a code change.

**When to use:**

Use E2E for the few flows that must work on a real device. Use Jest for everything you can assert in JavaScript.

**Examples:**

```txt
detox build --configuration ios.sim.release
detox test --configuration ios.sim.release
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
</details>

<details>
<summary>What are some common debugging techniques in React Native?</summary>

### What are some common debugging techniques in React Native?

**Definition:**

Debug with the in-app Dev Menu, LogBox, and React Native DevTools. These are disabled in release builds.

**How it works:**

Open the Dev Menu with Ctrl+Cmd+Z on the iOS simulator or Cmd+M / Ctrl+M on Android. Press `j` or choose Open DevTools. DevTools has a console, a component inspector, and a profiler. LogBox shows fatal errors and warning badges. `LogBox.ignoreLogs` hides a noisy warning. The in-app Perf Monitor is only a guide; Android Studio and Xcode are the accurate native tools.

**Examples:**

```tsx
import {LogBox} from 'react-native';
LogBox.ignoreLogs([/GraphQL error:.*/]);
```

**Sources:**

- [Debugging](https://reactnative.dev/docs/debugging)
</details>

<details>
<summary>What tools would you use for performance profiling in React Native?</summary>

### What tools would you use for performance profiling in React Native?

**Definition:**

Start with the in-app Performance Monitor, then use React Native DevTools' profiler and the native profilers in Android Studio and Xcode. Always measure a release build.

**How it works:**

The Dev Menu toggles Perf Monitor, which shows the JavaScript frame rate and the UI frame rate. DevTools includes a Profiler panel. The debugging guide says the in-app monitor is a guide and points to Android Studio and Xcode for accurate numbers. Hermes bytecode only shows its startup benefit in a release build.

**Examples:**

```txt
npm run android -- --mode=release
```

**Sources:**

- [Debugging](https://reactnative.dev/docs/debugging)
- [Profiling](https://reactnative.dev/docs/profiling)
- [Performance](https://reactnative.dev/docs/performance)
</details>

### React Native Navigation and Routing

<details>
<summary>Compare and contrast React Navigation and React Native Navigation.</summary>

### Compare and contrast React Navigation and React Native Navigation.

**Definition:**

React Navigation is the library the React Native navigation guide tells you to start with. `react-native-navigation` is the alternative when navigation is implemented with native screen containers, including an app that already navigates natively.

**How it works:**

React Navigation describes screens in JavaScript. `createNativeStackNavigator` still uses `UINavigationController` and Android fragments, so the transition itself runs on the UI thread. The performance guide says that is why native stacks stay smooth when the JavaScript thread drops frames. `react-native-navigation` is the documented option when you are integrating into an app that already manages navigation natively.

**When to use:**

React Navigation for a new JavaScript app. React Native Navigation when native code already owns the stack.

**Examples:**

```tsx
const RootStack = createNativeStackNavigator({
  screens: {Home: {screen: HomeScreen}, Profile: {screen: ProfileScreen}},
});
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Performance](https://reactnative.dev/docs/performance)
</details>

<details>
<summary>How do you pass parameters between screens using React Navigation?</summary>

### How do you pass parameters between screens using React Navigation?

**Definition:**

Pass a params object as the second argument to `navigation.navigate`. The next screen reads `route.params`.

**How it works:**

The navigation guide's profile example navigates with `{name: 'Jane'}` and renders `route.params.name`. Params should be serializable identifiers and small display values. Do not put secrets in params that may also be reflected in a deep link.

**Examples:**

```tsx
navigation.navigate('Profile', {name: 'Jane'});

function ProfileScreen({route}) {
  return <Text>{route.params.name}</Text>;
}
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>Explain tab navigation in React Native.</summary>

### Explain tab navigation in React Native.

**Definition:**

Tab navigation is a navigator that shows several screens and a tab bar. The React Native navigation guide says React Navigation includes tab and drawer packages as well as the native stack.

**How it works:**

Each tab is a screen component registered on a tab navigator, the same way `Home` and `Profile` are registered on a stack. The tab bar is part of the navigator UI. A tab screen can itself be a stack navigator, so each tab has its own history. The native stack's transitions stay on the UI thread.

**When to use:**

Use tabs when the user switches between a few top-level areas. Use a stack when one area opens details and should go back.

**Examples:**

```tsx
const RootStack = createNativeStackNavigator({
  screens: {Home: {screen: HomeScreen}, Settings: {screen: SettingsScreen}},
});
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
</details>

<details>
<summary>How can you implement deep linking with React Navigation?</summary>

### How can you implement deep linking with React Navigation?

**Definition:**

React Navigation can open a screen from a URL that `Linking` already delivers. Configure a path for each screen, then read the initial URL and subscribe to later ones.

**How it works:**

`Linking.getInitialURL` covers a cold start. The `'url'` event covers links while the app is open. Map path segments onto screen names and params. The security guide warns that a custom scheme is not exclusive: another app can register it. Do not put tokens in the URL. Prefer universal links on iOS.

**Examples:**

```tsx
const linking = {
  prefixes: ['example://'],
  config: {screens: {Profile: 'profile/:name'}},
};
```

**Sources:**

- [Linking](https://reactnative.dev/docs/linking)
- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>What is the difference between stack navigation and modal navigation in React Native?</summary>

### What is the difference between stack navigation and modal navigation in React Native?

**Definition:**

A stack pushes a screen and returns by popping it, usually with a header and a horizontal or platform transition. A modal is presented over the current screen and is dismissed rather than popped as part of the same task flow.

**How it works:**

The navigation guide's native stack registers screens in one navigator and can set options such as `title` per screen. React Navigation also lets a screen use a modal presentation option so it covers the stack instead of pushing inside it. `Modal` from React Native is a separate component: it is a view you show, not a route. The performance guide notes that native stack transitions keep running when the JavaScript thread is busy.

**When to use:**

Use a stack screen for a place the user navigates to and back from. Use a modal for a short decision or a temporary task over the current screen.

**Examples:**

```tsx
<Modal visible={open} onRequestClose={() => setOpen(false)}>
  <Text>Discard this draft?</Text>
</Modal>
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Modal](https://reactnative.dev/docs/modal)
- [Performance](https://reactnative.dev/docs/performance)
</details>

### React Native Build and Deployment

<details>
<summary>What is the process to build and package a React Native app for production?</summary>

### What is the process to build and package a React Native app for production?

**Definition:**

Produce a release build so Hermes compiles bytecode, dev warnings are stripped, and the native binary is signed for the store.

**How it works:**

The Hermes guide shows `npm run android -- --mode=release` and `npm run ios -- --mode=Release`. Android packaging for the Play Store is a signed release APK or AAB, documented in the signed-APK guide. iOS packaging for the App Store is an archive signed with a distribution certificate, documented in the publishing guide. Confirm `global.HermesInternal` and that the bundle loaded is `.hbc`.

**Examples:**

```txt
npm run android -- --mode=release
npm run ios -- --mode=Release
```

**Sources:**

- [Using Hermes](https://reactnative.dev/docs/hermes)
- [Publishing to Google Play Store](https://reactnative.dev/docs/signed-apk-android)
- [Publishing to Apple App Store](https://reactnative.dev/docs/publishing-to-app-store)
</details>

<details>
<summary>How do you configure app signing for deploying a React Native app to the Google Play Store or Apple App Store?</summary>

### How do you configure app signing for deploying a React Native app to the Google Play Store or Apple App Store?

**Definition:**

Android release builds are signed with an upload keystore. iOS release archives are signed with an Apple distribution certificate and a provisioning profile.

**How it works:**

The signed-APK guide walks through generating an upload keystore, putting the passwords in Gradle properties that are not committed, and pointing `signingConfigs.release` at that keystore. Play App Signing then re-signs the upload. The App Store guide uses Xcode signing with your Apple Developer team so the archive can be uploaded. Debug keystores are not the release identity.

**Examples:**

```gradle
signingConfigs {
    release {
        storeFile file(MYAPP_UPLOAD_STORE_FILE)
        storePassword MYAPP_UPLOAD_STORE_PASSWORD
        keyAlias MYAPP_UPLOAD_KEY_ALIAS
        keyPassword MYAPP_UPLOAD_KEY_PASSWORD
    }
}
```

**Sources:**

- [Publishing to Google Play Store](https://reactnative.dev/docs/signed-apk-android)
- [Publishing to Apple App Store](https://reactnative.dev/docs/publishing-to-app-store)
</details>

<details>
<summary>Explain the steps to update a React Native app already deployed on the app stores.</summary>

### Explain the steps to update a React Native app already deployed on the app stores.

**Definition:**

A native or JavaScript change that ships inside the binary needs a new store build with a higher version, signed with the same release identity.

**How it works:**

Bump the Android `versionCode` and the iOS `CFBundleVersion`, build a release binary, and upload it. Play Store and App Store review that binary. Users receive it through the store. JavaScript-only fixes can instead be delivered by an over-the-air update only if the already installed binary contains that update client. The Hermes bytecode is produced again at release build time.

**Examples:**

```txt
npm run android -- --mode=release
```

**Sources:**

- [Publishing to Google Play Store](https://reactnative.dev/docs/signed-apk-android)
- [Publishing to Apple App Store](https://reactnative.dev/docs/publishing-to-app-store)
- [Using Hermes](https://reactnative.dev/docs/hermes)
</details>

<details>
<summary>What is CodePush and how can it be used in React Native apps?</summary>

### What is CodePush and how can it be used in React Native apps?

**Definition:**

CodePush published JavaScript, image, and other bundle updates to installed apps without a store review. Hosted App Center CodePush was retired on 31 March 2025.

**How it works:**

Microsoft documents a standalone CodePush server you can run yourself, compatible with the CodePush React Native client. After that date the hosted service no longer accepts sign-in or API calls. Apps already pointing at App Center keep running; they do not get new hosted updates. Expo's replacement path is EAS Update, which requires a new store binary that includes `expo-updates`. Native code changes still need a store release.

**When to use:**

Self-host the standalone server if you already ship the CodePush client. Use EAS Update for a managed Expo pipeline. Use a store build for native changes.

**Examples:**

```txt
# Self-hosted server documented by Microsoft
# https://github.com/microsoft/code-push-server
```

**Sources:**

- [App Center retirement](https://learn.microsoft.com/en-us/appcenter/retirement)
- [CodePush standalone server](https://github.com/microsoft/code-push-server)
- [Migrate from CodePush](https://docs.expo.dev/eas-update/codepush)
</details>

<details>
<summary>How do you manage different app environments (development, staging, production) in React Native?</summary>

### How do you manage different app environments (development, staging, production) in React Native?

**Definition:**

Environments differ by API host and public config, not by secrets baked into the bundle. The security guide says dotenv-style files are for endpoints, not for API keys.

**How it works:**

Select the host with a build-time public variable or a non-secret config module. Release builds use the production host and Hermes bytecode. Dev builds use the local packager, Dev Menu, and LogBox, which are absent in release. Store tokens from each environment in that build's secure storage, not in source.

**Examples:**

```tsx
const apiHost = __DEV__ ? 'https://staging.example.com' : 'https://api.example.com';
```

**Sources:**

- [Security](https://reactnative.dev/docs/security)
- [Debugging](https://reactnative.dev/docs/debugging)
- [Environment setup](https://reactnative.dev/docs/environment-setup)
</details>

### React Native Ecosystem and Libraries

<details>
<summary>List some common libraries that you would consider essential for React Native development.</summary>

### List some common libraries that you would consider essential for React Native development.

**Definition:**

The core framework already includes `View`, `Text`, `Pressable`, `FlatList`, `StyleSheet`, `Linking`, `Animated`, and `fetch`. Most apps add navigation, safe area, and screens, which the navigation guide installs explicitly.

**How it works:**

The navigation guide installs `@react-navigation/native`, `@react-navigation/native-stack`, `react-native-screens`, and `react-native-safe-area-context`. Persistence of non-sensitive state uses the community Async Storage module named in the security guide. Secrets use `expo-secure-store` or `react-native-keychain`. Tests use Jest and React Native Testing Library from the testing guide.

**Examples:**

```txt
npm install @react-navigation/native @react-navigation/native-stack
npx expo install react-native-screens react-native-safe-area-context
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Security](https://reactnative.dev/docs/security)
- [Testing](https://reactnative.dev/docs/testing-overview)
</details>

<details>
<summary>How do you integrate a navigation library like React Navigation into a React Native app?</summary>

### How do you integrate a navigation library like React Navigation into a React Native app?

**Definition:**

Install the packages from the navigation guide, register screens on a native stack, and render `createStaticNavigation` as the root.

**How it works:**

For Expo, `npx expo install react-native-screens react-native-safe-area-context`. For a bare app, install them with npm and run `pod install` on iOS. Define `screens` with a component and options such as `title`. Inside a screen, `useNavigation().navigate` opens another registered name.

**Examples:**

```tsx
const Navigation = createStaticNavigation(RootStack);
export default function App() {
  return <Navigation />;
}
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
</details>

<details>
<summary>What is Expo, and what are its pros and cons?</summary>

### What is Expo, and what are its pros and cons?

**Definition:**

Expo is the toolchain the React Native setup and navigation guides use to create a project and install compatible native dependencies.

**How it works:**

The navigation guide starts a project with `npx create-expo-app@latest --template react-navigation/template` and installs peers with `npx expo install`, which picks versions that match the SDK. That removes a lot of native setup. A library that needs custom native code may require a config plugin or a prebuild, and you cannot assume every native module works inside Expo Go. Release binaries are still store builds.

**When to use:**

Use Expo when you want the documented starter and version-matched native modules. Use a bare React Native app when you already own the Xcode and Gradle projects.

**Examples:**

```txt
npx create-expo-app@latest --template react-navigation/template
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Environment setup](https://reactnative.dev/docs/environment-setup)
</details>

<details>
<summary>How would you integrate push notifications into a React Native app?</summary>

### How would you integrate push notifications into a React Native app?

**Definition:**

Push is a native OS service. React Native does not ship a push client. You request permission, register a device token, and send the token to your backend over HTTPS.

**How it works:**

On a physical device the OS issues a token after the user allows notifications. The app posts that token to your server with `fetch`. The server talks to APNs or FCM. Taps can open a screen through `Linking` or the navigation linking config. Do not put secrets or session tokens in the notification payload. A store build is required after adding the native push entitlement.

**Examples:**

```tsx
await fetch('https://api.example.com/devices', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({token}),
});
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Linking](https://reactnative.dev/docs/linking)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>Describe how you would use a library like React Native Elements to enhance the UI of your app.</summary>

### Describe how you would use a library like React Native Elements to enhance the UI of your app.

**Definition:**

A component kit supplies themed buttons, lists, and inputs that still render React Native host components and accept React Native style objects.

**How it works:**

Install the kit, wrap the app in its theme provider if it has one, and use its controls instead of rebuilding press, spacing, and typography. Keep Flexbox defaults in mind: column direction and no shrink unless set. Map the kit theme to `Appearance` so dark mode follows the system. Accessibility labels still have to be set when the visible text is not enough.

**When to use:**

Use a kit when you want consistent controls quickly. Use `StyleSheet` and core components when the product design does not match the kit.

**Examples:**

```tsx
<Button title="Save" onPress={save} />
```

**Sources:**

- [Style](https://reactnative.dev/docs/style)
- [Flexbox](https://reactnative.dev/docs/flexbox)
- [Appearance](https://reactnative.dev/docs/appearance)
- [Accessibility](https://reactnative.dev/docs/accessibility)
</details>

### Advanced React Native Topics

<details>
<summary>What is the Flipper tool and how does it help in React Native development?</summary>

### What is the Flipper tool and how does it help in React Native development?

**Definition:**

Flipper was the previous desktop debugger. The current debugging guide documents React Native DevTools instead, opened from the Dev Menu or by pressing `j`.

**How it works:**

DevTools attaches to the running app and shows logs, a component inspector, and a profiler. LogBox surfaces errors inside the app. The Perf Monitor overlays JavaScript and UI frame rates. None of these run in a release build. Accurate native traces still come from Android Studio and Xcode.

**Examples:**

```txt
# In the CLI while the app is running
j
```

**Sources:**

- [Debugging](https://reactnative.dev/docs/debugging)
- [Profiling](https://reactnative.dev/docs/profiling)
</details>

<details>
<summary>Can you explain the use of Turbo Modules in React Native?</summary>

### Can you explain the use of Turbo Modules in React Native?

**Definition:**

A Turbo Native Module exposes a native API to JavaScript through a typed spec and JSI, without the legacy serialized bridge.

**How it works:**

Write the spec in TypeScript or Flow. Configure Codegen so it generates the native interfaces. Application code imports the spec. Implement the generated interface on Android and iOS and register it with the runtime. This is the path the New Architecture uses for native capabilities that core does not provide.

**Examples:**

```tsx
import {TurboModuleRegistry, TurboModule} from 'react-native';

export interface Spec extends TurboModule {
  createEvent(title: string): Promise<void>;
}

export default TurboModuleRegistry.getEnforcing<Spec>('Calendar');
```

**Sources:**

- [Turbo Native Modules](https://reactnative.dev/docs/turbo-native-modules-introduction)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
</details>

<details>
<summary>Describe the process of linking native modules in a React Native app.</summary>

### Describe the process of linking native modules in a React Native app.

**Definition:**

Autolinking connects a library's native projects when you install the npm package. A module you write yourself is registered through Codegen and the app's native target.

**How it works:**

Install the package, then for a bare iOS app run `pod install` as the navigation guide does for `react-native-screens`. Android Gradle picks up autolinked packages. A Turbo Module also needs the spec so Codegen can emit the interface before the native code compiles. Expo projects use `npx expo install` so the native version matches the SDK.

**Examples:**

```txt
npm install react-native-screens
cd ios && pod install
```

**Sources:**

- [Turbo Native Modules](https://reactnative.dev/docs/turbo-native-modules-introduction)
- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Integration with Existing Apps](https://reactnative.dev/docs/integration-with-existing-apps)
</details>

<details>
<summary>What is Reanimated, and when would you use it in a React Native app?</summary>

### What is Reanimated, and when would you use it in a React Native app?

**Definition:**

Reanimated is a library that runs animation worklets on the UI thread. The built-in `Animated` API can also leave the JavaScript thread when `useNativeDriver` is true, but only for a limited set of styles.

**How it works:**

The performance guide says JavaScript-driven animation freezes when the JavaScript thread drops frames, while native-driven motion does not. Use Reanimated when a gesture must update layout continuously and `Animated`'s native driver is not enough. Use `Animated` with `useNativeDriver: true` for opacity and transform. Use `LayoutAnimation` for a one-shot layout change.

**When to use:**

Reanimated for gesture-driven UI-thread animation. Built-in `Animated` for native-driver opacity and transforms.

**Examples:**

```tsx
Animated.timing(value, {toValue: 1, duration: 180, useNativeDriver: true}).start();
```

**Sources:**

- [Animated](https://reactnative.dev/docs/animated)
- [Performance](https://reactnative.dev/docs/performance)
</details>

<details>
<summary>How do you create a bridge for a native module in React Native?</summary>

### How do you create a bridge for a native module in React Native?

**Definition:**

On the New Architecture you do not add a legacy bridge module. You publish a typed spec and implement the generated native interface. JSI calls it directly.

**How it works:**

The Turbo Module guide lists four steps: define the spec in TypeScript or Flow, run Codegen, call the spec from JavaScript, and implement the generated interface in Kotlin and Swift or Objective-C. The legacy `NativeModules` bridge remains only for apps that opted out of the New Architecture. JSI avoids serializing every call.

**Examples:**

```tsx
export interface Spec extends TurboModule {
  getBatteryLevel(): Promise<number>;
}
```

**Sources:**

- [Turbo Native Modules](https://reactnative.dev/docs/turbo-native-modules-introduction)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
- [Legacy Native Modules](https://reactnative.dev/docs/legacy/native-modules-intro)
</details>

<details>
<summary>Explain the concept of shared element transitions in React Native.</summary>

### Explain the concept of shared element transitions in React Native.

**Definition:**

A shared element transition continues one visual from the source screen into the matching visual on the next screen, instead of cross-fading two unrelated views.

**How it works:**

React Native's own native stack animates the screen container on the UI thread, which the performance guide contrasts with JavaScript-driven transitions that freeze when the JavaScript thread stalls. A shared-element effect is an extra library on top of that navigation: it matches views by an id and animates the frame between them. The animation should run on the UI thread so a busy JavaScript thread does not drop it.

**Examples:**

```tsx
navigation.navigate('Photo', {id: photo.id});
```

**Sources:**

- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
- [Performance](https://reactnative.dev/docs/performance)
- [Animated](https://reactnative.dev/docs/animated)
</details>

<details>
<summary>How would you go about writing native code in Swift or Kotlin for React Native?</summary>

### How would you go about writing native code in Swift or Kotlin for React Native?

**Definition:**

Write a Turbo Module: a typed JavaScript spec, Codegen output, then Swift or Objective-C on iOS and Kotlin on Android.

**How it works:**

The spec is the contract application code imports. Codegen generates the native interfaces. You implement those interfaces and hook them into the React Native runtime. The integration guide shows `newArchEnabled` and `hermesEnabled` in `gradle.properties` for an Android host. Keep the method surface small and asynchronous when the work can take time.

**Examples:**

```tsx
export interface Spec extends TurboModule {
  scan(): Promise<string>;
}
```

**Sources:**

- [Turbo Native Modules](https://reactnative.dev/docs/turbo-native-modules-introduction)
- [Integration with Existing Apps](https://reactnative.dev/docs/integration-with-existing-apps)
</details>

<details>
<summary>What is "Fast Refresh" in React Native?</summary>

### What is "Fast Refresh" in React Native?

**Definition:**

Fast Refresh applies most component edits in about a second without a full reload. It is on by default and can be toggled in the Dev Menu.

**How it works:**

Editing a file that only exports React components re-renders those components and tries to keep state. Editing a module that is not only components re-runs importers. A file imported outside the React tree falls back to a full reload. Syntax errors recover on the next successful save. Class components do not keep state. `useState` and `useRef` keep values if hook order is unchanged. `useEffect`, `useMemo`, and `useCallback` update on refresh even when dependencies did not change. `// @refresh reset` forces a remount.

**Examples:**

```tsx
// @refresh reset
export function Splash() {
  return <Text>Hello</Text>;
}
```

**Sources:**

- [Fast Refresh](https://reactnative.dev/docs/fast-refresh)
</details>

<details>
<summary>How does React Native compile to native code?</summary>

### How does React Native compile to native code?

**Definition:**

JavaScript is not compiled into Swift or Kotlin. React Native runs JavaScript in Hermes and creates real native views through the renderer.

**How it works:**

In a release build Hermes compiles the bundle to bytecode (`.hbc`) ahead of time, which improves startup. The native app shell is still compiled by Xcode or Gradle. Fabric, on the New Architecture, turns the React element tree into native views. JSI connects JavaScript to C++ without the old serialized bridge. Since 0.76 that architecture is the default.

**Examples:**

```txt
npm run ios -- --mode=Release
```

**Sources:**

- [Using Hermes](https://reactnative.dev/docs/hermes)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
</details>

<details>
<summary>Can you integrate a React Native application into an existing native iOS or Android app?</summary>

### Can you integrate a React Native application into an existing native iOS or Android app?

**Definition:**

Yes. React Native can render inside a view controller or activity of an app you already ship. The integration guide is the supported path.

**How it works:**

Add the React Native dependency to the existing Gradle or CocoaPods project, enable the New Architecture and Hermes unless you must opt out, and mount a root component in the native screen. If that app already navigates natively, the navigation guide points to `react-native-navigation` rather than replacing the native stack with React Navigation.

**Examples:**

```properties
newArchEnabled=true
hermesEnabled=true
```

**Sources:**

- [Integration with Existing Apps](https://reactnative.dev/docs/integration-with-existing-apps)
- [Navigating Between Screens](https://reactnative.dev/docs/navigation)
</details>

### React Native Best Practices

<details>
<summary>What are the best practices for structuring a large React Native codebase?</summary>

### What are the best practices for structuring a large React Native codebase?

**Definition:**

Split the app into small modules so each piece can be tested without rendering the whole UI. The testing guide uses that split as the definition of testable code.

**How it works:**

Keep screens thin. Put request and domain logic in functions that do not import components. Group by feature, with that feature's components, hooks, and tests together. Isolate platform files (`*.ios`, `*.android`) and native specs at the edge. One navigator module owns route names.

**Examples:**

```txt
src/orders/OrderScreen.tsx
src/orders/loadOrders.ts
src/orders/loadOrders.test.ts
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
- [Platform-Specific Code](https://reactnative.dev/docs/platform-specific-code)
</details>

<details>
<summary>How should you handle sensitive data in a React Native app?</summary>

### How should you handle sensitive data in a React Native app?

**Definition:**

Do not ship secrets in the bundle, do not persist them in Async Storage, and do not put them in deep links. Use the platform secure store for tokens.

**How it works:**

The security guide says API keys belong behind your server. Async Storage is unencrypted and sandboxed; it is for drafts and non-sensitive state. Tokens go to Keychain or Encrypted Shared Preferences through `expo-secure-store` or `react-native-keychain`. Use HTTPS. Add certificate pinning only if you can ship a new build before certificates expire. OAuth redirects on custom schemes need PKCE.

**Examples:**

```tsx
await SecureStore.setItemAsync('accessToken', token);
```

**Sources:**

- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>What are the best practices for API interaction and data fetching in React Native?</summary>

### What are the best practices for API interaction and data fetching in React Native?

**Definition:**

Call HTTPS with `fetch`, handle both network failures and HTTP errors, and keep request logic out of the view so it can be unit tested.

**How it works:**

Check `response.ok` before parsing JSON. Cancel or ignore a response if the screen unmounts. Do not log tokens. iOS ATS and Android cleartext blocking mean production URLs are `https`. Cookie auth through `fetch` is documented as unstable. The testing guide says to mock the service in unit tests so they do not hit the network.

**Examples:**

```tsx
const response = await fetch(url);
if (!response.ok) throw new Error(String(response.status));
return response.json();
```

**Sources:**

- [Networking](https://reactnative.dev/docs/network)
- [Testing](https://reactnative.dev/docs/testing-overview)
- [Security](https://reactnative.dev/docs/security)
</details>

<details>
<summary>Discuss the importance of accessibility in React Native apps.</summary>

### Discuss the importance of accessibility in React Native apps.

**Definition:**

iOS VoiceOver and Android TalkBack are the platform screen readers. React Native maps `accessible`, labels, and roles onto those APIs so people who do not rely on the screen can use the app.

**How it works:**

Touchable elements are accessible by default. Set `accessibilityLabel` when the visible text is not the spoken name. `accessibilityLabelledBy` points at another element's `nativeID` for form fields. On Android `accessible` becomes `focusable`; on iOS it becomes `isAccessibilityElement`. The testing guide says to assert with text and accessibility helpers rather than test IDs.

**Examples:**

```tsx
<Pressable accessibilityLabel="Save order" onPress={save}>
  <Text>Save</Text>
</Pressable>
```

**Sources:**

- [Accessibility](https://reactnative.dev/docs/accessibility)
- [Testing](https://reactnative.dev/docs/testing-overview)
</details>

<details>
<summary>How do you ensure that your React Native application is maintainable and scalable over time?</summary>

### How do you ensure that your React Native application is maintainable and scalable over time?

**Definition:**

Keep modules small and tested, stay on the New Architecture and Hermes defaults, and ship behavior changes through reviewed release builds.

**How it works:**

The testing guide's loop is a failing test, then a fix, so regressions stay caught. Separate view from logic. Type-check with the TypeScript setup the template includes, including `StyleSheet.create`. Avoid new legacy bridge modules. Profile release frame rates before adding work on the JavaScript thread. Secrets stay out of the repo and the bundle.

**Examples:**

```tsx
it('given a past date, colorForDueDate returns red', () => {
  expect(colorForDueDate('2000-10-20')).toBe('red');
});
```

**Sources:**

- [Testing](https://reactnative.dev/docs/testing-overview)
- [About the New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page)
- [Using Hermes](https://reactnative.dev/docs/hermes)
- [Security](https://reactnative.dev/docs/security)
</details>
