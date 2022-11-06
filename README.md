# Rimspace Plugin System

# Version 0.1.0.1

## *Our group chat: 424960844*
---

## Help document of Rimspace plug-in system

## Plug in Structure

> ### All plug-ins are located in the *Rimspace/plugins* directory
> 
> ### The *folder name* in this directory is the name of the *plug-in*
> 
> The plug-in directory should contain two files:
>
> 1. main.js **Master file**
> 2. manifest.json **Configuration file (optional)**


## Plugin Structure: Example

> Rimspace/ plugins/
>> MyPlugin/
>>
>>>
>>> main.js
>>>
>>> manifest.json
>>>
>> MyPlugin2/
>>
>>>
>>> main.js
>>>
>>> manifest.json
>>>
>> ...


# manifest.json
> ## Sample file contents
> &#160;{
> 
> &#160;&#160;&#160;&#160;"enable": false,  *//Whether to enable the plug-in*
> 
> &#160;&#160;&#160;&#160;"rsMinVersion": "0.1.0.0", *//Minimum supported version (no limit to null)*
> 
> &#160;&#160;&#160;&#160;"rsMaxVersion": "0.1.0.0", *//The maximum supported version (no limit to null)*
> 
> &#160;&#160;&#160;&#160;"langDependency": ["default"] *//Language File Dependencies*
> 
> &#160;}


# main.js

> ## Class Player
> 
> ## Object
> {
> 
> (property) cmdv: object;
> 
> (getter) ID: number;
> 
> (getter) IP: string;
>
> (getter) UUID: string;
>
> (getter) GameName: string;
>
> (getter) initTime: number;
>
> (getter) dps: number;
>
> (getter) cps: number;
> 
> (getter) latency: number;
>
> (getter) build: null | string;
> 
> (method) exec: (cmd: string, cb?: (json: object, obj?: object) => void, obj?: object ) => void;
> 
> (method) wexec: (cmdOrigin: string) => void;
> 
> (method) execute: (player: string, cmd: string, pos?: [number, number, number], cb?: (json: object) => void) => void;
> 
> (method) setblock: (x: number, y: number, z: number, block: string, data: number | string, cb?: (json: object) => void) => void;
> 
> (method) tellraw: (msg: string, target: string) => void;
> 
> (method) close(code?: number | undefined, data?: string | Buffer | undefined) => void;
> 
> }

> ## Class PCMD
> 
> ## Object
> {
> 
> (property) original: string;
> 
> (property) header: string[];
> 
> (property) body: string[];
> 
> (property) opt: object;
> 
> }

> ## This
> In the main file, 'this' is the plug-in object
>
> This object also contains the 'events' object
>
> ## Events
> - Quote: this.events
> 
> - Supported Events:
>> ## this.events
>> .onPlayerConnection: (player: Player) => void
>> 
>> .onClose: (exitCode: number) => void
>> 
>> .onPlayerDisconnection: (player: Player) => void
>> 
>> .onTickCircle: () => void
>> 
>> .onPlayerMessage: (player: Player, parsed: PCMD) => void
>>
>> .onConsoleInput: (parsed: PCMD) => void

- ## Rimspace Plugin Global Variable
> 
> ### api

```js
console.log(api);
```

> ### public
> 
> *Objects accessible to all plug-ins*

```js
console.log(public); // { }
```

> ### *RSON*

```js
RSON.parse(`[a] *: 1`); // { a: 1}
RSON.stringify({a: 1}); // [a] *: 1
```

> ### uuidv4
> 
> *Generate a uuid*

```js
logger.info(uuidv4());// 3ae71209-de06-4da7-a8ef-5c2fce7f11d5
```

> ### *random*

```js
logger.info(random(1, 3)); // 1, 2, 3
```

> ### parseCmdPos
> 
> ### *Separating coordinate data*

```js
console.log(parseCmdPos("setblock ~~~ air 0")); // {pos: [0, 0, 0], data: "air 0"}
console.log(parseCmdPos("setblock 100 255 100 dirt 1")); // {pos: ["100", "255", "100"], data: "dirt 1"}
console.log(parseCmdPos("setblock ~-0.1~ ~7 air 0")); // {pos: [-0.1, 0, 7], data: "air 0"}
console.log(parseCmdPos("fill 0 0 0 ~-1~-1~0.5air 0 destroy")); // {pos: ["0", "0", "0", -1, -1, -0.5], data: "air 0 destroy"}
```

