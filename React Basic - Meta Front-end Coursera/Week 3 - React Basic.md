# Linking and Routing
## Website navigation
>Browse through various pages or links from a single component.
>1. The horizontal navigation bar is often referred to as a navbar
>2. The vertical navigation bar is known as a sidebar navigation
>3. The menu hiding behind the button is usually represented by an icon that has three horizontal line and is thus referred to as the burger icon or the burger menu. Alternative to this is the drop-down navigator menu, known as the mega menu.
>4. The footer navigation menu is usually displayed as several visual columns containing links. 
>![[HTML_nav_work.png]]
>![[React_nav_work.png]]
>React router library:
> `import {BrowserRouter} from 'react-router-dom';` (Inside `index.js`)
> Inside `index.js`: `<BrowserRouter> <App/> </BrowserRouter>`
> 
> `import {Routes, Route, Link} from 'react-router-dom'`; (Inside `App.js` component)
> `<Link to="Write the path name" className="Write the class name"> Write the context </Link>`
> 
> `<Routes> <Route path="Write the path name" element={<Component Name/>}></Route> </Routes>`
> 
>--> A library that gives you more control over the routing of component.

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
>1. Import an asset
>2. Use an asset (**name reference** or **path reference** (must use **require(asset path)** ))