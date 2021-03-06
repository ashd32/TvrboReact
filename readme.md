# TvrboReact

## Features

TvrboReact is a clean, concise and powerful ReactJS starter project. It features the state of the art technologies from the React ecosystem, providing support for:

* **React**
* **Redux** + **Redux Thunk**
* **React Router 4** (with scroll history management)
* **Webpack 4**
* **Webpack Dev Server**
* **Web Sockets**
* **Server side rendering**
* **Streamed rendering**
* **React Hot Loader**
* **Mocha**
* **Session management** (Cookies + JSON Web Tokens)
* **Babel** (ES7, JSX, decorators, async/await)
* **ESLint**
* **Prettier**
* **ExpressJS**
* **Mongoose**
* **Nodemailer**
* **PM2**
* **makefile** (development and server tasks)

TvrboReact provides a rich foundation to build ReactJS applications featuring server-side rendering, session management, stream rendering, ODM, hot reloading, code linting, and more.

## Getting Started

### Requirements

Make sure you have NodeJS 8.11.1 or newer installed.

    brew install node

Make also sure that MongoDB is installed and running.

### Clone the Repository

Clone this repository into your computer.

You should now be able to run `make info` and see the full list of commands available to you.

### Install the dependences

    make install

This will also perform an initial build. To import initial data into the database, run:

    make populate

### Live development

    make dev

Then, go to `http://localhost:8080` in your browser and start developing with live reload/react hot loading!

### Production build

    make build

Will package all the assets into the `public` folder.

    make run

Will start the app and serve whatever is in the `public` folder. Stop it by hitting `Ctrl+C`. This is a good way to check the real performance of your app in production conditions.

### App management (in a server)

You may want to use a process management tool like **[PM2](http://pm2.keymetrics.io/)**.

Edit `process.json5` to set your project name, execution mode, etc. Four utility tasks are defined:

    make start
    make reload
    make restart
    make stop

## Coding

### Decorators

To access content in your Redux stores, connect your React components

    @connect({ entries, coins } => { entries, coins })
    class MyView extends Component {
    	...
    }

If a component uses `Route`, `Switch` or any route-aware component from **React Router**:

    @withRouter
    class MyView extends Component {
    	...
    }

If you were to use both, leave `@withRouter` as the first decorator.

### Configuration

Even if Webpack performs Tree Shaking on ES6 modules, it may not be a good idea to use a single config file for the server and the client. That's why both are split into separate files and must be included accordingly.

**Server only code** may import `app/config/server.js` and `app/config/client.js` and use any of its values.

However, **files bundled by Webpack** should only import values from `app/config/client`. Otherwise, execution will throw an error to prevent that you bundle and leak any API keys or other secret data.

Usage:

    import config from 'app/config/server';
    // ...
    console.log(config.HTTP_PORT);


    import appConfig from 'app/config/client';
    // ...
    console.log(appConfig.APP_NAME);

#### Defaults

* In development, Webpack Dev Server exposes the port `8080`, acting as a proxy to the NodeJS server (port `3000`)
* Otherwise, NodeJS listens on port `8080` when you start it with `make run`

### Testing

Run `make test` and let the magic happen. This will provide you detailed information in case of failure. To get a cleaner summary, you may run `make check` instead.

### Utilities

* You can use `test/populate.js` to add your code for sample data creation in your database. Run it with `make populate`.
* You can use `test/wipe.js` to clean any data in your local database and repopulate it again. Use `make wipe` for this.

<!--
### Localization
#### Template extraction

	make po:extract

Will extract the strings contained within `t("Translatable text inside t(...)")` and will generate/update the necessary template files for translation.

For every supported language defined in `app/client.config.js`, a folder will be created on `app/locales/` with the templates inside `translation.json` and `translation.po` files.

**NOTE**: Only `app/locales/../translation.json` will be used by the server. The `.po` files are intended for non technical translators, and they need to be **compiled** back to the corresponding `json` file.

Running this command will not wipe existing strings. Contents that are no longer used will be moved to the `translation_old.json` file.

#### Compiling from a .po file

	make po:compile

Reads all the `.po` files inside `app/locales/<lang>/` and compiles their content into the corresponding `translate.json` file.-->

### Project structure

    app
    	api               >  Implement here the API handlers to interact with the database
    	config            >  Client/server settings
    	lib
    		actions.js      >  Action creators
    		api.js          >  Client side api wrappers
    		session.js      >  Manage user sessions (check login, decode payload, etc)
    		intervals.js    >  Recurring tasks can be initialized here
    		...             (your own utilities)

    	mail              >  Mailing utilities with built-in image attachments
    	media             >  Media files that will be copied to 'public/media' on run
    	models            >  Your Mongoose data models
    	reducers          >  Implement the logic to create new states upon actions
    	store             >  Redux store creation and composition
    	styles            >  Provide styling for your components
    	views             >  JSX components intended to be used as pages
    	widgets           >  Smaller JSX components intended for encapsulation and reuse

    	app-template.js   >  Generates the HTML template (used when server rendering)
    	app.jsx           >  The root component (define your main routes here)
    	client.jsx        >  The entry point of the client render
    	server.jsx        >  The entry point of the server render
    	socket.js         >  Attaches the WebSocket server to the app handler

    test
    	index.js           >  The test runner
    	tests              >  Write your own tests here
    	populate.js        >  (Utility) Populate sample content (DB)
    	wipe.js            >  (Utility) Remove DB contents

    index.js             >  The start script for the server
    makefile             >  Tasks definition
    process.json5        >  PM2 config file
    webpack.*.config.js  >  Webpack development and production settings

    public               >  Folder where everything is packaged and served from

### Deploy to Heroku

To deploy the app to Heroku, follow these steps:

* [Download the Heroku toolbelt](https://toolbelt.heroku.com) and create a [Heroku account](https://www.heroku.com)
* Log in with `heroku login`
* Run `git init`
* Run `heroku create <APP_NAME>`
* Run `git push heroku master`
* Open `https://APP_NAME.herokuapp.com` in your browser
