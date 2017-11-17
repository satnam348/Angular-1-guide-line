## Internet Client - Server application architectures:

**The three basic forms of web based client-server architectures are:**

1. **Web Site**
1. **Web Application**
1. **Single Page Application _(SPA)_**


- **Web Site**
    -  a web site is a collection of html pages that are stored on a server and delivered to a browser client on request.
    -  a web site can be either static or dynamic.
        -  **Static** web sites had pre-formatted html files with related resources that are delivered as is on each request.
        -  **Dynamic** web sites have server code that compiles a page from a collection of resources and templates, combining this with user input to render a completed page. Each page is self-contained including all the html and references to all necessary resources ( css, images and scripts) needed to render the page properly within the browser application.  Each request is responded with a new page.
    -  Both static and dynamic Web site pages can contain some dynamic content that changes in the browser according to various stimuli, either environmentally (time, location, browse history) or from the user (data entry into widgets)


- **Web Application**
    - A Web application extends the architecture of the web site by adding state to each page to track the users interactions and input.  This state can be stored either locally using hidden elements in the page markup, in cookies, local or session storage within the browser, or on the server in a session object.
    - The user enters data into entry fields and controls on the page and then submits this data back to the server for pocessing.  If the application requires a multi-stage work flow the server will respond with a new page that has been rendered with new content according to the workflow, along with any state necessary to track the user.
    - For highly scalable Web Applications it is recommended not to user server side state as this limits the server that a specific user must connect to during their browsing session.  When adding servers to a cluster this session state coupling limits the load balancer from distributing requests across the farm as each user session must connect to the server they originally connected to.
    - One solution to scalability is to make the server side completely stateless and have the client pages pass state information between client and server on every page request.
    - Another solution to scalability is to store the session data in a shared database.
    - The downside to the classic web application architecture is the latency between page requests.  Each page is rendered and delivered as an independent page.  Any additional requests require a full round trip to the server to get a new page rendered, this causes the browser client to pause while waiting for new page content from the server as well as a flicker or flash when the new page is rendered.


- **Single Page Application**
    - A **Single Page Application**, often referred to as a **SPA** attempts to operate as a true client-server architecture where the browser client is an independent application that connects to one or more data services.
    - The 'single page' of the application is the index.html.  This file identifies the base structure and all necessary resources for the application (css, images, fonts, scripts) along with the external html wrapping (head/body):
    - The main downside of a SPA is the initial load time, generally because most SPA download the entire application structure on the first request for the page.  This is not a requirement, some sites have implemented "Lazy Loading" where a small subsection of the application is loaded and initialized.  This kernal can then interpret the requested route and request additional resources as needed.
    - The main benefit of the SPA architecture is the UX; For the user the experience is a much more streamlined flow where the visual layout remains fairly static like your desktop application window frame and menus.  What changes is the content within this frame.
    - examples of Single Page Applications:
        - gmail
