# Next.js — Interview

Next.js interview questions.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/).

### Next.js Fundamentals

<details>
<summary>What is Next.js and what are its main features?</summary>

### What is Next.js and what are its main features?

**Definition:**

**Next.js** stands out as a production-grade React framework, offering both speed and specialization. Its versatility, data fetching techniques, search engine optimization (SEO), and various deployment options make it a top choice for modern web development.

**How it works:**

**Main Features**

1. **Server-Side Rendering (SSR)**: Next.js dynamically generates the HTML on the server for each page request, delivering consistently rendered content to users and ensuring optimal SEO.

2. **Static Site Generation (SSG)**: By pre-computing the HTML of individual pages at the build time, Next.js achieves superior performance and security. It's especially adept at handling content-heavy websites.

3. **Hybrid Rendering**: Combining SSR and SSG, Next.js provides the flexibility to render specific pages during build time (SSG) and others on-demand (SSR). This feature is valuable when creating applications comprising both dynamic and static content.

4. **Routing**: Opt for either filesystem-based or dynamic routing. The former obviates the need for explicit route definition while the latter enables programmatic control. Both options synergize with SSG and SSR.

5. **Code Splitting**: Out of the box, Next.js identifies separate sections of your app and optimizes the bundling, dispatching only the necessary code (and resources) to clients, benefiting both performance and load times.

6. **Formidable Image Optimization**: Image components from Next.js automate image optimization, ensuring picture quality for varying devices while still upholding rapid load times.

7. **Instant Deployment**: Rely on services like Vercel for swift, straightforward deployments, A/B testing, and other advanced features.

8. **API Routes**: Embrace Next.js API routes for seamless incorporation of backend resources into your application.

9. **Built-in CSS Support**: With Next.js, you can utilize standard CSS files, CSS modules, or even preprocessors like Sass, all without additional setup.

10. **Vast Plugin Ecosystem**: Extend the functionality of your Next.js project by plugging into a wide array of solutions from the Next.js community.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>How does Next.js differ from Create React App?</summary>

### How does Next.js differ from Create React App?

**Definition:**

Let's look at the key differences between Next.js and Create React App.

**How it works:**

**Core Functionality**

- **Next.js**: Built-in server-side rendering (SSR) and client-side rendering (CSR). It also supports static site generation (SSG). Offers features like pre-fetching data to enhance performance. Provides integrated API routing.
- **Create React App**: Primarily focuses on client-side rendering (CSR). Doesn't provide built-in server-side rendering or advanced rendering modes such as SSG or ISR. Relies on third-party solutions for server-side rendering.

**Routing**

- **Next.js**: Offers file-system-based routing, which simplifies route definition. It also supports custom routes. Dynamic routing and **code splitting** are automatic, enhancing performance.
- **Create React App**: Utilizes client-side routing with packages like React Router. Routes need to be explicitly defined in a configuration file or as components.

**File Structure and Configuration**

- **Next.js**: Employs a zero-configuration model for quick setup. It comes with sensible defaults that promote best practices. Added configuration options can further customize its behavior.
- **Create React App**: Similar to Next.js, it uses a zero-configuration approach. Customizations are managed via `react-scripts` and some advanced, but extensive, `eject` configurations. This step irrevocably detaches from the default configuration, making it harder to integrate future updates.

**API Handling**

- **Next.js**: Integrates seamless serverless API routes. Developers can design API endpoints using standard HTTP requests. The result is a simplified backend setup without the need for server management or additional server logic.
- **Create React App**: Leverages libraries like Axios or Fetch to interact with backend services. Unlike Next.js, it doesn't have a built-in solution for server-side API routes.

**Code Splitting**

- **Next.js**: Automatically code-splits imports across pages and components. This on-demand loading optimizes initial page load times. Furthermore, module-level or even granular control over code splitting are possible.
- **Create React App**: Incorporates the `React.lazy` function and `Suspense` components to facilitate code splitting. Developers need to identify points for code splitting manually.

**Data Fetching**

- **Next.js**: Offers numerous approaches for data retrieval, such as `getStaticProps`, `getStaticPaths`, `getServerSideProps`, and client-side methods. These functions are part of the **Data Fetching API** and are tailored for specific rendering strategies.
- **Create React App**: Lacks specialized data-fetching techniques inherent in Next.js. Instead, it integrates with data-fetching libraries or employs traditional strategies such as using `useEffect` in functional components.

**Deployment**

- **Next.js**: Provides flexible deployment options. With serverless hosting, deploys are efficient and scalable, especially suited for small to medium projects. Traditional hosting with a Node.js server allows for more extensive customizations.
- **Create React App**: Most suitable for static or single-page applications, often deployed via content delivery networks (CDNs). For dynamic content, it needs to be paired with a backend like a REST API, and then deployed as two distinct applications.

**SEO and Meta-Tags**

- **Next.js**: Offers superior support for search engine optimization (SEO). It covers foundational aspects like meta tags, but its advanced rendering modes, such as SSG and ISR, significantly benefit SEO, which is especially critical for content-heavy sites.
- **Create React App**: Lacks built-in mechanisms like SSR or specialized rendering modes for optimizing SEO. Developers have to leverage third-party tools or implement their own solutions to achieve search engine friendliness.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>What command is used to create a new Next.js app?</summary>

### What command is used to create a new Next.js app?

**Definition:**

To create a new **Next.js** application, you can utilize `npx` in combination with the `create-next-app` package.

**How it works:**

Replace **"folder-name"** with your desired workspace directory. Here, `npx` is a package runner that ensures you get the most recent version of `create-next-app`. If you're on Node.js 10 or earlier, you need to install `create-next-app` as a global package.

This method, involving `npm` and a global package, is not recommended for more recent Node.js versions.

**Examples:**

```bash
npx create-next-app folder-name
```

```bash
npm install -g create-next-app
create-next-app folder-name
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>How does Next.js handle server-side rendering (SSR)?</summary>

### How does Next.js handle server-side rendering (SSR)?

**Definition:**

**Next.js** handles Server-Side Rendering (SSR) through a simple and intuitive mechanism.

**How it works:**

**Key Components**

- **Page**: Represents the specific route that's being accessed.
- **Route**: A user-defined endpoint corresponding to a specific page or screen.
- **App**: The root of a Next.js application, managing the overall setup and state.

**The SSR Process**

1. **Initial Request**: When a user accesses a specific route or page, Next.js intercepts this request.
2. **Data Fetching**: Any necessary page data is fetched, ensuring it's available before the page is rendered.
3. **SSR Render**: Next.js renders the requested page on the server and sends the generated content, including any fetched data, as a response to the user's initial request.

**Benefit: Immediate Data Display**

Since page content and data are rendered on the server and sent in the initial response, users see meaningful content faster, even for dynamic, data-driven pages.

**The Data-Hydration Stage**

After the initial server-side rendering, the client-side JavaScript takes over. This process involves:

- **Page Navigation**: Whenever the user navigates to a new page within the application.
- **Client-Side Routing**: The browser takes control over route changes, and Next.js manages the synchronization.
- **Data Fetching**: If required, Next.js triggers additional data fetching on the client side for subsequent state updates or dynamic content changes.

**Optimal User Experience**

- Initial Server Response: Delivers server-rendered content and necessary data in the first request, improving perceived loading times.
- Enhanced Dynamic User Experience: Seamlessly combines server-rendered content with client-side updates, optimizing page state and content.

**Examples:**

Here's the typical structure of a **Next.js** page:

```jsx
import Head from "next/head";

const SamplePage = ({ data }) => {
  return (
    <div>
      <Head>
        <title>Sample Page</title>
      </Head>
      <h1>Sample Content</h1>
      <p>{data}</p>
    </div>
  );
};

export async function getServerSideProps() {
  // Perform necessary data fetching
  let data = "Some server-rendered data";
  return { props: { data } };
}

export default SamplePage;
```

In this example:

- The data-fetching function, `getServerSideProps`, ensures the data is available before the page is rendered. This method is specifically for Server-Side Rendering in Next.js.
- The `data` prop, obtained from the data-fetching function, is passed to the `SamplePage` component before its initial rendering. This permits immediate display of relevant data.
- The `Head` component from `next/head` encapsulates metadata specific to the page, such as the title.

Here is the code:

```jsx
import Link from "next/link";

const Navigation = () => {
  return (
    <nav>
      <Link href="/about">
        <a>About</a>
      </Link>
      <Link href="/contact">
        <a>Contact</a>
      </Link>
    </nav>
  );
};

