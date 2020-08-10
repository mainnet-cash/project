This document is **totally** an invitation for discussion and it's not set in stone. It's just some basic ideas, which seem
to make sense now. If we (participants of the project) feel that something here stinks, we should just discuss it and
change it.

If you want to discuss this document - please open a GitHub issue in https://github.com/mainnet-cash/project or let's talk in Slack (see below)

Project
-------

Build a library as described on:
- https://web.archive.org/web/20200810182937/https://mainnet.cash/
- https://web.archive.org/web/20200810183217/https://mainnet.cash/2020-07-31

Errors:

`await wallet.send([[seller.depositAddress(), 0.01, 'USD']]);` 

should probably be 

`await wallet.send([seller.depositAddress(), [0.01, 'USD']]);`

Funding
-----

| Balance | Wallet | |
|---|---|---|
| 750.28325111 BCH | [bitcoincash:qqg....qfx6](https://explorer.bitcoin.com/bch/address/bitcoincash:qqgae2xl2rx7zwg4nuwv645u9pfkydrp45cy0wqfx6) | collected via https://flipstarter.mainnet.cash/ |

The spending will be public, but I'm yet to figure out where to publish it.

Where are we?
-------------

GitHub organization:

https://github.com/mainnet-cash/

Slack (for discussions):

https://join.slack.com/t/mainnetjs/shared_invite/zt-gjkf1zw4-S4oxNQqA~YeDxTAEdKWdrA
(link expires around Sep 8th, email hello@read.cash for a new one)

Governance
----------

The first version of the plan calls for open bounties so that there is a list of things to do and as soon as something is
detailed enough, a bounty is assigned. 

A preliminary high-level list of things to do contains ~100 items, but 1) some of them are pretty big (like "Video Tutorials"), 
and 2) I assume that for one task that
I see there's at least 2-3 that I fail to see now, so I'll start with an conservative assumption that we have ~400 items in our to-do list.

Another thing is that there are things that are not in the list, but are still necessary, such as to pay for servers, video editing,
video tutorials, donations to infrastructure projects or projects where you take a lot of code from, security audits, etc...
So let's start with the assumption that for an average task we might give $300. 

Remember though, that you're
getting Bitcoin Cash, which might rise in price 10 times or more, so $300 you get today,
might be $2,000 in a year. 

Also, remember that $225,000 that we have collected is not enough to pay the market
rates per hour. We assume that your motivation to work is to create something useful for people and a small amount being
paid is just a nice bonus.

Let's just not assume we can get rich from this project.

I intend to spend all of the collected amount (750 BCH) without taking any money myself (it's going to be unfair
to tell everyone that we don't have enough and yet pay myself well). The spending will be public (I'll log transactions
and what they were done for). You are free to shuffle/fuse the coins after. What you need to remember is that transaction
to you is public.

I don't intend to take money also because I can't dedicate too much time to actual development, so I will be mostly coordinating development with a bit of an architectural oversight. 
Ideally, as the project progresses if I can find a candidate who is strict enough to do a good job - I'll
be happy to assign a lead maintainer role to him and give him some funds for his job.

