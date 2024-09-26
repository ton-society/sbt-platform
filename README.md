# TON Society Platform
[TON Society Platform](https://society.ton.org) aims to connect projects and users through SBT badges. The platform offers the following:
- allows projects to issue SBT bagdges to their users easily and free of charge
- helps projects attract more users.
- enables users to receive additional badges across the ecosystems, meet and compete

[Learn more](https://eco.ton.org/en/opportunities/make-sbt-campaign)

## Steps for direct integration
### 1. Register an activity
Projects can apply to add relavant activities to the [Activity Catalog](https://society.ton.org/activities) by using [this form](https://eco.ton.org/en/application/make-sbt-campaign). We ask activity owners to provide infromation for both the page at TON Society and cSBT collection to be minted.

We will review all applications and contact activity owners once the application is accepted. You will receive ```partner_id``` and ```api_key``` which is needed to issue cSTBs.

### 2. Issuing a cSBT
Once users complete all the actions on your side you could request a unique link from our API and just lead user to this link for receiving a cSBT:

1. Ask users to [connect a wallet](https://docs.ton.org/develop/dapps/ton-connect/overview) before they start participating in the activity. In case of Telegram Mini App, you could just use Telegram address instead of wallet. You could [get it after Mini App launch](https://docs.telegram-mini-apps.com/platform/init-data).
2. Once users complete all the actions on your side, make a [```POST``` request](https://ton-society.github.io/sbt-platform/#/Activities/createRewardLink), passing your ```activity_id```, ```partner_id```, ```api_key``` and one of user identifiers: ```telegram_user_id``` or ```wallet_address```. The response will contain the link.
    * If you already tried to request a link for this specific user use ```/rewards/{participant_id}``` [endpoint](https://ton-society.github.io/sbt-platform/#/Activities/findRewardLink) to get existing link.
4. Share this link with the user.
5. To get the current status of user's badge use ```/rewards/{participant_id}/status``` [endpoint](https://ton-society.github.io/sbt-platform/#/Activities/getParticipantRewardStatus):
    1. ```NOT_CLAIMED``` The reward has not been claimed by the participant.
    2. ```CLAIMED``` The reward has been claimed by the participant.
    3. ```MINTED``` The reward has been minted on the TON Society side.
    4. ```RECEIVED``` The reward has been received by the participant and visible in the user profile.


### 3. Access cSBT and user data to display on a project side
Use TON Society APIs to display minted cSBTs and user information in your project.

**[Users and Collections](https://ton-society.github.io/sbt-platform/#/Users)**<br />
Provides access to publicly available information on society.ton.org regarding registered users and the SBT/cSBT collections associated with them.

**[cSBT data](https://ton-society.github.io/sbt-platform/#/Compressed%20SBTs)**<br />
Provides access to cSBT collections minted via TON Society in your project. This information complies with the [Compressed SBT Standard](https://github.com/krigga/TEPs/blob/compressed-nfts/text/0000-compressed-nft-standard.md#1-itemsindex).