export default Navigation;
```

In this example:

- The `Link` component encapsulates anchor tags, ensuring user navigation within the application is efficient and considers both server-rendered content and any subsequent client-rendered updates.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Can you explain the purpose of the pages directory in a Next.js project?</summary>

### Can you explain the purpose of the pages directory in a Next.js project?

**Definition:**

The **`pages` directory** serves as your primary **route definition tool** in a **Next.js app**. Each file within it represents a unique URL.

**How it works:**

**Key Features**

- **Automatic Routing**: No custom routing is needed. Any file added to `pages` becomes a route.
- **File-Route Mapping**: The file structure under `pages` maps to specific routes. For instance, `pages/post/index.js` is associated with `/post`.
- **Dynamic Routes**: Files using square brackets like `[id].js` allow for dynamic parameters. For example, `pages/post/[id].js` is tied to `/post/:id`.

**Special Roles**

Some specific filenames or subdirectories have distinct routing roles.

- **`404.js`**: If present, Next.js uses this file to render a custom 404 page.
- **`_app.js`**: Sits at the top level. It is the master page that wraps other pages. Useful for including global styles or contexts.
- **`_document.js`**: Also on the top level, controls the server-rendered HTML document. Strong for continuous UI across pages.

**Examples:**

Consider the following file structure:

```plaintext
pages/
  ├── index.js
  │
  ├── post/
  │    └── [id].js
  │
  └── about.js
```

This structure corresponds to the following routes:

- **/index.js**: Serves as the landing page.
- **/post**: Serves the `post` route. Should contain code to handle non-`id` requests.
- **/about.js**: Direct route to the about page and is also treated as a route.
- **/post/abc**:
  - Maps to `pages/post/[id].js` with `id` as "abc".

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>In Next.js, how do you create a page that is rendered on the server for every request?</summary>

### In Next.js, how do you create a page that is rendered on the server for every request?

**Definition:**

In Next.js, you can configure server-side rendering for pages that **always need fresh data**. A good example is a real-time dashboard or a data-driven landing page.

**How it works:**

**Server-Side Rendering (SSR) vs. Static Generation (SG)**

- **SSR**: This sends the request to the server on every visit to fetch fresh data and generate the HTML response. This approach is often necessary if the data changes frequently.
- **SG**: All requests serve pre-built HTML, ideal for content that doesn't change upon every visit. However, SG could become inappropriate for certain use-cases where real-time data is a requirement.

**Implementing SSR in Next.js**

To enable SSR in Next.js, follow these practical steps:

1. **Configure Your Page's Component**:
   - Along with the standard static `getStaticProps` method, include `getServerSideProps` for server-side rendering.
   - This method runs on **every request**, making it suitable for dynamic data.
   - Ensure you **export** this method within your page component.
2. **Using the Data**:
   - `getServerSideProps` should return the necessary data, as is common for data-fetching methods in Next.js.
   - Data will be available as props to your page component.
3. **Deploy on a System that Supports SSR**:
   - Next.js delivers SSR out-of-the-box, which means you do not need to change any configurations to support this concern.
   - For efficiency, you may utilize a **caching mechanism** to shape data that stays constant between subsequent requests.

**Examples:**

Here is the code:

1. **Page Component**: Pages/ServerSide.js

```javascript
export default function ServerSide({ time }) {
  return <p>Current time: {time}</p>;
}

export async function getServerSideProps() {
  return {
    props: {
      time: new Date().toISOString(),
    },
  };
}
```

This code provides a `time` prop that updates on each request, showcasing server-side rendering.

2. **Linking in Your Application**: ParentComponent.js

```javascript
import Link from "next/link";

export default function ParentComponent() {
  return (
    <div>
      <Link href="/ServerSide">Live Time Page</Link>
    </div>
  );
}
```

Users see the updated time each time they access the "Live Time Page."

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>What file extensions does Next.js support for pages?</summary>

### What file extensions does Next.js support for pages?

**Definition:**

**Next.js** allows great flexibility in defining page components, supporting a variety of file extensions.

**How it works:**

**Supported Extensions**

- `.js`: Traditional JavaScript.
- `.jsx`: React with JavaScript.
- `.ts`: TypeScript.
- `.tsx`: React with TypeScript.

**Text formats**

- `.mdx`: MDX for markdown with JSX extensions, enabling interactivity.

**Styling Components**

- `.css`: Standard CSS.
- `.module.css`: CSS Modules for local scoping.
- `.scss`: SCSS, with its extended features and nesting.

**Data Fetching**

- `.fetch.js`: Client-side data fetching.
- `.fetch.ts`: Client-side TypeScript data fetching.

**Error Handling**

- `_error.*`: Special filename used to define custom error pages.

**Examples:**

- Page using `.jsx` and `.scss`

```jsx
// myPage.jsx
import React from "react";
import styles from "./myPage.module.scss";

const MyPage = () => {
  return <div className={styles.myPage}>Hello, SCSS!</div>;
};

export default MyPage;
```

- MDX file with React components and styling

```jsx
// myMDXPage.mdx
import MyComponent from '../components/MyComponent';
import './myMDXPageStyles.css';

# My MDX Page

<MyComponent />
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>How do environment variables work in Next.js?</summary>

### How do environment variables work in Next.js?

**Definition:**

**Environment variables** in Next.js are used to configure deployment-specific values.

**How it works:**

**Server, Build, and Client-side Env Vars**

- **Server**: Required before app/web server starts, e.g., API keys.
- **Build**: For settings during the build process.
- **Client**: For global client-side use. Ensure no sensitive information here.

**Configuring Environment Variables**

1. Create an `.env.local` file in the project's root, listing the key-value pairs, each on a separate line.
2. Access the defined variables in code using `process.env.<VARIABLE_NAME>`.
3. Secure sensitive information on platforms like Vercel, by using their environment variable management tools.
4. Establishing Default Values: By adding defaults in the code, Next.js ensures the app doesn't break if an environment variable is missing.

**Examples:**

```plaintext
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

```javascript
console.log(process.env.DB_HOST);
```

```javascript
const dbHost = process.env.DB_HOST || "localhost";
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>What is Automatic Static Optimization in Next.js?</summary>

### What is Automatic Static Optimization in Next.js?

**Definition:**

**Automatic Static Optimization** in **Next.js** enables pages to be automatically prerendered to static HTML as long as they do not fetch data from an external source.

This technique eliminates the need for runtime server rendering when a page only relies on client-side data.

**How it works:**

**How It Works**

1. **Page Analysis**: During build time, Next.js analyzes each page to determine if it fetches data. If not, the page is marked for static optimization. This often includes pages that only use internal state, client-side libraries, or context.
2. **Prerendering**: For pages marked as static, Next.js generates HTML files during the build process. These optimized pages are then served without requiring server-side rendering.
3. **Data Revalidate**: Static pages can contain stale data. Next.js uses a revalidate strategy to periodically refresh data. This is an ideal trade-off for many applications, balancing performance and data accuracy.

**Use Cases**

- **Content Pages**: Ideal for content-rich pages, such as blogs or marketing content, where the data doesn't change frequently and a slight delay in updates is acceptable.
- **Marketing Campaigns**: For promotional campaigns or one-time events, where real-time data isn't a priority and quick initial page loads are crucial.

**Caching and Limitations**

- **Client Cache**: Since individual users might be served cached static pages, client-side data can still become out-of-date. Techniques like incremental static regeneration (ISR) offer better freshness guarantees by updating pages at specified intervals.
- **Data Dependencies**: Pages dependent on fresh, external data need server-side or hybrid rendering. Otherwise, they risk serving outdated content.

**Examples:**

```javascript
// pages/product/[id].jsx

// Using getStaticProps for static optimization
export async function getStaticProps({ params }) {
  const product = await someFetchFunction(params.id);
  return {
    props: { product },
    revalidate: 10, // Regenerate every 10 seconds for a more updated page
  };
}

// If real-time data is an absolute must, use getServerSideProps instead
export async function getServerSideProps({ params }) {
  const product = await someFetchFunction(params.id);
  return { props: { product } };
}

function Product({ product }) {
  // Render product details
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

### Pages and Routing

<details>
<summary>How does file-based routing work in Next.js?</summary>

### How does file-based routing work in Next.js?

**Definition:**

File-based routing in Next.js simplifies the organization of web application pages. By adhering to specific file naming conventions in tandem with dedicated folders, developers can seamlessly structure their Next.js projects.

**How it works:**

**Mechanism**

The presence of **JavaScript or Markdown files** under specific directories signals Next.js to configure pages within the application. For instance, navigational links can be established using file directory structure.

**Key Directories**

- The `pages` directory serves as the **root** where the application pages are located.
- Secondary folders, such as `pages/blog`, represent **sections** within the application.

**File Formats**

- For a standard page, use a `.js` or `.jsx` extension, or `.ts`/`.tsx` for TypeScript.
- For blog or documentation-like sections, `.mdx` (Markdown and React hybrid) can be employed, allowing content to render alongside components.

**Pluses and minuses:**

**Benefits**

- **Straightforward Navigation**: The project's structure in the `pages` directory mirrors the website's structure.
- **Modularity**: Sections are self-contained in separate directories, contributing to a more manageable and structured codebase.
- **Isomorphism Support**: The presence of both client- and server-side code fosters web applications that run on both the client and server, enhancing SEO and initial load times.

**Limitations**

- **Limited to Core Directories**: Unique configurations might necessitate stepping beyond the capabilities of file-based routing, demanding additional setup.
- **Directory Depth Complexity**: Managing a large number of deeply nested directories can be challenging, potentially triggering comprehension or performance issues.

**Examples:**

Consider a simple e-commerce website.

Below is the directory structure:

```plaintext
pages/
|__ index.js
|__ cart.js
|__ products/
     |__ index.js
     |__ [product-id].js
