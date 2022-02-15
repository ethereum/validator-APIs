# Prysm Specific Flows
Prysm (https://github.com/prysmaticlabs/prysm) at the time of writing is one of the consensus layer clients implementing the Keymanager APIs.
There are some Prysm specific quirks that this document would like to make aware to users.

### Prysm Default Validator API Address
`http://http://127.0.0.1:7500/`

Prysm by default runs on the above address when running with `--web` flag. please look at our documentation [here](https://docs.prylabs.network/docs/prysm-usage/web-interface) for more information. Prysm includes it's own native UI implementation as well.


## prysm version 2.0.7

1. Authentication: Prysm requires a bearer token for using the validator APIs, this token is also used for prysm's native. For more information on where to find this token please look at the Prysm Docs.

4. CORS: Prysm has CORS protection with ports 3000 (typically REACT), 7500, and 4200 (typically Angular). Please use the appropriate Prysm flag to enable other ports. Please look at the Prysm Docs for more information.

3. Prysm Wallet & keymanager Type: prysm uses something called a prysm wallet ( not related to hardware wallet ) to manage the validator keys inside the prysm client. all users of prysm must create a wallet with a specific keymanager type in order to import or delete validator keys. The following errors will be thrown if requirements are not met:
- invalid wallet: either the wallet itself is corrupted, missing, or unable to open.
- invalid keymanager type: the keymanager type when creating the wallet isn't usable with the keymanager apis. Only the `local` keymanager type is allowed to be used for Keymanager APIs referring to the type where users are storing the validators in their local prysm instance instead of using a remote signer.
- validator service cannot get keymanager: The Keymanager API is called too soon after wallet creation.
4. API middleware and Internal RPC: Prysm doesn't natively use REST and uses gRPC internally instead for APIs, that means Prysm is translating Http REST calls int RPC calls and vice versa. This can cause certain types of issues that although unintended, can happen.
- Error code or error message unexpected: because our RPC codes are translating to a HTTP rest error there may be unexpected response codes in which case please report to the Prysm Team
- Golang primitives default values: Golang ( the programming language prysm is written in) will default certain primatives by default ( string as "" instead of null, boolean as false instead of null, etc) we try to account for as many as we can with tests but please let the team know if Prysm behaves differently from other clients.