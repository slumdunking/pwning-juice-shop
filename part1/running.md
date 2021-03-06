# Running OWASP Juice Shop

## Run options

In the following sections you find step-by-step instructions to deploy a
running instance of OWASP Juice Shop for your personal hacking
endeavours. Only the most commonly used methods are described here. For a
full list of options - including Vagrant and Amazon EC2 deployment -
please refer to the corresponding
[_Setup_ section of the `README.md` on GitHub](https://github.com/bkimminich/juice-shop#setup).

### One-click cloud instance

!["Deploy to Heroku" button](img/deploy-to-heroku.svg)

The quickest way to get a running instance of Juice Shop is to click the
_Deploy to Heroku_ button in the
[_Setup_ section of the `README.md` on GitHub](https://github.com/bkimminich/juice-shop#deploy-on-heroku-free-0month-dyno).
You have to log in with your Heroku account and will then receive a
single instance (or _dyno_ in Heroku lingo) hosting the application. If
you have forked the Juice Shop repository on GitHub, the _Deploy to
Heroku_ button will deploy your forked version of the application. To
deploy the latest official version you must use the button of the
original repository at https://github.com/bkimminich/juice-shop.

As the Juice Shop is supposed to be hacked and attacked - maybe even
with aggressive brute-force scripts or automated scanner software - one
might think that Heroku would not allow such activities on their cloud
platform. Quite the opposite! When describing the intended use of Juice
Shop to the Heroku support team they answered with:

> That sounds like a great idea. So long as you aren't asking people to
> DDoS it that should be fine. People are certainly welcome to try their
> luck against the platform and your app so long as it's not DDoS.

### Local installation

To run the Juice Shop locally you need to have
[Node.js](http://nodejs.org/) installed on your computer. Please refer
to the
[Node.js version compatibility table on GitHub](https://github.com/bkimminich/juice-shop#nodejs-version-compatibility)
to find out what versions are currently supported. Juice Shop follows
the
[Node.js Long-term Support Release Schedule](https://github.com/nodejs/LTS)
for this purpose. During development and Continuous Integration (CI) the
application is most thoroughly tested with the current _Long-term
Support (LTS)_ version of Node.js. At the same time it tries to remain
compatible with at least one previous and the upcoming _Current_ version
of Node.js.

#### From sources

1. Install [Node.js](http://nodejs.org/) on your computer.
2. On the command line run `git clone
   https://github.com/bkimminich/juice-shop.git`.
3. Go into the cloned folder with `cd juice-shop`
4. Run `npm install`. This only has to be done before the first start or
   after you changed the source code.
5. Run `npm start` to launch the application.
6. Browse to <http://localhost:3000>

#### From pre-packaged distribution

1. Install a 64bit [Node.js](http://nodejs.org/) on your Windows or
   Linux machine.
2. Download `juice-shop-<version>_<node-version>_<os>_x64.zip` (or
   `.tgz`) attached to the
   [latest release on GitHub](https://github.com/bkimminich/juice-shop/releases/latest).
3. Unpack the archive and run `npm start` in unpacked folder to launch
   the application
4. Browse to <http://localhost:3000>

### Docker image

You need to have [Docker](https://www.docker.com/) installed to run
Juice Shop as a container inside it. Following the instructions below
will download the current stable version (built from `master` branch on
GitHub) which internally runs the application on the currently
recommended Node.js version. If you want to use a different Docker image
version, please look up the available tags in the
[Node.js version compatibility table](https://github.com/bkimminich/juice-shop#nodejs-version-compatibility)
in the project's README.md.

1. Install [Docker](https://www.docker.com/) on your computer.
2. On the command line run `docker pull bkimminich/juice-shop` to
   download the `latest` image as described above.
3. Run `docker run -d -p 3000:3000 bkimminich/juice-shop` to launch the
   container with that image.
4. Browse to <http://localhost:3000>.

If you are using Docker on Windows - inside a VirtualBox VM - make sure
that you also enable port forwarding from host `127.0.0.1:3000` to
`0.0.0.0:3000` for TCP.

## _Self-healing_-feature

OWASP Juice Shop was not exactly designed and built with a high
availability and reactive enterprise-scale architecture in mind. It runs
perfectly fine and fast when it is attacked via a browser by a human.
When under attack by an automated tool - especially aggressive brute
force scripts - the server might crash under the load. This could - in
theory - leave the database and file system in an unpredictable state
that prevents a restart of the application.

That is why - in practice - Juice Shop wipes the entire database and the
folder users might have modified during hacking. After performing this
_self-healing_ the application is supposed to be restartable, no matter
what kind of problem originally caused it to crash. For convenience the
_self-healing_ happens during the start-up (i.e. `npm start`) of the
server, so no extra command needs to be issued to trigger it.