```

In the above structure, we see:

- `pages/index.js`: Serves as the landing page.
- `pages/cart.js`: Represents the shopping cart accessible via `yoursite.com/cart`.
- `pages/products/index.js`: Defines the 'Products' landing page.
- `pages/products/[product-id].js`: Corresponds to a product details page, accessible with the product's unique identifier, for example, `yoursite.com/products/123`.

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>How do you create dynamic routes in Next.js?</summary>

### How do you create dynamic routes in Next.js?

**Definition:**

**Dynamic Routes** in **Next.js** allow you to render pages or components based on URL parameters. This feature is useful for scenarios involving user dashboards, blog posts, or product pages, where content is tied to specific, dynamic URLs.

**How it works:**

**Route Configuration**

- **File Naming Structure**:
  - For top-level dynamic routes, depending on whether it's a page or a client-only route, use `[param].js` or `[param].client.js` respectively. For parameterized routes nested under a directory, the filename format is `[[...slug]].js`.
- **Route** and **Query Parameters**:
  - To capture route parameters, use `[param]` for a single parameter or `[[...slug]]` for chains of parameters.
  - Additionally, you can use the `useRouter` hook or `router` object to extract query parameters like a typical URL query.

The URL `/post/abc` would render "Post: abc".

**URL Configuration**

- **Grouped Page Paths**: To group a set of dynamic routes under a shared path segment, define a parameterized `pages/[category]/[id].js` page:
  - Inner pages like `/food/sandwich`, `/drinks/cola`, and so on would be caught within that route.
- **Shallow Routing**: Shallow routing, enabled with the `shallow` option of **`router.push`** or **`next/link`**, keeps query data when navigating:
  - For instance, when on `/view?user=48&page=1` and clicking on `<Link href='/view?user=12'>`, the URL remains `/view?user=12&page=1`.

**Tool Recommendations**

- **Visual Studio Code** with the **Path Autocomplete** and **vscode-react-docgen** extensions streamlines route and query param management.

**Examples:**

Here is the Next.js and React code:

```jsx
// pages/post/[id].js
import { useRouter } from "next/router";

export default function Post() {
  const router = useRouter();
  const { id } = router.query;

  return <div>Post: {id}</div>;
}
```

Here is the Next.js and React code:

```jsx
// pages/view.js
import Link from "next/link";

export default function View() {
  return (
    <div>
      <Link href={{ pathname: "/view", query: { user: 10 } }} shallow>
        <a>User 10</a>
      </Link>
    </div>
  );
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Explain how to access URL parameters in a dynamic route.</summary>

### Explain how to access URL parameters in a dynamic route.

**Definition:**

**Next.js** simplifies the process of handling dynamic routes and URL parameters. Here are the steps to access URL parameters in a dynamic route.

**How it works:**

**1. Define a Dynamic Route**

For instance, create a dynamic route as `pages/post/[id].js` where `id` denotes the unique identifier you'll extract as the parameter.

**2. Access the Parameters**

To access the parameter, use `router.query` in your component file.

**Use-Cases for Dynamic Parameters**

- **Fetching Content**: Ideal for retrieving specific content from a database.
- **Pagination**: Useful for navigating through multiple pages of data.
- **SEO Optimization**: Offers cleaner and more descriptive URLs.
- **Custom URL Structures**: Great for creating custom URL structures catering to unique requirements.

**Examples:**

Here is the code snippet:

```javascript
import { useRouter } from "next/router";

const DynamicPost = () => {
  const router = useRouter();
  const { id } = router.query;

  return <p>Post ID: {id}</p>;
};

export default DynamicPost;
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>Describe the functionality of the Link component in Next.js.</summary>

### Describe the functionality of the Link component in Next.js.

**Definition:**

The **`Link`** component in **Next.js** simplifies client-side navigation within your application, enhancing performance and user experience.

**How it works:**

**Key Features**

- **Prefetching**: `Link` automatically pre-caches linked pages that the user is likely to visit next. This optimizes load times during navigation.
- **Accessibility**: Assists with navigational cues for screen readers and keyboard users.
- **Code Splitting**: Next.js employs this strategy by default, loading only the JavaScript code necessary for the specific page being visited.
- **Intelligent Routing**: Handles link clicks gracefully, enhancing the user experience by providing polished navigational effects.

In the following `Example.tsx`, you can see how Next.js handles code splitting under the hood.

The `AboutUs` component is only loaded when the user navigates to that specific route, showcasing effective code splitting.

**When to use:**

The **`Link`** component excels for in-app navigations, particularly when:

- Directing users from one section of an app to another.
- Positive feedback is needed on user actions, such as in form submission success screens.

However, it's not the ideal choice for:

- Opening links in new browser tabs/windows.
- Managing dynamic state or side-effects.

**Examples:**

```tsx
import Link from "next/link";

const Home = () => {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/about">
        <a>About Us</a>
      </Link>
      {/* Other navigational links */}
    </div>
  );
};

export default Home;
```

```tsx
const AboutUs = () => {
  return (
    <div>
      <h1>About Us</h1>
      {/* About Us content */}
    </div>
  );
};

export default AboutUs;
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>How do you handle catch-all routes in Next.js?</summary>

### How do you handle catch-all routes in Next.js?

**Definition:**

**Catch-All Routes** in **Next.js** allow dynamic URL matching with flexible path parameters, empowering your app's routing system.

**How it works:**

**Key Features**

- **Dynamic Parameters**: Access route wildcard segments through the `...` rest parameter.
- **Matching Order**: Definition order determines the route precedence.
- **Fallback behavior**: Optionally configure fallback settings.

**Configuration**

Next.js routing file (`pages/foo/[...bar].js`):

- **Root Catch-All**: Load for any unmatched path or `/foo` route without further subpaths.
- **Nested Catch-Alls**: Triggered when the root path is accessed along with subdirectories.
- **Fallback Pages**: Ideal for error handling or awaiting dynamic data.

**Fallback Modes**

Specify fallback behavior in the `getStaticPaths` method:

- **True**: Generates paths at build time and handles missing routes as fallbacks (SSG-only).
- **False**: Precisely predefines page paths during build (no fallback).
- **'blocking'**: Handles fallbacks via server at run-time (SSR or SSG).

**Examples:**

```jsx
export default function CatchAll() {
  // Logic for Root Catch-All
}
```

```jsx
export default function NestedCatchAll() {
  // Logic for Nested Catch-All
}
```

```js
export async function getServerSideProps() {
  // Data-fetching logic
  return { props: { data } };
}

export default function Fallback({ data }) {
  return <div>{data}</div>;
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

### Data Fetching and Rendering

<details>
<summary>What is getStaticProps and when would you use it?</summary>

### What is getStaticProps and when would you use it?

**Definition:**

**`getStaticProps`** in Next.js allows you to **pre-render** a page at **build time** by fetching data from an API, database, or any other data source.

However, the static page isn't rebuilt until you trigger a **redeployment**. This makes `getStaticProps` beneficial when content doesn't require frequent updates and can be cached for improved performance.

By choosing **when** content is updated based on a request or a rebuild, you can optimize your build and improve on tasks such as **SEO**.

**When to use:**

- **Content Persistence**: For content that doesn't change often, like a blog post, where you don't need to rebuild the page every time there's a change.
- **Data Fetching**: When data is fetched from a headless CMS, internal API, external services, or databases that are not linked in real-time.
- **Performance**: To benefit from caching and serve pages faster, especially for content that's accessed frequently.

**Examples:**

Here is the JavaScript code:

```javascript
export async function getStaticProps() {
  // Fetch data from an API
  const res = await fetch("https://example.com/data");
  const data = await res.json();

  // Pass data to the page via props
  return {
    props: { data },
    // Re-generate the page every 60 seconds (optional)
    revalidate: 60,
  };
}

