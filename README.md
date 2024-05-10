# TON Society Platform
[TON Society Platform](https://society.ton.org) aims to connect projects and users through SBT rewards. The platform offers the following:
- allows projects to issue SBT rewards to their users easily and free of charge
- helps projects attract more users.
- enables users to receive additional rewards across the ecosystems, meet and compete

## How it works
1. Projects can publish a variety of activities, organized on their side, in the [TON Society Platform catalogue](https://society.ton.org/). Users can browse the catalog to find activities and participate in the relevant projects:

![345](https://github.com/ton-society/sbt-platform/assets/314577/7e99a9ad-bfb6-4741-83e0-fd2b5fab5bf7)

2. Upon activity completion, projects share with users a **special link** that allows them to receive an **SBT reward**. The SBT appears in the user's wallet within 24 hours.


https://github.com/ton-society/sbt-platform/assets/314577/34f3353a-12df-4eb2-bcdc-c1707437fe5a


3. The received awards are displayed in the user's personal profile. Moreover, users can track the progress of other participants in the [leaderboard](https://society.ton.org/contributors). This what **encourages users to compete and participate in more activities.**
   
![Profile+leaderboard](https://github.com/ton-society/sbt-platform/assets/314577/44a40dfb-4146-4eff-b032-5fe6cb40b025)


## TON Society use Compressed SBTs (cSBT)
Instead of standard SBTs, TON Society uses compressed ones. This approach allows for large-scale minting of SBTs without cost. Here's how compressed SBTs (cSBTs) work:

1. Information about cSBT collections and minted items is stored on the TON Society's backend service.
2. Minting proofs for a given collection are sent on-chain every 24 hours in a single transaction — this leads to significant savings on fees.
3. TON Society's minting system is fully complaint with [Compressed SBT Standard](https://github.com/krigga/TEPs/blob/compressed-nfts/text/0000-compressed-nft-standard.md#1-itemsindex). All ecosystem players willing to display these cSBTs should support it using [open APIs](https://ton-society.github.io/sbt-platform/#/Compressed%20SBTs) built according to the Standard.
4. Currently, cSBT items and collections minted through TON Society are supported and displayed by Getgems, TON Space, Tonkeeper, Tonviewer, and the TON API.

If activity owners decide to reward their participants using classic SBT collections, they need to implement minting on their own. Afterward, the activity and its minted collection can be registered in the TON Society catalog to increase visibility and attract more interest.

## Steps for projects to join

### 1. Register an activity
Projects can apply to add relavant activities to the [Activity Catalog](https://society.ton.org/activities) by using [this form](https://docs.google.com/forms/d/e/1FAIpQLSdLGBSLahzJ9snoWUwpq7AkNaWrXBvAOk9jfW53D4uh_bDxow/viewform). We ask activity owners to provide infromation for both the page at TON Society and cSBT collection to be minted.

We will review all applications and contact activity owners once the application is accepted.

### 2. Receive a link to issue an cSBT
There are two types of links that can be used to issue a cSBT reward. We recommend using a personalized link whenever possible to prevent unauthorized cSBT issuance.

#### 2.1 Anonymous link
Activity owners will receive this link from us after registering their activity and can share it with any users. Anyone can use the link to receive a reward.


#### 2.2 Personalized link 
A unique link should be generated for each specific user. Activity owners can generate the link once it's been confirmed that a person is eligible for a reward.

To get a personalized link:

1. After the activity aproval, contact us to get corresponding ```activity_id```, ```partner_id``` and ```key```.
2. On your side, request users to [connect a wallet](https://docs.ton.org/develop/dapps/ton-connect/overview) before they start participating in the activity. In case of Telegram Mini App, simply [request access to the user's Telegram handle](https://core.telegram.org/bots/webapps#initializing-mini-apps).
3. Once users complete all the actions on your side, make a ```POST``` request to specific TON Society endpoint, passing your ```activity_id```, ```partner_id```, ```key``` and one of user identifiers: ```telegram_handle``` or ```wallet_address```. The response will contain the link.
4. Share this link with the user.

### 3. Access cSBT and user data to display on a project side
Use TON Society APIs to display minted cSBTs and user information in your project.

**[Users and Collections](https://ton-society.github.io/sbt-platform/#/Users)**<br />
Provides access to publicly available information on society.ton.org regarding registered users and the SBT/cSBT collections associated with them.

**[cSBT data](https://ton-society.github.io/sbt-platform/#/Compressed%20SBTs)**<br />
Provides access to cSBT collections minted via TON Society in your project. This information complies with the [Compressed SBT Standard](https://github.com/krigga/TEPs/blob/compressed-nfts/text/0000-compressed-nft-standard.md#1-itemsindex).
