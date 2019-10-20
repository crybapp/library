# macOS Setup
Firstly, you'll need to install some developer tools to get Cryb setup on macOS.

## Docs
* [Using Terminal](#using-terminal)
* [Tools Needed](#tools-needed)
* [Next Step](#next-step)

## Using Terminal
You'll want to use `Terminal.app`, which is pre-installed on all macOS installations by default. You can find Terminal under `Appications/Utilities/Terminal.app`.

Here you'll run all the commands needed to setup Cryb.

**Footnotes**
* *If you want to use a terminal emulator, such as iTerm or Hyper, then go ahead!*

## Tools Needed
* Homebrew
    * **Recommended**
    * Used for installing required packages
    * [Install](https://brew.sh)
* Node.js Runtime
    * **Required**
    * Used as the runtime for all major Cryb deployments.
    * [Install](https://nodejs.org/en/)
* Yarn
    * **Recommended**
    * Used instead of `npm` for running deployments and installing packages.
    * [Install](https://yarnpkg.com/en/docs/install#mac-stable)
    * If you installed Homebrew, you can run `brew install yarn` to install Yarn
* MongoDB
    * **Required**
    * Used as the primary datastore for `@cryb/api`, `@cryb/portals` and `@cryb/aperture`
    * [Install](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)
    * If you installed Homebrew, you can simply run the following commands to install MongoDB:
        * `brew tap mongodb/brew`
        * `brew install mongodb-community@4.2`
    * You may also want to install a MongoDB GUI, such as [Robo 3T](https://robomongo.org/download) or [MongoDB Compass](https://www.mongodb.com/products/compass) to make it easier to view and edit data inside of your MongoDB server
* Redis
    * **Required**
    * Used as the primary cache and pub/sub for `@cryb/api` and `@cryb/portals`
    * [Install](https://redis.io/topics/quickstart)
    * If you installed Homebrew, you can run `brew install redis` to install Redis
    * You may also want to install a Redis GUi such as [Medis](https://github.com/luin/medis) to view or edit data and run commands on your Redis server

## Next Step
Next we'll setup `@cryb/api`: [Setting up API](./api.md)