export default function MyStaticPage({ data }) {
  // Render data on the page
  // ...
}
```

**Sources:**

- [Devinterview.io — Next.js](https://devinterview.io/questions/web-and-mobile-development/next-interview-questions/)

</details>

<details>
<summary>What is getServerSideProps and what kind of renderin<details>
<summary>What is getServerSideProps and what kind of rendering does it enable?</summary>

### What is getServerSideProps and what kind of rendering does it enable?

**Definition:**

`getServerSideProps` is a Pages Router function. Export it from a page and Next.js runs it on the server for every request, then uses the returned `props` to render that page’s HTML. That is server-side rendering (SSR): the HTML is produced at request time, not at `next build`.

**How it works:**

- It can be exported only from a page. It runs on the server and its return value is JSON.
- A visit renders initial HTML from that data. The same `props` are embedded so the page can hydrate. Do not put secrets in `props`.
- A client navigation through `next/link` or `next/router` asks the server to run `getServerSideProps` again.
- You can call a CMS, database, or third-party API directly. You do not have to go through a Next.js API route.
- A thrown error renders `pages/500.js` in production. In development you get the error overlay.
- You may set `Cache-Control` on `res` (for example `s-maxage` and `stale-while-revalidate`). The docs still recommend `getStaticProps` with ISR when the HTML can be cached.

**When to use:**

- The page depends on data that exists only at request time, such as an `Authorization` header or geolocation.
- Prefer `getStaticProps` when the data can be prerendered and cached.

**Examples:**

```jsx
export async function getServerSideProps() {
  const res = await fetch("https://api.github.com/repos/vercel/next.js");
  const repo = await res.json();
  return { props: { repo } };
}

export default function Page({ repo }) {
  return <p>{repo.stargazers_count}</p>;
}
```

In the App Router the same request-time fetch is an async Server Component with `cache: 'no-store'`. There is no `getServerSideProps` export.

**Sources:**

- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
- [Next.js — App Router migration](https://nextjs.org/docs/app/guides/migrating/app-router-migration)

</details>

<details>
<summary>How would you implement Incremental Static Regeneration (ISR) in Next.js?</summary>

### How would you implement Incremental Static Regeneration (ISR) in Next.js?

**Definition:**

ISR updates a prerendered page without rebuilding the whole site. Most requests are served from the static cache. After the revalidate window, the next request still gets the stale page while Next.js regenerates it in the background.

**How it works:**

- Pages Router: return `revalidate` (seconds) from `getStaticProps`. On-demand, an API route calls `res.revalidate(urlPath)`. If you only use on-demand revalidation, omit `revalidate` (it defaults to `false`).
- A thrown error during background regeneration keeps the last successful page. The next request retries `getStaticProps`.
- App Router: export `revalidate` from the page (segment config) and prerender known params with `generateStaticParams`. On demand, call `revalidatePath` or `revalidateTag` from `next/cache`. `revalidateTag('posts', 'max')` is the current call shape in the docs.
- A `fetch` with `revalidate: 0` or `cache: 'no-store'` makes that route dynamic.
- ISR needs the Node.js runtime. It is not available with `output: 'export'`. The default file cache is per instance. Share it with a custom cache handler when you run more than one server.

**When to use:**

- Content that can be a few seconds or minutes stale, such as a blog or catalog.
- Use request-time rendering when the response must be unique per user.

**Examples:**

```jsx
export async function getStaticProps() {
  const post = await fetch("https://api.vercel.app/blog/1").then((res) =>
    res.json(),
  );
  return { props: { post }, revalidate: 60 };
}
```

App Router equivalent:

```jsx
export const revalidate = 60;

export default async function Page() {
  const posts = await fetch("https://api.vercel.app/blog").then((res) =>
    res.json(),
  );
  return <p>{posts.length}</p>;
}
```

**Sources:**

- [Next.js — ISR (Pages)](https://nextjs.org/docs/pages/building-your-application/data-fetching/incremental-static-regeneration)
- [Next.js — ISR (App Router)](https://nextjs.org/docs/app/guides/incremental-static-regeneration)

</details>

<details>
<summary>Can you use client-side data fetching in Next.js? If so, how?</summary>

### Can you use client-side data fetching in Next.js? If so, how?

**Definition:**

Yes. Client-side fetching loads data in the browser after the component mounts. The docs use it when the data does not need to be in the first HTML: no SEO requirement, no prerender, or the view must refresh often.

**How it works:**

- A `useEffect` fetch runs at mount. The component starts in a loading state, then renders when `setState` receives the JSON.
- That data is not part of the prerendered HTML and is not cached by Next.js the way `getStaticProps` is. The page shell can paint before the data arrives, and a slow request delays the content.
- The Next.js docs recommend [SWR](https://swr.vercel.app) for client fetches. It caches, revalidates, and refetches on focus.
- Mark the component with `'use client'` in the App Router. Server Components cannot call `useEffect`.

**When to use:**

- Dashboards, user-specific widgets, and data that changes faster than a static revalidate window.
- Keep public, indexable content on the server so the HTML already contains it.

**Examples:**

```jsx
import { useEffect, useState } from "react";