If that plan doesn't work, i.e. work is not being done well and on-time, then the other plan would be to hire
outside developers and ask them to complete whatever we can do at market rates (outside developers won't care about
BCH potentially growing in price, so we'll have to give them the market rate), but I'm hoping that
we can manage it ourselves.

The project will be considered done when all funding is spent and all I have left in the wallet is one year's
worth of server costs. At that point, we might consider starting another Flipstarter if we collectively feel there's
more to be done or we'll leave it to open-source contributors to help.

Another possible outcome could be that we build something like Patreon and we'll have dedicated developers
(from project participants) at market rate, if funding allows.

We could (and will) try to monetize some parts of the project, namely for example WebHooks on our remote server. We
should allow some quota per domain free (maybe 1000 webhooks), but as users can easily generate millions of addresses,
to keep this service up, it would be necessary to take a small payment for bigger numbers (like a million webhooks).
Users will be free to install their copy, though, and use it without any limits for free. The idea is not to make
money here, the idea is to prevent abuse.

Goals and Project scope
-----------------------

We intend to create a high-level interface to the Bitcoin Cash network. By "high-level" I mean that it closely resembles
the language of a person who has a good understanding of how money works but has little understanding about how
Bitcoin Cash or cryptocurrencies work inside. So things like "UTXO", "Private Key", "opcodes", "OP_RETURN" should be
considered low-level and we should have them in our interface ONLY if unavoidable. We should strive to have
an interface that has things like "wallet", "name", "send", "wait for deposit" - things a manager would say,
not things that a developer would say.

The project includes:
- TypeScript library, compiled to JavaScript for browsers, (TypeScript for type-safety)
- REST API server, based on TypeScript code we wrote for the library
- Other language bindings, which at first would be thin wrappers to this REST API, but later (as finances allow)
  will implement crucial functionality (like signing a transaction). Signing a transaction can be safe if a user
  installs their copy of REST server (via Docker or something like that), that way the private key, despite
  being sent to the server never leaves user-controlled hardware, which makes the scheme safe. For development and
  testing purposes we can safely allow people to use our (public) instance of this REST server and, with a warning,
  they might even use it for production (if they are just testing an idea and aren't ready to commit to their own
  installation)
- Public infrastructure (Deployed REST API + whatever else is needed)
- Documentation website + translations of it
- Video tutorials
- Invitation of outside developers to try to use this
- PR campaign (additional funds)

We should always keep the REST API in mind with a user in other programming languages. We might get ourselves in
a situation where TypeScript API will be incompatible with REST calls.

See below for a detailed To-Do list.

Priorities
----------
- (1) Developer (as in "a user of our library") happiness, convenience, using "high-level" concepts
- (2) Security
- (2) Backwards-compatibility (as soon as we hit 1.0)
- (2) Serializability
- (2) Stability
- (3) Open Source, free to copy

Yes, I have four 2's. In my opinion, they are equally important.

"Developer happiness" means that if we can get a happy developer even though he's currently sending his
private keys to a remote server - we should do it, but seriously warn him about what he's doing. The goal of this
project is to attract developers. There should be a way to do things securely, but we must not prevent users
from doing not-very-secure things, but we should warn and nudge him to do it right.

The "serializability" principle applies only to stuff that we give to user and we expect it later back in subsequent
calls.

By "serializability" I mean that we should strive to keep things simple so that a user can simply get a string and
save it to a database for future use. An example might be UTXO - we could give it to the developer as an object such as
{"txid": "1234abcdef", "vout": 1} (which is an object and it's much harder to store for future use), or we could
give him a simple string like "1234abcdef:1", which is much easier to store.
While this is a simple example, a more complex example would be an escrow contract or a token. If a user requests
an escrow contract - a suitable representation for it might be a JSON-encoded string that includes a fully-signed
"refund" and fully-signed "proceed" transaction, so that the user can send us this as a string back with a call
to "refund" and we'll immediately have all the necessary data to do that. A general contract would probably need
to be serialized to a JSON string that has a private key, contract source (for ex. if it is in CashScript/Spedn),
and UTXO to spend, so that a user can send back this string along with a call that he needs to make and be done,
instead of storing the private key and the contract source and UTXO and whatever else is necessary separately.
The same is for tokens. It's not enough to serialize it to TXID, we need private key there too, so that again -
a user can send the REST server this string that we gave him and a call what needs to be done (mint, send, etc...)
and we can do it.

The "serializability" principle is mostly a consequence of "Developer happiness." We must try to make it super-easy
for users of our library.

"Stability" means that our library and infrastructure must work. This also means that tests are required.

The last point means that all our code will be available under the MIT license.

Stability
---------

We will start with a mark that this is a **"prototype"**, meaning that we are free to change any interfaces at any time
if we see that we did something stupid. As soon as we declare it to be 1.0, we must stop changing interfaces and
keep backward-compatibility in the highest regard (i.e. new releases of 1.x.x must never break previous code).

What next?
----------

If you want to apply as a TypeScript developer: If I know your name as a Bitcoin Cash developer who can deliver stuff
for sure, you are free to join immediately and take any available tasks, if I don't know you - I have a simple test
in JavaScript for you, which you should be able to do in 10-15 minutes. I just don't want to spend time if somebody
who can't code will take a task and will never finish it. Contact me (ReadCash on Slack, see above).

First of all, we need to create some scaffolding for this (i.e. testing framework, setup CircleCI to run these tests),
then we'll try to create RegTestWallets first so that the library will download BCHD and run it in regtest mode
(a simulated Bitcoin Cash network on your computer with free money whenever needed). Why BCHD? Mostly because it's
single-binary, cross-platform, it supports web calls (via grpc-web) out of the box, it has indexing, subscriptions -
mostly everything we need for this library, and SLP tokens support is coming soon (even double-spend proofs are coming,
which will be amazing for zero-conf).

Another candidate could be Electrum servers (these power Electron Cash), but unfortunaltely they require at least 2
components - electrum server and a node (like BCHD), which every developer would have to install to use it,
so I'm leaning more towards using BCHD, since it's already sufficient (web+node) and self-contained single binary.

By RegTestWallet I mean a non-HD wallet. Simple private key or seed phrase + derivation path wallet.
The opposite would be RegTestHdWallet.

But it won't be too hard to write adapters for other nodes if the need arises. I mean, it's hard to imagine now why
would people need to have support for other nodes, but such an abstraction should be not too hard to implement if needed.

Then we should make sure that something as simple as .send([1.1, 'BCH']) works, we can move on to TestNetWallet and then
to Wallet.

NB about precision. I know that it's not a great idea to use 1.1 as an amount due to precision problems, but remember
priority number 1 - developer happiness. If a user needs exact precision he would use [1100000000, 'sat'], which is
more precise but gives you much less happiness. We could think about using "1.1" as string _optionally_
(i.e. number|string union type), but this might get us into trouble with statically typed languages when using REST API.
Most of the statically-typed languages won't accept "float|int|string" as something valid, so it's an open question,
in the absence of a good solution, let's just keep it float to keep the developer happy.
Why I don't want to use string by default - it's hard, you need to convert it to number in many languages and
it's a mess. As for using it in return values in REST - I think we could return it as a simple object
{bch: 1.1, sats: 1100000000}, so depending on whether you need precision or convenience - you could take a single value.
(Open for discussion)

If we manage to perfect the TestNetWallet, we'll move to Wallet (which is a MainNet wallet). And HD versions of these
wallets. We'll need to decide what to use as storage since HD wallets will need some storage. It's probably IndexedDB
for the browser (because it's a recommended way to store stuff in the browser), but we'll probably use a real database
(probably, Postgres or MySQL) for the REST server (since we'll also be keeping stuff like WebHooks there, and there
might be a lot of them, but node.js doesn't have IndexedDB, so I think any wrapper that simulates a database,
won't be sufficient).

We'll probably create a Dockerfile / image for the REST server to simplify the development.

I think the REST server should also be configurable to run the host for iframe-based wallets (if we decide to do one
later, it's not in the scope of the project, though I'd like that).

After that, I think we'll be free to take any tasks from the list because these should be sufficient to start work
on anything like tokens (though this will probably require SLPDB instance), contracts, etc...

I'll be opening GitHub issues with tasks that need to be completed along with expected bounty and people are free to
apply. (Remember though to complete the initial test first unless your name is well-known in BCH community)

As soon as TypeScript/REST API is somewhat stable, we can invite collaborators to create thin wrappers in other languages.
I hope that we could build some sort of a generator that will generate bindings from REST API definition (Swagger?)
rather than writing each method by hand. But maybe it's an overkill.

To-do items (work in progress)
------------------------------
This is an unsorted list of things and ideas that comprise the scope of the project.
The will be converted to GitHub issues.

These items were mostly collected from here:
- https://web.archive.org/web/20200810182937/https://mainnet.cash/
- https://web.archive.org/web/20200810183217/https://mainnet.cash/2020-07-31

```
The documentation website
Register Slack
Servers?
Each method should get corresponding REST API
```

```
Prototype
  (using BITBOX, then integrate functions, libauth?)
  RegTestWallet(name) (Non-HD)
    (named wallet)
    .depositAddress
    .getBalance('USD')
    .send([seller.depositAddress(), [0.01, 'USD']]);
    .sendMax
    .maxSatoshisToSend
  REST Server API
  Setup testing
  CircleCI
    Browser-based testing
    Library build
    Publish it to NPM
    Publish to CDN?
  REST Server Docker build
  REST docs

TestNetWallet
  .putTestnetSatoshis()
  .waitForTransaction()
  .depositAddressQrCodeBase64();
  webhook.register(seller.depositAddress(), 'transaction:in', 'https://mysite.com/api/webhook');

iframe wallet (was not in scope)
webworker wallet (was not in scope)

Wallet (mainnet)

HDWallets in browser
HDWallets via REST?

await seller.cashShuffle();
arbiter.contracts.blindEscrow.create(

.slp.mint('Loyalty token', 'LTK', 1000);
.slp.send([jerry, [3, token]]);
Non-Fungible tokens
.sideshift.exchange([1, 'BCH'], seller.slp.depositAddress(), 'USDH');

.swapProtocol.signal([1, 'BCH'], seller.slp.depositAddress(), 'USDH');
.anyHedge.hedge([1, 'BCH'], 'USD', '3:1', '5 days from now');

new MultisigWallet(2, 2, publicKeys);
.transactionFromString(txString).sign().send();

.encryptedMessages.send(['bitcoincash:qqbob...', msg]);
.encryptedMessages.retrieve('last', 5);

.address(0).registerCashAddress('Jimmy');
.cointext.send(['+15553333333', 1, 'USD']);

.unnamedApp.send(['hello@read.cash', 100, 'USD'],
```

Video tutorials

```
Translations: (we need to decide which languages are actually needed somehow)
- English
- 中文
- Deutsch
- Italiano
- Tagalog
- हिन्दी
```

```
Other languages:
- Browser + LocalStorage
- node.js
- Python
- Java
- PHP
- Ruby
- ElectronJS
- C#
- C++
(thin wrappers, then native signing)
```

```
expiring tips contract (?)
CashScript/Spedn? (+sandbox, debugger?)

CashChannels (?)
CashID (?)
Reusable Payment Codes (?)
CashAccounts (?)
CashFusion (?)
memo/member protocol
```

```
Invite outside developers
PR campaign
Security audit (?)
```

Technical notes
---------------

Running BCHD in RegTest mode:

```
Start a node:
./bchd --regtest --rpclisten=:18334 --rpcuser=chris --rpcpass=letmein --miningaddr=bchreg:prc38tlqr6t5fk2nfcacp3w3hcljz4nj3sw247lksj

Control the node:
./bchctl --testnet -u chris -P letmein generate 101 
# generates 101 block, sending mining rewards to the address above (allows spending mining rewards)
```

An example how to use BCHD's GRPC from JS:

```
import * as bchrpc from "grpc-bchrpc-web/pb/bchrpc_pb";
const GrpcClient = require('grpc-bchrpc-web');

export const host = 'bchd.fountainhead.cash';
export const grpc = new GrpcClient.GrpcClient({ url: `https://${host}:443` });

export function getAddressUtxos(grpc, { address, includeMempool }) {
    const req = new bchrpc.GetAddressUnspentOutputsRequest();
    req.setAddress(address);
    if (includeMempool) {
        req.setIncludeMempool(true);
    }
    return new Promise((resolve, reject) => {
        grpc.client.getAddressUnspentOutputs(req, (err, data) => {
            if (err !== null) { reject(err); } else { resolve(data); }
        });
    });
}
```

Libraries:

- https://github.com/simpleledgerinc/grpc-bchrpc-web (for browser)
- https://github.com/simpleledgerinc/grpc-bchrpc-node (for node.js)

GRPC API definition :

https://github.com/gcash/bchd/blob/master/bchrpc/bchrpc.proto
