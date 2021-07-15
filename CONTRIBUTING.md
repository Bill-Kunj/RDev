# Development and Design notes for Contributors

Thanks for your interest in contributing!

## Quality Code Style

All contributions should pass:

 1. `npm run lint-check`
 2. `npm run lint:types`

## Object capability (ocap) discipline

In order to support robust composition and cooperation without
vulnerability, code in this project should adhere to [object
capability discipline][ocap].

  - **Memory safety and encapsulation**
    - There is no way to get a reference to an object except by
      creating one or being given one at creation or via a message; no
      casting integers to pointers, for example. _JavaScript is safe
      in this way._

      From outside an object, there is no way to access the internal
      state of the object without the object's consent (where consent
      is expressed by responding to messages). _We use `Object.freeze`
      and closures rather than properties on `this` to achieve this._

  - **Primitive effects only via references**
    - The only way an object can affect the world outside itself is
      via references to other objects. All primitives for interacting
      with the external world are embodied by primitive objects and
      **anything globally accessible is immutable data**. There must be
      no `open(filename)` function in the global namespace, nor may
      such a function be imported. _We use a convention
      of only accessing ambient authority inside WARNING sections._

  - **PyRGOV Testing framework**
    - PyRGOV is in its infancy. It currently does not adhere to ocap
      discipline. Enhancements to the testing framework will be expected
      to remediate this oversight.

[ocap]: http://erights.org/elib/capability/ode/ode-capabilities.html


## To run RNode stand alone on localhost:
for bootstrapping, snapshots, and updating a rgov localhost instance for linux and Window10 WSL2

Watch video of how to run an rnode localhost and add rgov actions here https://youtu.be/9TIPXXSXwnE bootstraping updates https://youtu.be/fuXFDRXJsVM

## localhost deployment and development
To create an rchain node locally, deploy rchain dependencies, and deploy rdev use the following commands. These commands will:
  1) Create several log files in `bootstrap/log`, which can be largely ignored.
  2) clone the [rchain](https://github.com/rchain/rchain) repository
  3) deploy rholang files from RChain and RDev
  4) Create a master dictionary
  5) Place references to RDev rholang functionality into the master dictionary under a key using the rholang filename
  6) Generate a JSON file containing the URI of the master dictionary: `src/MasterURI.localhost.json`
  7) Create a [snapshot](snapshots) containing the resulting rnode database that can be restored with restore-snapshot
  6) Place the running rnode log file in `bootstrap/log/run-rnode.log`

```bash
cd bootstrap
./bootstrap
./deploy-all
./run-rnode
cd ..
```

## dev interface installation
```bash
npm install
```

## running dev interface
This command will bundle the RDev JavaScript, build the RDev web page, and open your browser to RDev.
```bash
npm start
```

## snapshots
Restore a snapshot previously created with `bootstrap/create-snapshot`
```bash
cd bootstrap && restore-snapshot
```

After initial bootstrap and deploy-all, there will be two snapshots available: `bootstrap` and `rgov`

List snapshots available for restore
```bash
cd bootstrap
list-snapshot
```

Save a copy of the localhost rnode that can be restored at a later date
```bash
cd bootstrap  && create-snapshot
```

## Command line deployment of rholang
Deploy the rholang file "test.rho"
```bash
cd bootstrap && ./deploy ../test.rho
```
Propose the previously deployed rholang file "test.rho"
```bash
bootstrap/propose
```

## Misc: dependencies
We have a dependency on rchain-toolkit. For utils. This can go away soon, right?