export function Profile() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("/api/profile-data")
      .then((res) => res.json())
      .then(setData);
  }, []);

  if (!data) return <p>Loading...</p>;
  return <h1>{data.name}</h1>;
}
```

**Sources:**

- [Next.js — Client-side fetching](https://nextjs.org/docs/pages/building-your-application/data-fetching/client-side)

</details>

<details>
<summary>Explain the 'fallback' key used in the getStaticPaths function.</summary>

### Explain the 'fallback' key used in the getStaticPaths function.

**Definition:**

`fallback` on the object returned from `getStaticPaths` decides what happens when a visitor requests a dynamic path that was not generated at `next build`.

**How it works:**

- `false`: only the `paths` you returned are built. Any other path is a 404. Add a path by running `next build` again.
- `true`: unknown paths are not a 404. The first request gets a fallback page (`router.isFallback` is true) while `getStaticProps` runs. Next.js then caches that path. Crawlers, and client navigations through `next/link` or `next/router`, skip the fallback and behave like `blocking`. `fallback: true` is not supported with `output: 'export'`.
- `'blocking'`: the request waits until `getStaticProps` finishes, then the full HTML is sent. There is no fallback UI.
- App Router replacement: `dynamicParams`. `true` (default) generates unknown params on demand. `false` returns 404. The docs say the old `'blocking'` versus `true` difference is covered by streaming, so `dynamicParams` has no `'blocking'` value.

**When to use:**

- `false` for a small, closed set of paths.
- `true` when you want a loading UI on the first visit.
- `'blocking'` when you would rather wait than show a fallback.

**Examples:**

```jsx
export async function getStaticPaths() {
  return {
    paths: [{ params: { id: "1" } }, { params: { id: "2" } }],
    fallback: "blocking",
  };
}
```

**Sources:**

- [Next.js — getStaticPaths](https://nextjs.org/docs/pages/api-reference/functions/get-static-paths)
- [Next.js — dynamicParams](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config/dynamicParams)

</details>

### API Routes

<details>
<summary>How do you create an API route in Next.js?</summary>

### How do you create an API route in Next.js?

**Definition:**

In the Pages Router, a file under `pages/api` becomes an endpoint at `/api/*`. The file exports a handler `(req, res)`. It is a server-only bundle and is not added to the client JavaScript.

**How it works:**

- `req` is an `http.IncomingMessage`. Helpers: `req.cookies` (default `{}`), `req.query` (default `{}`), `req.body` (parsed from `Content-Type`, or `null`).
- `res.status(code)`, `res.json(body)`, `res.send(body)`, `res.redirect([status,] path)` (default status `307`), and `res.revalidate(urlPath)`.
- Branch on `req.method` for `GET`, `POST`, and the rest.
- API routes are same-origin unless you add CORS headers. They cannot be used with a static export. `pageExtensions` in `next.config` applies to them.
- Export `config.api.bodyParser` to change the default `1mb` limit, or set `bodyParser: false` when you need the raw body (webhook signatures).
- App Router replacement: a Route Handler in `app/api/.../route.js` that exports `GET`, `POST`, and so on, and returns a `Response`.

**Examples:**

```js
export default function handler(req, res) {
  res.status(200).json({ message: "Hello from Next.js!" });
}
```

**Sources:**

- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
- [Next.js — Dynamic Route Handlers](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes)

</details>

<details>
<summary>Can you use middleware within Next.js API routes?</summary>

### Can you use middleware within Next.js API routes?

**Definition:**

A Pages API route is one handler function, not an Express stack. You can call plain functions before you write the response. Next.js also runs a project-level Proxy (the file used to be named `middleware`) before the route is rendered.

**How it works:**

- The `middleware` file convention is deprecated and renamed to `proxy`. Put `proxy.ts` next to `app` or `pages` and export a function named `proxy` (or a default export).
- Proxy can redirect, rewrite, change headers, or respond directly. Pass data onward with headers, cookies, a rewrite, a redirect, or the URL. Do not rely on shared globals.
- `export const config = { matcher: '/api/:path*' }` limits Proxy to API paths. Without a matcher, Proxy runs on every request, including `/_next/static` and files in `public`.
- Inside the route, shared checks are ordinary functions: read `req`, then `return` a response or continue. There is no `next()` argument on a Pages API handler.

**When to use:**

- Proxy for auth redirects, logging, and header edits that apply to many paths.
- A function inside the handler for logic that only that endpoint needs.

**Examples:**

```js
import { NextResponse } from "next/server";

export function proxy(request) {
  return NextResponse.redirect(new URL("/home", request.url));
}

export const config = { matcher: "/api/:path*" };
```

**Sources:**

- [Next.js — Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)
- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)

</details>

<details>
<summary>Explain how to handle query parameters in Next.js API routes.</summary>

### Explain how to handle query parameters in Next.js API routes.

**Definition:**

In a Pages API route, `req.query` is the parsed query string. It defaults to `{}`. Dynamic segments from the file name are placed on the same object.

**How it works:**

- `pages/api/post/[pid].js` receives `req.query.pid` for `/api/post/abc`.
- A catch-all `pages/api/post/[...slug].js` sets `slug` to an array: `/api/post/a/b` → `{ slug: ['a', 'b'] }`.
- An optional catch-all `[[...slug]].js` also matches the path with no extra segments, and then `req.query` is `{}`.
- A static file such as `pages/api/post/create.js` wins over `[pid]` and over `[...slug]`.
- App Router: read the query with the Web `URL` API, `new URL(request.url).searchParams`. Dynamic segments arrive as the promised `params` argument, not on `req.query`.

**Examples:**

```js
export default function handler(req, res) {
  const { pid } = req.query;
  res.status(200).send(`Post: ${pid}`);
}
```

**Sources:**

- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)

</details>

### Styling in Next.js

<details>
<summary>How can you include CSS modules in your Next.js application?</summary>

### How can you include CSS modules in your Next.js application?

**Definition:**

CSS Modules are files named `*.module.css`. Next.js turns each class into a unique name so two files can both use `.title` without colliding.

**How it works:**

- Import the file: `import styles from './blog.module.css'`.
- Put the generated name on `className={styles.blog}`.
- In production, Next.js merges stylesheets into code-split CSS files. Order follows import order. The docs say not to let a formatter reorder CSS imports if order matters.
- Global CSS and Tailwind belong in the root layout. Use a module for component-specific rules.

**Examples:**

```css
.blog {
  padding: 24px;
}
```

```jsx
import styles from "./blog.module.css";

export default function Page() {
  return <main className={styles.blog} />;
}
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)

</details>

<details>
<summary>Compare and contrast CSS-in-JS and styled-components in the context of Next.js.</summary>

### Compare and contrast CSS-in-JS and styled-components in the context of Next.js.

**Definition:**

CSS-in-JS writes styles in JavaScript. styled-components is one library in that family. Next.js also supports CSS Modules and Tailwind without a CSS-in-JS runtime. The Next.js compiler can compile styled-components, including its server render.

**How it works:**

- CSS Modules scope classes at build time and ship CSS files. A CSS-in-JS library usually creates class names when the component runs, which needs a server pass so the first HTML includes the CSS.
- For styled-components, set `compiler.styledComponents` in `next.config`. The docs list `ssr` (enabled by default), `displayName` (on in development, off in production unless you override it), `minify`, and `cssProp`.
- The CSS guide’s default advice is Tailwind for most UI and CSS Modules when you need authored CSS. Use CSS-in-JS when the team already styles with that library.

**When to use:**

- styled-components when you want colocated tagged templates and the compiler’s SSR option.
- CSS Modules when you want static CSS and no runtime style library.

**Examples:**

```js
module.exports = {
  compiler: {
    styledComponents: { ssr: true },
  },
};
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)
- [Next.js — compiler](https://nextjs.org/docs/architecture/nextjs-compiler)

</details>

<details>
<summary>Can global styles be used in Next.js? If so, how?</summary>

### Can global styles be used in Next.js? If so, how?

**Definition:**

Yes. Global CSS applies to the whole app. In the App Router you import it from the root layout so every route receives it.

**How it works:**

- Create `app/global.css` and `import './global.css'` in `app/layout.js`.
- The docs also allow a global stylesheet import from any layout, page, or component in `app`. Because of React’s stylesheet handling with Suspense, those files are not removed when you navigate, so they can clash. Keep truly global rules (resets, Tailwind) in the root import, and use CSS Modules for component CSS.
- Tailwind is a global import too: `@import 'tailwindcss'` in that CSS file, then import the file from the root layout.
- External packages (`import 'bootstrap/dist/css/bootstrap.css'`) can be imported in the App Router, including from the root layout.

**Examples:**

```jsx
import "./global.css";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)

</details>

### Performance Optimization

<details>
<summary>What is the Image component and how does it help optimize performance in Next.js?</summary>

### What is the Image component and how does it help optimize performance in Next.js?

**Definition:**

`next/image` renders an `<img>` and runs Next.js image optimization: sizing, format, and lazy loading, instead of shipping the original file unchanged.

**How it works:**

- Required props in the docs: `src` (path, remote URL, or static import) and `alt`.
- `width` and `height` set the rendered size. `fill` makes the image fill its parent instead.
- `sizes` tells the browser which width to use at each breakpoint. `quality` is an integer from 1 to 100. `loading` can be `lazy`. `placeholder="blur"` uses `blurDataURL`.
- `unoptimized` turns optimization off. `preload` preloads the image.
- Remote URLs still need to be allowed by your image config. Do not omit `alt`.

**Examples:**

```jsx
import Image from "next/image";

export default function Page() {
  return (
    <Image
      src="/profile.png"
      width={500}
      height={500}
      alt="Picture of the author"
    />
  );
}
```

**Sources:**

- [Next.js — Image](https://nextjs.org/docs/app/api-reference/components/image)

</details>

<details>
<summary>How does code splitting work in Next.js?</summary>

### How does code splitting work in Next.js?

**Definition:**

Next.js splits the client JavaScript by route so a visit downloads the code for that route, not the whole app. `next/dynamic` splits a component into its own bundle and loads it later.

**How it works:**

- `dynamic(() => import('../components/A'))` loads that component in a separate client bundle.
- Rendering it only after state changes loads it on demand.
- `{ ssr: false }` skips server rendering and loads the component only in the browser.
- The lazy-loading guide says automatic code splitting does not apply when a Server Component dynamically imports a Client Component. Put `dynamic` in a Client Component (`'use client'`) when you need that split.

**When to use:**

- Heavy widgets (charts, editors) that are not needed for the first paint.
- Leave small shared components on the default route split.

**Examples:**

```jsx
"use client";

import dynamic from "next/dynamic";

const Chart = dynamic(() => import("../components/Chart"), { ssr: false });

export function Dashboard() {
  return <Chart />;
}
```

**Sources:**

- [Next.js — Lazy loading](https://nextjs.org/docs/app/guides/lazy-loading)

</details>

<details>
<summary>What are the methods provided by Next.js for prefetching pages?</summary>

### What are the methods provided by Next.js for prefetching pages?

**Definition:**

`next/link` prefetches the linked route in the background so a later client navigation is faster. Prefetch runs only in production, when the link enters the viewport.

**How it works:**

- `prefetch` accepts `"auto"`, `null`, `true`, or `false`. `"auto"` and `null` are the default. Static routes prefetch the full route. Dynamic routes prefetch down to the nearest segment that has a `loading.js` boundary.
- `prefetch={true}` prefetches the full route for static and dynamic URLs.
- `prefetch={false}` disables viewport and hover prefetch.
- If prefetched data has expired by the time the user hovers, Next.js tries again.
- `useRouter` from `next/navigation` also has `prefetch`. With `typedRoutes: true`, that call is type-checked. It does not type `next/router` in the Pages Router.
- If Proxy rewrites the user to another page, the link must be told both the URL to show and the URL to prefetch, or the prefetch hits the wrong path.

**Examples:**

```jsx
import Link from "next/link";

export function Nav() {
  return (
    <Link href="/dashboard" prefetch={false}>
      Dashboard
    </Link>
  );
}
```

**Sources:**

- [Next.js — Link](https://nextjs.org/docs/app/api-reference/components/link)

</details>

### Deployment and Hosting

<details>
<summary>What are the benefits of deploying a Next.js application to Vercel?</summary>

### What are the benefits of deploying a Next.js application to Vercel?

**Definition:**

Vercel is the hosting platform from the same team as Next.js. A Next.js app deployed there runs the framework’s server features without you operating a Node process yourself.

**How it works:**

- `create-next-app` is the documented way to start an app (`yarn create next-app@latest my-app --yes` uses TypeScript, Tailwind, ESLint, the App Router, and Turbopack).
- Features that need a server are supported on a Node.js host: server rendering, Route Handlers, and ISR. The ISR guide’s platform table lists Node.js server and Docker as supported, and static export as not supported for ISR.
- On Vercel the platform runs that server for you, including background regeneration. Self-hosting the same features means you run `next build` and `next start` (or a Docker image) and, with more than one instance, a shared cache handler so on-demand revalidation is not stuck on one machine.

**When to use:**

- Vercel when you want the platform to run the Next.js server.
- Node or Docker when the app must live on your own infrastructure. ISR still works there.

**Sources:**

- [Next.js — Installation](https://nextjs.org/docs/app/getting-started/installation)
- [Next.js — ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration)

</details>

<details>
<summary>How do you configure custom domain names when deploying a Next.js application?</summary>

### How do you configure custom domain names when deploying a Next.js application?

**Definition:**

A custom domain is a DNS and host setting. Next.js does not issue domains. The host that serves the deployment maps the hostname to the app.

**How it works:**

- On Vercel you add the domain in the project settings and point DNS at the records Vercel shows. The Next.js app code stays the same.
- On your own server, the reverse proxy (or load balancer) terminates TLS for that hostname and forwards to the Node process from `next start`.
- Inside the app, absolute URLs and callbacks must use the public origin. Prefer the request host or an environment variable you set in that deployment, and keep `metadataBase` (when you set metadata) on that origin.
- Internationalized hosts and security headers are separate: `headers()` in `next.config` can vary by path, and the Pages `i18n` config can prefix locales onto those sources.

**Sources:**

- [Next.js — headers](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [Next.js — ISR platform support](https://nextjs.org/docs/app/guides/incremental-static-regeneration)

</details>

<details>
<summary>Explain how to deploy a Next.js app using a different hosting service, such as AWS or Netlify.</summary>

### Explain how to deploy a Next.js app using a different hosting service, such as AWS or Netlify.

**Definition:**

Any host that can run Node.js can serve a Next.js production build. Static export is a narrower option and drops features that need a server.

**How it works:**

- Build with `next build`. Start with `next start` (or `node server.js` only if you maintain a custom server). Set `NODE_ENV=production` for the start command, as the custom-server guide does.
- The ISR guide says a Node.js server and a Docker container both support ISR. Static export does not. Adapters are platform-specific.
- A custom server file is not traced by `output: 'standalone'`. Standalone emits its own `server.js`. Do not combine that mode with a hand-written custom server.
- Put the container or Node process behind TLS. If several instances serve ISR, configure a shared cache handler. On-demand revalidation only clears the instance that receives the call when the cache is the local filesystem.

**Examples:**

```json
{
  "scripts": {
    "build": "next build",
    "start": "next start"
  }
}
```

**Sources:**

- [Next.js — Custom server](https://nextjs.org/docs/pages/guides/custom-server)
- [Next.js — ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration)

</details>

### Integrations and Plugins

<details>
<summary>How do you integrate a CSS framework like Tailwind CSS into a Next.js project?</summary>

### How do you integrate a CSS framework like Tailwind CSS into a Next.js project?

**Definition:**

The current Next.js CSS guide installs Tailwind CSS v4 with the PostCSS plugin `@tailwindcss/postcss`, then imports it from the global stylesheet.

**How it works:**

- `npm install -D tailwindcss @tailwindcss/postcss`
- `postcss.config.mjs` sets `plugins: { '@tailwindcss/postcss': {} }`
- The global CSS file contains `@import 'tailwindcss';`
- Import that CSS file from the root layout. Utility classes such as `className="text-4xl font-bold"` then work.
- `create-next-app --yes` already includes Tailwind.
- The older `content` array (`./app/**/*.{js,ts,jsx,tsx}`) appears in the App Router migration guide for the previous Tailwind setup. The v4 guide above does not use that array.

**Examples:**

```js
export default {
  plugins: {
    "@tailwindcss/postcss": {},
  },
};
```

**Sources:**

- [Next.js — CSS](https://nextjs.org/docs/app/getting-started/css)
- [Next.js — App Router migration](https://nextjs.org/docs/app/guides/migrating/app-router-migration)

</details>

<details>
<summary>What are some popular plugins for Next.js and what functionalities do they add?</summary>

### What are some popular plugins for Next.js and what functionalities do they add?

**Definition:**

Next.js does not have a plugin registry inside the framework. You extend it with npm packages and with options in `next.config`. The compiler and TypeScript integration are the built-in extension points the docs describe.

**How it works:**

- `compiler.styledComponents` turns on the styled-components transform (`ssr`, `displayName`, `minify`, and the other flags in the compiler reference).
- TypeScript is built in. `next dev` / `next build` install types and write `tsconfig.json` when you add a `.ts` or `.tsx` file. A plugin inside the editor warns on invalid segment config and on `'use client'` mistakes.
- `typedRoutes: true` generates route types for `next/link` and for `next/navigation` methods.
- Tailwind is integrated as PostCSS, not as a Next plugin object. See the CSS guide.
- MDX, auth, and similar tools are separate packages. Add only a package whose own docs match your Next.js version. Do not invent a `plugins: []` array. The config object in the docs is `NextConfig`.

**Sources:**

- [Next.js — compiler](https://nextjs.org/docs/architecture/nextjs-compiler)
- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

<details>
<summary>How do you enable TypeScript support in a Next.js project?</summary>

### How do you enable TypeScript support in a Next.js project?

**Definition:**

Next.js has built-in TypeScript. `create-next-app` sets it up. An existing JavaScript app opts in by renaming a file to `.ts` or `.tsx`.

**How it works:**

- Run `next dev` or `next build`. Next.js installs the dependencies and adds a `tsconfig.json` with the recommended options.
- If you already have `jsconfig.json`, copy its `paths` into `tsconfig.json` and delete `jsconfig.json`.
- `next-env.d.ts` is generated and must stay in the `include` array. Do not edit it. The docs say to gitignore it.
- `next build` fails on type errors unless `typescript.ignoreBuildErrors` is `true`. The docs mark that flag as dangerous. Prefer `tsc --noEmit` in CI.
- For TypeScript 7, install `typescript@^7` in the project. Next.js uses the local `tsc` CLI. Set `experimental.useTypeScriptCli` to `false` to use the JavaScript compiler API instead.

**Examples:**

```bash
npm install -D typescript@^7
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