> ### lang
> 
> *Call language file*

```js
logger.info(lang("lang header", "version"));// 0.1.0.1
```

> ### config
> 
> *Configuration Option (readonly)*

```js
console.log(config())
```

> ### exit
> 
> Exit rimspace

```js
exit(0);
```

> ### logger
> 
> Console logger
```js
logger.debug("debug");
logger.info("info");
logger.warn("warn");
logger.error("error");
logger.fatal("fatal");
```

> - ### Refer to *Variable Reference* for type and usage


---
# Start
- # Come and create your first plugin!
---

- # Here are some simple practical examples

# Hello, world!

- ## main.js

```js
logger.info("Hello, world!"); 
```

- ## manifest.json

```json
{
    "enable": true,
    "rsMinVersion": null,
    "rsMaxVersion": null,
    "langDependency": ["default"]
}
```

&#160;


# Hello, Steve!

- ## main.js

```js
this.events.onPlayerConnection = function(player){
    player.exec("say Hello, Steve!");
    player.tellraw("Herobrine: f**k u!");
}
```

- ## manifest.json

```json
{
    "enable": true,
    "rsMinVersion": null,
    "rsMaxVersion": null,
    "langDependency": ["default"]
}
```

&#160;


# Repeater

- ## main.js

```js
this.events.onPlayerMessage = (player, parsed) => {
    player.exec("say " + parsed.original);
}
```

- ## manifest.json

```json
{
    "enable": true,
    "rsMinVersion": null,
    "rsMaxVersion": null,
    "langDependency": ["default"]
}
```

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

&#160;

---
# *Variable Reference*
```js
//main.js
//supported global variable
console.log(api, exit, logger, lang, uuidv4, public, RSON);
```

```ts
//types...
const node: {
    pid: number;
    ppid: number;
    cwd: () => string;
    execPath: string;
    execArgv: string;
    env: NodeJS.ProcessEnv;
    features: {
        inspector: boolean;
        debug: boolean;
        uv: boolean;
        ipv6: boolean;
        tls_alpn: boolean;
        tls_sni: boolean;
        tls_ocsp: boolean;
        tls: boolean;
    };
    platform: NodeJS.Platform;
    arch: NodeJS.Architecture;
    release: NodeJS.ProcessRelease;
    version: string;
    versions: NodeJS.ProcessVersions;
    cpus(): os.CpuInfo[];
    freemem(): number;
    totalmem(): number;
    memoryUsage(): NodeJS.MemoryUsageFn;
    homedir(): string;
    userInfo(
        options: {
            encoding: "buffer";
        }
    ): os.UserInfo<Buffer>;
    uptime(): number;
    EOL: string;
    devNull: string;

} = api.node;
const packages: object = api.packages;
const uuid: (options?: crypto.RandomUUIDOptions | undefined) => string = packages.uuidv4;
const playerForeach: (cb: (player: Player) => void) => void = api.rimspace.playerForeach;
const rimspace: {
    stat: {
        startTime: number;
        cpuUsage: number;
        uptimeTick: number;
        errors: number;
        nodemem: number;
        cpu: {
            module: string;
            cores: number;
            speed: number;
        };
        platform: NodeJS.Platform;
        arch: string;
        userInfo: os.UserInfo<string>;
        sysVersion: string;
        net: NodeJS.Dict<os.NetworkInterfaceInfo[]>;
        env: NodeJS.ProcessEnv;
        connectionTimes: number;
    };
    config(): object;
    version: string;
    RSON: {
        parse: (text: string) => object;
        stringify: (obj: object) => string;
    };
    logger: {
        info(...args: any[]): void;
        cinfo(info: string, color: string): void;
        warn(...args: any[]): void;
        error(...args: any[]): void;
        fatal(...args: any[]): never;
        nolog(...args: any[]): void;
    };
    lang(...args: any[]): any;
    exit(code: any): void;
    getPlayerArray(): object[];
    playerForeach: (cb: (obj: object) => void) => void;
    playerQuantity: number;
    languageLoaded: string[];
} = api.rimspace;
const plugin: object = this;
const func: {
    drawProgress(len: number, percentage: number, color?: string): string;
    random(arg0: number, arg1: number): number;
    parseCmdPos(cmdOrigin: string): {
        pos: any[],
        data: string
    };
    readFileSync(path: string, encoding ?: string): Buffer;
} = api.func;
const fs: object = api.initFs();
```
