zcash-wallets
===

Zcash wallets made simple, javascript implementation of [BIP 32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) Bitcoin hierarchical deterministic keys for Zcash t and z-addresses

##Rationale

Create your Zcash t/z-addresses and wallets by your own and recover them from a seed you know if you lose them, this eliminates the constant need of wallets backup with the associated risks and this eliminates the risk of losing all of your addresses

##Implementation, Details and Installation

Please see [bitcoin-wallets](https://github.com/Ayms/bitcoin-wallets)

Zcash's z-addresses are not trivial to generate, this is not used for bitcoin addresses but don't forget to copy [SHA256Compress.js](https://github.com/Ayms/bitcoin-wallets/tree/master/SHA256Compress.js) for Zcash

This might change in the future but for now z-addresses are derived from the hardened m/0/1 branch as defined in BIP32 with the first four bits of the private key set to 0 for the spending key (t-addresses are derived from the hardened m/0/0 branch like bitcoin core is doing)

##Warning

This has not been extensively tested for now, so please keep this in mind although we are confident that this is working correctly (and z-addresses derivation rules might change as explained above, so please backup too the code used at the time you generated your wallet) 

## Use - Generate wallets
	
Output is stored in wallet.txt, all details for the keys are stored in log.txt (recommended to backup somewhere)

	create_wallet(new Buffer('4ecf2e71d567072fe2f9cda40873afcaae4224e3f249018621a90dd43e88f8de','hex'),null,null,'zcash');
	
Output example in wallet.txt for z-addresses (last z-address generated in wallet.txt):

	------------------------------------ depth 3 index hardened 99
	chain code: 70c3eb71d0790eb44d6889de84dbf425b890937a3ade659088ba829193153bf5
	private key: 241ecfa0c6a57786dfb9b8f4ef2deb0b8fc857d253c1667746f4f0f29a61eeb2
	public key: 04cf63f5bc1452c46aa264a01398ee739e0f2de37d6b686855d6f2b38309ba4d9b5e2960f6a2e05cbdc41b17abe88da0236cacac918f4f6cf10f1f7b2acd7b5954
	public key (compact): 02cf63f5bc1452c46aa264a01398ee739e0f2de37d6b686855d6f2b38309ba4d9b
	Extended private key: 33msrix1Pk6znGSspTeeh4gpy6A2H5GJkq4Q38js2n52F3HmyoF2DpQgx1p79DutKtxPoQZMgVZs9VZwZoxZhMmCnhu8iP6fpREPS9bGinkj9EQ3ybz
	Extended public key: 33msrizgKy6MBmymeqCwuYmJ57h2eDD3JrttSbTiPzzd3RhPY8n16cjqVZ8Mh2ENBBHb7E1C7DqQKUxJnBfEAXyLkj1R9EpkQuSfmGzAThatRZWT1a7
	address: t1MYprub2fNk48y37psz2E2rrj4QYRctYF8
	
	You don't really have to take care of the above, the important part is:

	------------------------------------ depth 3 index hardened 99
	spending key: 041ecfa0c6a57786dfb9b8f4ef2deb0b8fc857d253c1667746f4f0f29a61eeb2
	paying key: 504e39ef8d2b36c3cbcccaffd5e2e97d21163f20d7fa0859f716b464c1f7e9e0
	transmission key: 5661ac798de6f48301db7856546d521808ad0096877ad147172b6827dc210fc3
	spending key address: SKxpnNaYmyqc7yB2fVxav6N1aGoJFrts3TnGUkEcF6dc7M3HXT4o
	z-address: zcdejskXc37Gixc7dhe87ZCeaehSi7yHCCkiFhvav7aTxVY3RwXoV6dsu2hcNBVmW4vZHY9uWwCA4DrddwbjqFpMrguQoDW

Check that this is a valid z-address and that the keys are correct:
	
	zcash-cli z_validateaddress  zcdejskXc37Gixc7dhe87ZCeaehSi7yHCCkiFhvav7aTxVY3RwXoV6dsu2hcNBVmW4vZHY9uWwCA4DrddwbjqFpMrguQoDW
	{
		"isvalid" : true,
		"address" : "zcdejskXc37Gixc7dhe87ZCeaehSi7yHCCkiFhvav7aTxVY3RwXoV6dsu2hcNBVmW4vZHY9uWwCA4DrddwbjqFpMrguQoDW",
		"payingkey" : "504e39ef8d2b36c3cbcccaffd5e2e97d21163f20d7fa0859f716b464c1f7e9e0",
		"transmissionkey" : "5661ac798de6f48301db7856546d521808ad0096877ad147172b6827dc210fc3",
		"ismine" : false
	}

Check that zcash-cli would generate the same keys:
	
	 zcash-cli z_importkey SKxpnNaYmyqc7yB2fVxav6N1aGoJFrts3TnGUkEcF6dc7M3HXT4o
	 zcash-cli z_exportwallet path/wallet.txt
	 
Check the last line of wallet.txt:
	 
	 SKxpnNaYmyqc7yB2fVxav6N1aGoJFrts3TnGUkEcF6dc7M3HXT4o 1970-01-01T00:00:00Z # zaddr=zcdejskXc37Gixc7dhe87ZCeaehSi7yHCCkiFhvav7aTxVY3RwXoV6dsu2hcNBVmW4vZHY9uWwCA4DrddwbjqFpMrguQoDW
	
##Example

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