### Advanced Features

<details>
<summary>How do you implement a custom server in Next.js and why would you do so?</summary>

### How do you implement a custom server in Next.js and why would you do so?

**Definition:**

`next start` is the normal production server. A custom server calls `next()` yourself and passes requests to `app.getRequestHandler()`. The docs say to do this only when the built-in router cannot meet the requirement.

**How it works:**

- `next({ dev, hostname, port, dir, conf, quiet, turbopack, webpack, httpServer })`. `dev` defaults to `false`. `dir` defaults to `'.'`. `turbopack` defaults to enabled.
- `app.prepare()` then `createServer((req, res) => handle(req, res))`.
- Set your own headers before `handle`. After `handle` starts the response, `res.headersSent` is already true and a later `res.setHeader` is discarded.
- `server.js` is not compiled by Next.js. It must run on your Node version.
- Change `package.json` so `dev` and `start` run `node server.js`. Keep `build` as `next build`.
- `useFileSystemPublicRoutes: false` stops SSR of `pages` filenames when your server owns the paths. Client navigations can still hit them, so guard those routes.
- `output: 'standalone'` does not trace a custom server. It emits a different `server.js`. Do not use both.

**When to use:**

- You must attach Next.js to an existing HTTP server and the integrated router is not enough.
- An existing backend can still sit beside Next.js without a custom server. The guide calls that out specifically.

**Examples:**

