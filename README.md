# TON Society Platform
[TON Society Platform](https://society.ton.org) aims to connect projects and users through SBT rewards. The platform offers the following:
- allows projects to issue SBT rewards to their users easily and free of charge
- helps projects attract more users.
- enables users to receive additional rewards across the ecosystems, meet and compete

[Learn more](https://eco.ton.org/en/opportunities/make-sbt-campaign)

## Steps for direct integration
### 1. Register an activity
Projects can apply to add relavant activities to the [Activity Catalog](https://society.ton.org/activities) by using [this form](https://eco.ton.org/en/application/make-sbt-campaign). We ask activity owners to provide infromation for both the page at TON Society and cSBT collection to be minted.

We will review all applications and contact activity owners once the application is accepted. You will receive ```partner_id``` and ```api_key``` which is needed to issue cSTBs.

### 2. Issuing a cSBT
Once users complete all the actions on your side you could request a unique link from our API and just lead user to this link for receiving a cSBT:

1. After the activity aproval, contact us to get corresponding ```activity_id```, ```partner_id``` and ```api_key```.
2. On your side, request users to [connect a wallet](https://docs.ton.org/develop/dapps/ton-connect/overview) before they start participating in the activity. In case of Telegram Mini App, [simply use](https://www.tapps.center/docs/packages/tma-js-init-data/user) ```window.Telegram.WebApp.initDataUnsafe.user.id```.
3. Once users complete all the actions on your side, make a [```POST``` request](https://ton-society.github.io/sbt-platform/#/Activities/createRewardLink), passing your ```activity_id```, ```partner_id```, ```api_key``` and one of user identifiers: ```telegram_user_id``` or ```wallet_address```. The response will contain the link.
4. Share this link with the user.


### 3. Access cSBT and user data to display on a project side
Use TON Society APIs to display minted cSBTs and user information in your project.

**[Users and Collections](https://ton-society.github.io/sbt-platform/#/Users)**<br />
Provides access to publicly available information on society.ton.org regarding registered users and the SBT/cSBT collections associated with them.

**[cSBT data](https://ton-society.github.io/sbt-platform/#/Compressed%20SBTs)**<br />
Provides access to cSBT collections minted via TON Society in your project. This information complies with the [Compressed SBT Standard](https://github.com/krigga/TEPs/blob/compressed-nfts/text/0000-compressed-nft-standard.md#1-itemsindex).
