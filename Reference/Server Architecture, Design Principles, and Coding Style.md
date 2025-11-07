# 

[Architecture and Design Principles](#architecture-and-design-principles)  
[Scalability and Availability](#scalability-and-availability)  
[Data Storage](#data-storage)  
[Service Oriented Architecture](#service-oriented-architecture)  
[Backpressure](#backpressure)  
[Testing](#testing)  
[Third Party Modules](#third-party-modules)  
[Code Organization](#code-organization)  
[Files and Directories](#files-and-directories)  
[Code for Debugging](#code-for-debugging)  
[Named Constructors](#named-constructors)  
[ScatterGather](#scattergather)  
[Coding Style](#coding-style)  
[JSHint](#jshint)  
[Text Editors](#text-editors)  
[Indentation](#indentation)  
[Line Width](#line-width)  
[Identifiers](#identifiers)  
[Quoting](#quoting)  
[Semicolons](#semicolons)  
[Comments](#comments)  
[var Statements](#var-statements)  
[Curly Braces](#curly-braces)  
[When to Indent](#when-to-indent)  
[Optimize For Reading Later](#optimize-for-reading-later)

# 

# Architecture and Design Principles {#architecture-and-design-principles}

This document describes the production operation for the Voxer service. This system is written in node.js and runs on SmartOS. There is also an analytics system that’s written with Java for the Hadoop ecosystem. Analytics is documented elsewhere.

## **Scalability and Availability** {#scalability-and-availability}

Voxer serves a diverse, global user base that depends on the service 24 hours a day. We intend the service to always be up, even during upgrades, machines failures, or maintenance. We are building a new class of telecommunications infrastructure, so it should always be up.

To deliver this kind of service, we must tolerate the loss of any single machine or process. It’s possible and likely that certain combinations of failures will degrade service, but the loss of any single machine or process should not be any more serious than any other machine or process.  If any machine or process fails, we should try to bring it back as soon as is practical, but the Voxer service must continue to operate fully until the failed component is restored.

To add capacity to the system, we add more hardware. All processes should be able to distribute their work across an increasing pool of machines. This is sometimes referred to as “scaling out, not up”.

## **Data Storage** {#data-storage}

We use three different data storage strategies with different tradeoffs for different types of data. All three data storage systems meet the Scalability and Availability requirements listed above.

**Riak** is for durable storage. Data is read and written to disk drives, and this data is saved indefinitely. Riak is relatively slow and expensive, so we try to use Riak mostly for writes and serve reads as much from cache as possible. Riak maintains the authoritative copy of our data.

**Data Store** (DS) is both a cache in front of Riak and an ephemeral storage system. DS presents a Riak-like API to the application, and stores each object in a file in the filesystem. We rely on the OS disk cache to keep the relevant blocks in memory. DS does not do replication, but it also does not serve the authoritative copy of data, so loss of a DS process or disk can be handled by Riak.

**In Process \-** Some purely in-memory state is maintained inside our node.js processes themselves. This includes data like NR sessions and updates channel data but also network connections held open to facilitate rendezvous in our protocol.  Ideally, this should be avoided unless it is ok for this state to be lost.

At the time of this writing, Redis is used for various things. Redis is a very fast in-memory database, but it does not adhere to our Scalability and Availability requirements, so we are phasing it out.

## **Service Oriented Architecture** {#service-oriented-architecture}

All major functionality should be deployed as a service. Services for the production operation should be written in node.js, although analytics services are typically written in Java. We use the http\_server module for the server component and poolee for the client side.

If a request fails, it is preferable to use poolee’s retry mechanism to try the request again on a different node. If requests either cannot be retried or have exceed a reasonable number of retries, then we fail the request back to the mobile client. Because the mobile clients can work offline and must deal with unreliable network conditions, they all have retry mechanisms.

The “live” experience is more important than delivering everything in order. There are no traditional message queues in the system. Requests are routed live, or else they are retried later.

Services talk JSON over HTTP. Each JSON string is terminated by a CRLF. Multiple JSON strings may be returned on some responses. Raw media bytes are also transported over HTTP.

A service is a pool of processes that provide equivalent functionality to the rest of the system. Most services maintain a consistent hash ring or “ring” for short. Rings are used to distribute work across the pool with minimal coordination on each request. Once the ring members agree on the list of active members, any member can do a lookup of some sort of key and find the current “home” for that key. We use rings to schedule connection rendezvous points, order operations into Riak for the same key, manage timers, and keep session state.

## **Backpressure** {#backpressure}

Note that “backpressure” is not officially a word, but it seems like it should be. Sometimes things slow down, and something has to fail. If we detect this condition before it happens, we’ll get to pick what fails. If we don’t, everything fails.

The primary backpressure mechanism in our system is poolee.  If too many requests are in progress at a time, new requests are rejected with HTTP 503\.  All server code should consider backpressure situations (like checking IO fail conditions, queue buildup, etc) and notify upstream accordingly.

Riak in particular has problems sometimes properly detecting and adapting to backpressure from slow nodes. This is typically what   
causes a Riak cascading failure.

## **Testing** {#testing}

All code should be testable. At the time of this writing, a reasonable set of functional tests exist, and very few unit tests exists. Some code remains difficult to test. All new code should be testable, and ideally have tests written for it.  Good testing practices will be discussed elsewhere.

## **Third Party Modules** {#third-party-modules}

The use of third party modules is a tradeoff of initial implementation time for overall operational cost. It is clearly faster to get started in many cases by using a third party module. Voxer has more traffic going through node.js than just about anybody else. We regularly find bugs in node itself. Most modules from the node.js community are not subjected to this kind of production rigor, and as a result tend to have tricky bugs. Third party modules are typically trying to solve different problems than we have, and have thus made different tradeoffs.

In addition to the operational costs which are easier to measure, third party modules also expose us to potential legal issues from licenses and patents. The potential for damage here is pretty large, but it’s also difficult to quantify the likelihood. If we do write most of our own software, then we’ll be less exposed to this sort of thing.

When considering a third party module, see if it’s possible to write the necessary functionality internally. If it isn’t, then propose this module for inclusion to the server team, and we’ll discuss the various tradeoffs.

We already do use some third party modules, and we will continue to do so when the tradeoffs make sense. We use node\_redis because Matt is the primary author, although we are phasing this out once we phase out Redis. We also use bcrypt because it involves specialized cryptographic knowledge that we do not have in-house.

# Code Organization {#code-organization}

## **Files and Directories** {#files-and-directories}

Each service should have its own directory with a descriptive name. The starting point file for the service should also be descriptive of the service such as “node\_router.js” or “header\_store.js”. Avoid generic file names like “server.js” or “index.js” because they are likely to conflict with another service.

Each service should have tests in a “test” directory. The .js files that make up the service should all be in the top level directory for that service.

## **Code for Debugging** {#code-for-debugging}

For debugging reasons, all modules and interesting state should be somehow exposed to the REPL. Named constructors and prototypes should be used for most logic, and the names of these constructors should be unique across all of our code. This makes debugging core dumps more useful.

To facilitate REPL debugging, testing, and assembling multiple modules into a single process, we construct and pass around an explicit “global object” called “RV” for nostalgic reasons. This RV object is what is exposed to the REPL.

Here is an example module that itself pulls in several other modules.

var RV;

module.exports \= function init(new\_RV) {  
    RV \= new\_RV || {};

    RV.KeepAliveAgent \= require("./keep\_alive\_agent")(RV);  
    RV.PoolPinger \= require("./pool\_pinger")(RV);  
    RV.PoolEndpoint \= require("./pool\_endpoint")(RV);  
    RV.PoolEndpointRequest \= require("./pool\_endpoint\_request")(RV);  
    RV.PoolRequestSet \= require("./pool\_request\_set")(RV);

    return require("./pool")(RV);  
};

Users of this module would do something like:

var RV \= {};  
var Pool \= require(“./poolee”)(RV):  
var example\_pool \= new Pool(\[“1.2.3.4:123”\]);

## **Named Constructors** {#named-constructors}

Here is an example of using named constructors to describe logic. Instead of using nested anonymous functions as callbacks like this:

function member\_info(thread\_id, callback) {  
    database.get("tread info", thread\_id, function (err, res, obj) {  
        if (obj.group) {  
            database.get("group list", obj.group\_id, function (err, res, obj) {  
                callback("group: " \+ obj);  
            });  
        } else {  
            callback("not group: " \+ obj);  
        }  
    });  
}

This same logic can be structured around callbacks on an object’s prototype like this:

function MemberInfo(thread\_id, callback) {  
    this.thread\_id \= thread\_id;  
    this.callback \= callback;  
    database.get("thread info", thread\_id, this.on\_thread\_info.bind(this));  
}

MemberInfo.prototype.on\_thread\_info \= function (err, res, obj) {  
    if (obj.group) {  
        database.get("group list", obj.group\_id, this.on\_group\_list.bind(this));  
    } else {  
        this.callback(“not group: ” \+ obj);  
    }  
};

MemberInfo.prototype.on\_group\_list \= function (err, res, obj) {  
    this.callback(“group: “ \+ obj);  
};

Of course in real code we have lots of error checking and other interesting logic here. This is just a contrived example.

## **ScatterGather** {#scattergather}

To start multiple operations and wait them all to complete, use lib/scatter\_gather.js.

# Coding Style {#coding-style}

## **JSHint** {#jshint}

We use JSHint to validate our code. All code and tests should pass jshint with our configuration.

## **Text Editors** {#text-editors}

Use whatever text editor makes you happy and productive. It is very nice if your editor runs jshint on every save.

## **Indentation** {#indentation}

We use 4 spaces for indentation, always spaces and never TABs.

## **Line Width** {#line-width}

Try to keep lines less than 160 characters wide. Most lines will obviously be a lot narrower than this.

## **Identifiers** {#identifiers}

We use underscores for all identifiers except the names of constructor functions. Constructor functions  
are capitalized and then camel-cased. Methods on the prototype are snake case with underscores.

Example:

    function FancyObj() {  
        this.count \= 0;  
    }

    FancyObj.prototype.go\_big \= function () {  
        this.count \= 1000;  
    }  
      
    var some\_obj \= new FancyObj();

## 

## **Quoting** {#quoting}

We use double quotes for strings.  
    

## **Semicolons** {#semicolons}

Semicolons go at the end of each statement.

## **Comments** {#comments}

The point of comments should be to explain things that aren't obvious by simply glancing at the code.

Use the double-slash space "// " per-line comment style and not the /\* \*/ block comment style.

The notion of what is "obvious" is subjective, but here are a couple examples:

Example of a non-obvious section with a useful comment. These are the most useful kind of comments:

    while (in\_chunk.bytes\_remaining \> 0 && in\_chunk.bytes\_remaining \>= cur\_chunk.bytes\_remaining) {  
        **// if in\_chunk has more data than will fit cur\_chunk, fill it, save it, then get the next chunk**  
        **// This may fill and save several chunks if lots of incoming data arrives at once**

        copied \= in\_chunk.copy(cur\_chunk.buf, cur\_chunk.write\_pos, in\_chunk.read\_pos, in\_chunk.read\_pos \+ cur\_chunk.bytes\_remaining);

Example of an obvious comment line for something that's just a copy of a well-chosen identifier. Don't do this:

    **// get from ring**  
    GetBody.prototype.get\_from\_ring \= function (home\_bs) {

The name of the function is "get\_from\_ring", so the comment is just a distraction.

If you’re not sure if something is obvious or not, err in the side of caution and add a comment.

Example of explaining why an identifier was chosen:

    // "destroy" is part of the node stream API  
    CacheStream.prototype.destroy \= function () {

## **var Statements** {#var-statements}

Be aware of functional scope. While JavaScript may appear to have lexical scope, it does not. This has implications that jshint can often find, but sometimes cannot. All variables declared in a function are visible to the entire function and all nested functions, no matter where you declare them.

## **Curly Braces** {#curly-braces}

if / else:

    if (a && b) {  
        do\_something();  
    } else if (c) {  
        do\_another\_thing();  
    } else {  
        log\_error();  
    }

Empty object literal:

    obj \= {};

Simple object literal:

    obj \= { foo: "bar" };

Returning an object literal:

    return {  
        count: 1,  
        label: "example"  
    };

## **When to Indent** {#when-to-indent}

Simple object literals can be on one line:

    var list \= \[1, 2, 3, 4\];

More complex object literals should use multiple lines:

    obj \= {  
        key: "some key",  
        val: "this adds a lot of value",  
        count: 1,  
        deleted: false  
    }

Simple and compact one line conditionals are OK if they are non-clever, but use an if statement instead of depending on side-effects of &&:

    if (flag) { simple\_thing(); }

It's better to use curly braces and a new line for the conditional body if the logic is anything other than completely obvious:

    if (res && (res.statusCode \=== 200 || res.statusCode \=== 204)) {  
        success();  
    }

If statements should try to be on the same line unless it gets too wide:

    if (this.disk\_write\_stream\_opened && this.disk\_write\_stream.writable && pos \< max\_bytes) {

If it gets too wide, use as few lines as necessary, splitting the condition where it makes the most sense:

    if (this.disk\_write\_stream\_opened && this.disk\_write\_stream.writable && pos \< max\_bytes &&  
        (major \=== RV.min\_version\[system\_name\].major && minor \=== RV.min\_version\[system\_name\].minor && patch \>= RV.min\_version\[system\_name\].patch)) {  
        do\_something();  
    }

String construction should use as few lines as possible, splitting whenever makes sense:

    log("get from ring", self.message\_id \+ " \- finished " \+ fmt.http\_code(code) \+ " recv from/sent to " \+ bytes\_in \+ "/" \+ bytes\_out);

For loops should all start with one line:

    for (i \= list.start\_pos; i \< list.length; i++) {

Anonymous functions always start a new line for their body:

    \[1,2,3\].forEach(function (val) {  
        console.log(val);  
    });

## **Optimize For Reading Later** {#optimize-for-reading-later}

It can be tempting to write terse and clever lines to a problem in a way that is somewhat satisfying to the author but perplexing to any other readers. It’s better to use an extra few lines of code here and there and be obvious and explicit about what the code does.

*Add examples*