```js
import { createServer } from "http";
import next from "next";

const port = parseInt(process.env.PORT || "3000", 10);
const dev = process.env.NODE_ENV !== "production";
const app = next({ dev });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  createServer((req, res) => {
    handle(req, res);
  }).listen(port);
});
```

**Sources:**

- [Next.js — Custom server](https://nextjs.org/docs/pages/guides/custom-server)

</details>

<details>
<summary>What are the considerations when setting up internationalized routing in Next.js?</summary>

### What are the considerations when setting up internationalized routing in Next.js?

**Definition:**

Pages Router `i18n` in `next.config` declares `locales` and `defaultLocale`. Next.js then prefixes routes with the locale. The App Router does not use that config object the same way. Locale lives in the path, usually a `[lang]` segment, and you read it from `params`.

**How it works:**

- With Pages `i18n`, a `headers()` source matches every configured locale unless you set `locale: false` and write the prefix yourself.
- The default locale is served at `/` as well as at its prefix, depending on the matcher. The headers reference shows `/en` matching `/` when `en` is `defaultLocale` and `locale: false`.
- A source of `/(.*)` becomes `/(en|fr|de)/(.*)` and does not match the bare `/` the way `/:path*` would. Test both.
- Keep one locale in the URL so links, `getStaticPaths` / `generateStaticParams`, and caches do not mix languages.
- Proxy can redirect `/` to a locale, but the docs say Proxy should stay a redirect or rewrite, not a place for shared rendering logic.

**When to use:**

- Pages `i18n` when the app is still on the Pages Router.
- An App Router `[lang]` segment for new apps, with `generateStaticParams` returning each locale.

**Examples:**

```js
module.exports = {
  i18n: {
    locales: ["en", "fr", "de"],
    defaultLocale: "en",
  },
};
```

**Sources:**

- [Next.js — headers and i18n](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [Next.js — Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)

</details>

<details>
<summary>How do you handle custom error pages in a Next.js application?</summary>

### How do you handle custom error pages in a Next.js application?

**Definition:**

The App Router uses `not-found.js` for a missing resource. Call `notFound()` from the page to render it. A thrown error inside Pages `getServerSideProps` renders `pages/500.js` in production.

**How it works:**

- `app/not-found.js` exports a component. The docs say a streamed response can come back with HTTP 200, while a non-streamed 404 stays 404.
- `experimental.globalNotFound: true` in `next.config` turns on an application-wide 404 that skips normal route rendering. The flag is experimental.
- Pages `getServerSideProps` errors use `pages/500.js`. During `next dev` that file is not used. You see the overlay instead.
- An ISR regeneration error does not replace the last good page. The next request tries again.

**Examples:**

```jsx
import Link from "next/link";

export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <Link href="/">Return Home</Link>
    </div>
  );
}
```

**Sources:**

- [Next.js — not-found](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)

</details>

### Best Practices

<details>
<summary>What folder structure is recommended for large-scale Next.js applications?</summary>

### What folder structure is recommended for large-scale Next.js applications?

**Definition:**

The App Router is file-system routing under `app/`. A route is a folder with a `page.tsx`. Shared UI can sit in colocated files that are not routes. Dynamic segments use brackets, as in `app/products/[id]/page.tsx`.

**How it works:**

- `app/layout.tsx` wraps child routes. `app/page.tsx` is `/`.
- Only `page`, `layout`, `route`, `not-found`, `loading`, and the other file conventions become routing files. A `components` folder next to them does not add a URL.
- `generateStaticParams` lives in the dynamic `page` or `route` that owns the segment.
- `proxy.ts` stays at the project root (or `src`), beside `app` or `pages`, not inside a route folder.
- The Pages Router equivalent is `pages/`. Do not mix two competing trees for the same URL.

**Examples:**

```txt
app/
├── layout.tsx
├── page.tsx
└── products/
    └── [id]/
        └── page.tsx
```

**Sources:**

- [Next.js — Building](https://nextjs.org/docs/app/guides/building)

</details>

<details>
<summary>How should you structure components in a Next.js application for optimal code reusability?</summary>

### How should you structure components in a Next.js application for optimal code reusability?

**Definition:**

Server Components are the default in `app`. They can be async and can fetch. A component that uses state or effects needs `'use client'` at the top of its file. Share presentational pieces by importing them. Do not copy fetch logic into every page.

**How it works:**

- Put a shared Server Component in something like `components/` and import it from several `page.tsx` files. Colocation is fine. A file is not a route unless it is a convention file.
- Push `'use client'` down to the leaf that needs interactivity so the parent can stay a Server Component.
- Load heavy client leaves with `next/dynamic` from a Client Component.
- Type shared route props with the generated helpers `PageProps` and `LayoutProps` instead of repeating param types.

**Examples:**

```jsx
export function Price({ cents }) {
  return <p>{(cents / 100).toFixed(2)}</p>;
}
```

**Sources:**

- [Next.js — Lazy loading](https://nextjs.org/docs/app/guides/lazy-loading)
- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

<details>
<summary>What are some security best practices you should follow in a Next.js app?</summary>

### What are some security best practices you should follow in a Next.js app?

**Definition:**

Treat the rendered props and the public env as data the browser can see. Keep secrets on the server. Add response headers for the policies you need.

**How it works:**

- `getServerSideProps` props are in the HTML so hydration works. The docs say not to pass sensitive data in `props`.
- A Server Component can read secrets while it runs on the server. Do not import that module into a Client Component.
- `headers()` in `next.config` sets response headers per `source`. Use it for a Content-Security-Policy and related policies. With Pages `i18n`, sources match locales unless `locale: false`.
- API routes are same-origin until you add CORS. Disable the body parser and verify the raw body for webhooks.
- On-demand ISR must check a secret before `res.revalidate` or `revalidatePath`. The Pages example compares `req.query.secret` to `process.env.MY_SECRET_TOKEN` and returns 401 otherwise.
- Proxy is for redirects and header changes, not for storing session data in a global.

**Examples:**

```js
module.exports = {
  async headers() {
    return [
      {
        source: "/(.*)",
        headers: [{ key: "X-Content-Type-Options", value: "nosniff" }],
      },
    ];
  },
};
```

**Sources:**

- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
- [Next.js — headers](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)

</details>

### State Management in Next.js

<details>
<summary>How do you manage global state in a Next.js application without using external libraries?</summary>

### How do you manage global state in a Next.js application without using external libraries?

**Definition:**

React context and `useState` are enough for client state that many components read. Server Components do not hold that state. They render on the server per request.

**How it works:**

- Create a Client Component that calls `useState` and passes the value through `React.createContext`. Export a provider and a hook.
- Render the provider inside the root layout, wrapping `{children}`, so navigations under that layout keep the same client state.
- Do not fetch private data into context during SSR and then assume it is secret. Whatever you pass into a Client Component is serialized to the browser.
- For data that should reset per request, read it in a Server Component and pass a plain prop. That is not global client state.

**When to use:**

- Theme, locale preference, or a small cart held in the tab.
- A database or ISR cache for data that must survive a reload.

**Examples:**

```jsx
"use client";

import { createContext, useContext, useState } from "react";

const ThemeContext = createContext("light");

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  return useContext(ThemeContext);
}
```

**Sources:**

- [Next.js — Lazy loading and Client Components](https://nextjs.org/docs/app/guides/lazy-loading)
- [React — createContext](https://react.dev/reference/react/createContext)

</details>

<details>
<summary>When should you consider using a state management library like Redux in a Next.js project?</summary>

### When should you consider using a state management library like Redux in a Next.js project?

**Definition:**

Add a client store when many distant Client Components update the same state and prop drilling or one context becomes hard to maintain. Next.js does not require Redux.

**How it works:**

- Server-rendered data should stay in Server Components, `fetch`, and the cache (`revalidate`, `revalidatePath`). Copying it into Redux duplicates the source of truth.
- A store runs in the browser. Mark the provider `'use client'` and mount it in the root layout if the whole app needs it.
- Context is the built-in option for a single value such as theme. A library pays off when you have many independent slices, middleware, or time-travel debugging.
- Do not put secrets or request-only tokens in a store that is sent to the client.

**When to use:**

- Large interactive clients with updates that do not map to a URL or a server fetch.
- Skip it for a marketing site whose data comes from ISR or a Server Component.

**Sources:**

- [Next.js — ISR](https://nextjs.org/docs/app/guides/incremental-static-regeneration)
- [React — createContext](https://react.dev/reference/react/createContext)

</details>

<details>
<summary>Explain how to persist state between page transitions in a Next.js app.</summary>

### Explain how to persist state between page transitions in a Next.js app.

**Definition:**

Client navigations with `next/link` do not reload the document. React state in a layout that stays mounted survives the transition. A full load wipes memory. Persist across loads with `localStorage` or a server store.

**How it works:**

- Put the provider in the root layout. Child `page.tsx` changes do not remount that provider, so `useState` there remains.
- State inside a `page.tsx` is lost when that page unmounts. Lift it to the layout if it must survive.
- `localStorage` is only available after mount. Read it in `useEffect` so the server render and the first client render match.
- Data that should be shared across devices belongs in a database and a Server Component, not in component state.
- Prefetch does not run your state logic. It loads the route. See the `Link` `prefetch` prop.

**Examples:**

```jsx
"use client";

import { useEffect, useState } from "react";

export function DraftName() {
  const [name, setName] = useState("");

  useEffect(() => {
    const saved = localStorage.getItem("draft-name");
    if (saved) setName(saved);
  }, []);

  return (
    <input
      value={name}
      onChange={(event) => {
        setName(event.target.value);
        localStorage.setItem("draft-name", event.target.value);
      }}
    />
  );
}
```

**Sources:**

- [Next.js — Link](https://nextjs.org/docs/app/api-reference/components/link)
- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

</details>

### Next.js and TypeScript

<details>
<summary>What are the advantages of using TypeScript in a Next.js project?</summary>

### What are the advantages of using TypeScript in a Next.js project?

**Definition:**

TypeScript checks props, route params, and config before `next build` ships the app. Next.js adds route-aware helpers and an editor plugin on top of the compiler.

**How it works:**

- `next build` runs type checking and fails the build on errors (`typescript.ignoreBuildErrors` opts out and is documented as dangerous).
- The editor plugin warns on invalid segment config and on client hooks used without `'use client'`.
- App Router Server Components can pass `Date`, `Map`, and `Set` without the Pages Router JSON serialization step. You type the fetch result with ordinary TypeScript.
- Generated helpers `PageProps`, `LayoutProps`, and `RouteContext` are global. `next typegen`, `next dev`, and `next build` write them.
- `typedRoutes: true` checks `href` on `next/link` and `push` / `replace` / `prefetch` from `next/navigation`.

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

<details>
<summary>How do you add type definitions for a Next.js page's props?</summary>

### How do you add type definitions for a Next.js page's props?

**Definition:**

In the App Router, type the page with the generated `PageProps` helper, or with an explicit `params` promise. In the Pages Router, use `GetServerSideProps` / `GetStaticProps` and `InferGetServerSidePropsType`.

**How it works:**

- `PageProps`, `LayoutProps`, and `RouteContext` are generated and need no import.
- Dynamic App Router params are a promise: `params: Promise<{ id: string }>`, then `const { id } = await params`.
- Pages `getServerSideProps` is typed as `GetServerSideProps<{ repo: Repo }>`. The page props use `InferGetServerSidePropsType<typeof getServerSideProps>`.
- `NextApiRequest` body is `any`. Validate it at runtime before use.

**Examples:**

```tsx
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  return <p>{id}</p>;
}
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)
- [Next.js — getServerSideProps](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)

</details>

<details>
<summary>What steps are involved in migrating an existing Next.js project to TypeScript?</summary>

### What steps are involved in migrating an existing Next.js project to TypeScript?

**Definition:**

Rename JavaScript files to `.ts` and `.tsx`, then let Next.js create `tsconfig.json`. Move path aliases, and fix the errors `next build` reports.

**How it works:**

- Rename a file to `.ts` or `.tsx`. Run `next dev` or `next build`. Next.js installs dependencies and writes the recommended `tsconfig.json`.
- Copy `compilerOptions.paths` from `jsconfig.json`, then delete `jsconfig.json`.
- Keep `next-env.d.ts` and `.next/types/**/*.ts` in `include`. Add your own `*.d.ts` for extra module declarations. Do not edit `next-env.d.ts`.
- Type pages and route handlers as you touch them. `next build` fails until the project type-checks, unless you set `typescript.ignoreBuildErrors`.
- Async Server Components need TypeScript 5.1.3 or newer and `@types/react` 18.2.8 or newer, or JSX typings reject a returned `Promise`.
- Incremental checking is available when `tsconfig` enables it (supported since Next.js 10.2.1).

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

<details>
<summary>Can you describe how to use TypeScript for defining API route handlers in Next.js?</summary>

### Can you describe how to use TypeScript for defining API route handlers in Next.js?

**Definition:**

Pages API routes type the handler with `NextApiRequest` and `NextApiResponse<Data>` from `next`. App Router Route Handlers use the Web `Request` and a generated `RouteContext` for dynamic params.

**How it works:**

- `NextApiResponse<ResponseData>` checks the object you pass to `res.json`.
- `req.body` stays `any`. Validate it before you trust fields.
- A dynamic App Router handler is `GET(request: Request, context: RouteContext<'/api/posts/[id]'>)`. `await` `context.params` (the docs destructure `{ params }`).
- `generateStaticParams` can prerender those handlers. IDs it returns are static. Other IDs run at request time.

**Examples:**

```ts
import type { NextApiRequest, NextApiResponse } from "next";

type ResponseData = { message: string };

export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseData>,
) {
  res.status(200).json({ message: "Hello from Next.js!" });
}
```

**Sources:**

- [Next.js — API Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)
- [Next.js — Dynamic routes](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes)

</details>

<details>
<summary>How do you enforce type checking for dynamic routes in Next.js using TypeScript?</summary>

### How do you enforce type checking for dynamic routes in Next.js using TypeScript?

**Definition:**

`typedRoutes: true` makes `next/link` `href` and App Router `router.push`, `replace`, and `prefetch` accept only real routes. Dynamic segments are part of that generated type.

**How it works:**

- Set `typedRoutes: true` in `next.config.ts`. The project must use TypeScript.
- `next dev`, `next build`, or `next typegen` writes `.next/types`. Include `.next/types/**/*.ts` in `tsconfig` `include` if you did not use `create-next-app`.
- String literals are checked (`href="/aboot"` errors). A non-literal string needs `as Route` from `next`.
- This does not type `next/router` in the Pages Router.
- Dynamic route handlers type params with `RouteContext<'/api/posts/[id]'>`. Page components can type `params` as `Promise<{ id: string }>`. `PageProps` is the generated global helper for pages.

**Examples:**

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = { typedRoutes: true };

export default nextConfig;
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

<details>
<summary>How does Next.js handle automatic type generation for pages and API routes when using TypeScript?</summary>

### How does Next.js handle automatic type generation for pages and API routes when using TypeScript?

**Definition:**

Next.js generates route types while `next dev`, `next build`, or `next typegen` runs. You do not import `PageProps`, `LayoutProps`, or `RouteContext`. They are global helpers.

**How it works:**

- `next-env.d.ts` references Next.js types so imports of CSS and images type-check. The file is regenerated. Do not edit it. Keep it in `include`.
- `.next/types` holds the route map used by `typedRoutes` and the segment helpers.
- `typescript.tsconfigPath` selects which `tsconfig` is passed to the checker. In development, only `tsconfig.json` is watched. Restart `next dev` after you edit another path.
- `experimental.typedEnv: true` generates editor types for environment variables loaded in development. It does not include `.env.production*` unless you run `next dev` with `NODE_ENV=production`.
- With the TypeScript 7 CLI checker, diagnostics are native `tsc` output. Next.js does not rewrite them into route-specific frames unless you set `experimental.useTypeScriptCli` to `false`.

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>

<details>
<summary>What is the role of the tsconfig.json file in a Next.js project with TypeScript, and how do you customize it for your specific needs?</summary>

### What is the role of the tsconfig.json file in a Next.js project with TypeScript, and how do you customize it for your specific needs?

**Definition:**

`tsconfig.json` is the TypeScript project Next.js type-checks. `create-next-app` writes one. `next dev` writes one the first time it sees a `.ts` file.

**How it works:**

- `include` must list `next-env.d.ts` and `.next/types/**/*.ts`, plus your source. `exclude` usually lists `node_modules`.
- Put custom ambient types in a separate `.d.ts` and add that file to `include`.
- `compilerOptions.paths` are your aliases. Copy them from `jsconfig.json` when you migrate.
- Incremental type checking is honored when the `tsconfig` enables it.
- To type-check with a different file in CI, set `typescript.tsconfigPath` (for example `tsconfig.build.json`). The IDE still reads `tsconfig.json`.
- `typescript.ignoreBuildErrors: true` skips the check during `next build`. Run `tsc --noEmit` yourself if you turn that on.

**Examples:**

```json
{
  "include": ["next-env.d.ts", ".next/types/**/*.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

**Sources:**

- [Next.js — TypeScript](https://nextjs.org/docs/app/api-reference/config/typescript)

</details>
