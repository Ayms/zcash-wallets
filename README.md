zcash-wallets
===

Zcash wallets made simple, javascript implementation of [BIP 32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) Bitcoin hierarchical deterministic keys for Zcash's t and z-addresses

##Rationale

Create your Zcash t/z-addresses and wallets by your own and recover them from a seed you know if you lose them, this eliminates the constant need of wallets backup with the associated risks and this eliminates the risk of losing all of your addresses

##Implementation, Details and Installation

Please see [bitcoin-wallets](https://github.com/Ayms/bitcoin-wallets)

Zcash's z-addresses are not trivial to generate, this is not used for bitcoin addresses but you need [SHA256Compress.js](https://github.com/Ayms/bitcoin-wallets/tree/master/SHA256Compress.js) for Zcash

This might change in the future but for now z-addresses are derived from the hardened m/0/1 branch as defined in BIP32 with the first four bits of the private keys set to 0 for the spending keys (t-addresses are derived from the hardened m/0/0 branch like bitcoin core is doing)

##Warning

This has not been extensively tested for now, although we are confident that this is working correctly (and z-addresses derivation rules might change as explained above, so please backup too the code used when you generate your wallet)

Before using it you might consider reading [Bug 1893](https://github.com/zcash/zcash/issues/1893)

## Use - Generate wallets
	
Output is stored in wallet.txt, all details for the keys are stored in log.txt (recommended to backup somewhere)

	create_wallet(new Buffer('4ecf2e71d567072fe2f9cda40873afcaae4224e3f249018621a90dd43e88f8de','hex'),null,null,'zcash');
	
or with the secret option allowing not to store the master seed

	create_wallet(new Buffer('4ecf2e71d567072fe2f9cda40873afcaae4224e3f249018621a90dd43e88f8de','hex'),true,null,'zcash');
	
or to generate more than 100 addresses:

	create_wallet(new Buffer('4ecf2e71d567072fe2f9cda40873afcaae4224e3f249018621a90dd43e88f8de','hex'),null,1000,'zcash');
	
Output example in wallet.txt for z-addresses (last z-address generated in wallet.txt):

	------------------------------------ depth 3 index hardened 99
	chain code: 5fe26266ced419847229c67c52cdcb27e7d0e30e19f93db29e43f85d90b11656
	private key: 068bf5f79b8ffe10891474f24c32a5888c6ed1bb59750a0a4fa99421dd1b0cdf
	public key: 0456b4bcc66c2d63134c85c39866b0275dfa55463ecbc50d2406038af54826a723a28d8b4ee8ede58115f13db1bd8ffc122a25f927991c2d41983e9f69cafb3d79
	public key (compact): 0356b4bcc66c2d63134c85c39866b0275dfa55463ecbc50d2406038af54826a723
	Extended private key: 33msrix1PjeQgRC1R53uFEpDYyykjijZizSUc3Bi6d1XvUjTzUksrXHw7Qh218ffCHmpDoSG6Pehh1BSSFMt6DXg1wjwUG6ghmTmHet727BRjFEAamA
	Extended public key: 33msrizgKxdm5viuFScCTitgf1Wm6rgJH2Gy1VuZTqw8is95YpHrjKd5ex1GYvz93bMdQEBy4t2ZAC5L6sP1Ci5gNkFpYhPBvQ3bnScC7fADVzrWMWH
	address: t1e44ZQHzUB6Q4MYLRvQdzGu9NbexPuMvwd
	------------------------------------ depth 3 index hardened 99
	spending key: 0fbab70eb70f8d5a99e32621c990edc960852b84cf41a4efeee4ef08538ca4fa
	paying key: 67b6baa929cd8793d0c49afb9ac9efc851ad915f502f801f9c6e281a0c9c6cae
	transmission key: 63b706663a1dbd57d1efc1e00b40e8c2226b2c42d4dd4369f214e688df1fa2fe
	spending key address: SKxutugkTFsubPcuWfq12DUU2vDDiUhg8LP834FPiFwYvPTuYF9W
	z-address: zcX3d1ppxo71Xrvk4p5JtvFCxob2uYFuVfJ2gphSzb2taZpqrTrSZYXXW7a8By4aa4coC9Lh5aQF48gW9X8gqZbsRDwwm79

Check that this is a valid z-address and that the keys are correct:
	
	zcash-cli z_validateaddress zcX3d1ppxo71Xrvk4p5JtvFCxob2uYFuVfJ2gphSzb2taZpqrTrSZYXXW7a8By4aa4coC9Lh5aQF48gW9X8gqZbsRDwwm79
	{
		"isvalid" : true,
		"address" : "zcX3d1ppxo71Xrvk4p5JtvFCxob2uYFuVfJ2gphSzb2taZpqrTrSZYXXW7a8By4aa4coC9Lh5aQF48gW9X8gqZbsRDwwm79",
		"payingkey" : "67b6baa929cd8793d0c49afb9ac9efc851ad915f502f801f9c6e281a0c9c6cae",
		"transmissionkey" : "63b706663a1dbd57d1efc1e00b40e8c2226b2c42d4dd4369f214e688df1fa2fe",
		"ismine" : false
	}

Check that zcash-cli generates the same keys:
	
	 zcash-cli z_importkey SKxpnNaYmyqc7yB2fVxav6N1aGoJFrts3TnGUkEcF6dc7M3HXT4o rescan=false
	 zcash-cli z_exportwallet path/wallet.txt
	 
Check in wallet.txt:
	 
	 SKxutugkTFsubPcuWfq12DUU2vDDiUhg8LP834FPiFwYvPTuYF9W 1970-01-01T00:00:00Z # zaddr=zcX3d1ppxo71Xrvk4p5JtvFCxob2uYFuVfJ2gphSzb2taZpqrTrSZYXXW7a8By4aa4coC9Lh5aQF48gW9X8gqZbsRDwwm79
	 
Import your wallet: see z_importwallet in [Zcash payment API](https://github.com/zcash/zcash/blob/master/doc/payment-api.md)

Note: z-addresses are commented in wallet.txt, uncomment those that you want to use
	
##Examples

See [logz.txt](https://github.com/Ayms/bitcoin-wallets/tree/master/tests/logz.txt) and [walletz.txt](https://github.com/Ayms/bitcoin-wallets/tree/master/tests/walletz.txt)

##License

MIT

## Related projects :

* [Ayms/bitcoin-wallets](https://github.com/Ayms/bitcoin-wallets)
* [Ayms/bittorrent-nodeid](https://github.com/Ayms/bittorrent-nodeid)
* [Ayms/torrent-live](https://github.com/Ayms/torrent-live)
* [Ayms/node-Tor](https://github.com/Ayms/node-Tor)
* [Ayms/iAnonym](https://github.com/Ayms/iAnonym)
* [Interception Detector](http://www.ianonym.com/intercept.html)
* [Ayms/abstract-tls](https://github.com/Ayms/abstract-tls)
* [Ayms/websocket](https://github.com/Ayms/websocket)
* [Ayms/node-typedarray](https://github.com/Ayms/node-typedarray)
* [Ayms/node-dom](https://github.com/Ayms/node-dom)
* [Ayms/node-bot](https://github.com/Ayms/node-bot)
* [Ayms/node-gadgets](https://github.com/Ayms/node-gadgets)
