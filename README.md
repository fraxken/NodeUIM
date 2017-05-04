# NodeUIM
CA UIM NodeJS interface to work with pu.exe in a full async way.

Find API Documentation [Here](https://github.com/fraxken/NodeUIM/wiki)

## Requirement 

- NodeJS 7.x.x or higher

> Install on the system **or take the binary version to execute without installing NodeJS on the system**. 

## Examples 

> Async is an external package, if npm is install just run : npm install async

```js
const async     = require('async');

const Nimsoft   = require('./nimsoft.js');
const Logger    = Nimsoft.Logger;

const config    = require('./configuration.json');

const logger = new Logger({
    file: 'checkconfig.log'
});

process.on('exit', () => {
    logger.nohead(console.timeEnd('time'));
    logger.close();
});

logger.nohead(console.time('time'));
logger.info('Get hubs!');
logger.nohead('---------------------------------');

const getHubs = SDK.Request(config);
getHubs({ callback: 'gethubs' }).then( PDS => {
    async.each(PDS.get('hublist'),(hub, next) => {
        logger.info(hub.name);
        next();
    }, err => {
        logger.info('All hubs done!');
        process.exit(0);
    });
})
.catch( error => {
    logger.error(error);
    process.exit(1);
});
```

Interface for request method : 

```js
interface IRequest {
    path: string,
    callback: string,
    timeout: number,
    debug: boolean,
    maxBuffer: number,
    encoding: string
}
```

# Roadmap 

- npm package
- implement nimAlarm
