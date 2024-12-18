# TON Society Platform
[TON Society Platform](https://society.ton.org) aims to connect projects and users through SBT badges. The platform offers the following:
- allows projects to issue SBT bagdges to their users easily and free of charge
- helps projects attract more users.
- enables users to receive additional badges across the ecosystems, meet and compete

[Learn more](https://eco.ton.org/en/opportunities/make-sbt-campaign)

## Steps for direct integration
### 1. Register an activity
1. Contact us to receive ```partner_id``` and ```api_key``` which is needed to issue cSTBs.
2. Register your activity making [```POST``` request](https://ton-society.github.io/sbt-platform/#/Activities/createEvent). You will receive ```activity_id``` and ```activity_slug``` back that is required for issuing cSBTs.
3. You could edit activity details [using this API](https://ton-society.github.io/sbt-platform/#/Activities/updateEvent). For now, SBT details could not be changed via API. If you need to change that, please contact us directly.

### 2. Issuing a cSBT
Once users complete all the actions on your side you could request a unique link from our API and just lead user to this link for receiving a cSBT:

1. Ask users to [connect a wallet](https://docs.ton.org/develop/dapps/ton-connect/overview) before they start participating in the activity. In case of Telegram Mini App, it's prefferable to use Telegram user id instead of wallet. You could [get it after Mini App launch](https://docs.telegram-mini-apps.com/platform/init-data).
2. Once users complete all the actions on your side, submit [Telegram user ID](https://ton-society.github.io/sbt-platform/#/Allowlists/createTelegramUserIdAllowlistEntry) or [User friendly wallet address](https://ton-society.github.io/sbt-platform/#/Allowlists/createWalletAllowlistEntry) passing your ```activity_slug```. From that moment, user is added to whitelist and will be able to mint cSBT.
4. Share the link https://t.me/ton_society_bot/start?startapp=```{your_activity_slug}```&mode=compact with the user.
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
