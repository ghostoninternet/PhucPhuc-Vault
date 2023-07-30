# Linking and Routing
## Website navigation
>Browse through various pages or links from a single component.
>1. The horizontal navigation bar is often referred to as a navbar
>2. The vertical navigation bar is known as a sidebar navigation
>3. The menu hiding behind the button is usually represented by an icon that has three horizontal line and is thus referred to as the burger icon or the burger menu. Alternative to this is the drop-down navigator menu, known as the mega menu.
>4. The footer navigation menu is usually displayed as several visual columns containing links. 
>![[HTML_nav_work.png]]
>![[React_nav_work.png]]

```JavaScript
/*Inside index.js*/
import {BrowserRouter} from 'react-router-dom';
<BrowserRouter> 
	<App/> 
</BrowserRouter>
```

```JavaScript
/*Inside App.js Component*/
import {Routes, Route, Link} from 'react-router-dom';
<Link to="Write the path name" className="Write the class name"> 
	Write the context 
</Link>
<Routes>
	<Route path="Write the path name" element={<Component Name/>}/>
</Routes>
```

>`ReactRouter`--> A library that gives you more control over the routing of component.

### Multi-page application
>Before the advent of modern JavaScript frameworks, most websites were implemented as multi-page applications. That is, when a user clicks on a link, the browser navigates to a new webpage, sends a request to the web server; this then responds with the full webpage and the new page is displayed in the browser.
>
>This can make your application resource intensive to the Web Server. CPU time is spent rendering dynamic pages and network bandwidth is used sending entire webpages back for every request. If your website is complex, it may appear slow to your users, even slower if they have a slow or limited internet connection.
>
>To solve this problem, many web developers develop their web applications as Single Page Applications.

### Single-Page Apps
>A Single Page Application allows the user to interact with the website without downloading entire new webpages. Instead, it rewrites the current webpage as the user interacts with it. The outcome is that the application will feel faster and more responsive to the user.
### How Does a Single-Page App Work ?
>When the user navigates to the web application in the browser, the Web Server will return the necessary resources to run the application. There are two approaches to serving code and resources in Single Page Applications.
>1. When the browser requests the application, return and load all necessary HTML, CSS and JavaScript immediately. This is known as **bundling**.
>2. When the browser requests the application, return only the minimum HTML, CSS and JavaScript needed to load the application. Additional resources are downloaded as required by the application, for example, when a user navigates to a specific section of the application. This is known as _lazy loading_ or _code splitting_.
![[Traditional_SPA_Lifecycle.png]]

## Anchor Tag Elements in Single-Page Elements
>A single-page application can’t have regular anchor tag elements as a traditional web app can.
>
>The reason for this is that the default behavior of an anchor tag is to load another HTML file from a server and refresh the page. This page refresh is not possible in a SPA that's powered by a library such as React because a total page refresh is not the way that a SPA works, as explained earlier in this lesson item.
>
>Instead, a SPA comes with its own special implementation of anchor tags and links, which only give an illusion of loading different pages to the end user when in fact, they simply load different components into a single element of the real DOM into which the virtual DOM tree gets mounted and updated.
![[SPA_example.png]]
![[Traditional_example.png]]

# Using Assets in React

### How to use assets ?
>1. Import an asset `import cat from './assets/images/cat.jpg'`
>2. Use an asset (**name reference** or **path reference** (with path reference must use **require(asset path)** ))

```JavaScript
/* name reference */
import cat from './assets/images/cat.jpg'

function showAnimal() {
	return(
		<div>
			<img src={cat} alt="A picture of cat"/>
		</div>
	);
}
export default showAnimal;
```

```JavaScript
/* path reference */
import cat from './assets/images/cat.jpg'

function showAnimal() {
	return(
		<div>
			<img src={require('./assets/images/cat.jpg')} alt="A picture of cat"/>
		</div>
	);
}
export default showAnimal;
```

>The app’s files will likely be bundled when working with a React app. Bundling is a process that takes all the imported files in an app and joins them into a single file, referred to as a **bundle**. Several tools can perform this bundling. **webpack** is the built-in tool for the `create-react-app`. Simply put, **webpack** is a module bundler. Practically, this means that it will take various kinds of files, such as SVG and image files, CSS and SCSS files, JavaScript files, and TypeScript files, and it will bundle them together so that a browser can understand that bundle and work with it.
>
>The imports here are from fictional libraries and resources because the specific libraries are not necessary. All these different imports can be of various file types: .js, .svg, .css, and so on. In turn, all the imported files might have their own imported files, and even those might have their imports. This means that depending on other files, all of these files can create a **dependency graph**. The order in which all these files are loading is essential. That dependency graph can get so complex that it becomes almost impossible for a human to structure a complex project and bundle all those dependencies properly. 
>---> This is the reason you need tools like **webpack**.
>
>So, webpack builds a dependency graph and bundles modules into one or more files that a browser can consume.
>
>Another significant characteristic of webpack is that it helps developers create modern web apps.
>It helps you achieve this using two modes: **production** mode or **development** mode.
>1. In **development** mode, webpack bundles your files and optimizes your bundles for updates - so that any updates to any of the files in your locally developed app are quickly re-bundled. It also builds source maps so you can inspect the original file included in the bundled code.
>2. In **production** mode, webpack bundles your files so that they are optimized for speed. This means the files are minified and organized to take up the least amount of memory. So, they are optimized for speed because these bundles are fast to download when a user visits the website online.
>
>This works great for smaller apps, but if you have a more extensive app, this approach is likely to affect your site’s speed. The longer it takes for a web app to load, the more likely the visitor will leave and move on to another unrelated website. There are several ways to tackle this issue of a large bundle.
>One such approach is **code-splitting**, a practice where a module bundler like webpack splits the single bundle file into multiple bundles, which are then loaded on an as-needed basis. With the help of code-splitting, you can **lazy load** only the parts that the visitor to the app needs to have at any given time. This approach significantly reduces the download times and allows React-powered apps to get much better speeds.
>An example of a viable alternative is **SSR (Server-side rendering)**. With SSR, React components are rendered to HTML on the server, and the visitor downloads the finished HTML code. An alternative to SSR is client-side rendering, which downloads the index.html file and then lets React inject its own code into a dedicated HTML element (the **root** element in `create-react-app`). In this course, you’ve only worked with client-side rendering.
>Sometimes, you can combine client-side rendering and server-side rendering. This approach results in what’s referred to as **isomorphic apps**.

### How to work wit Audio and Video
>1. Load a local video using `<video>` tag.
>2. Using specific instructions on how a given social media platform recommends that you embed the video and audio assets into website.
>3. Using Third-party NPM package.
>		How to choose the right one ?
>		1. Check the frequency of updates
>		2. Check GitHub page.
>		3. Search the package name.