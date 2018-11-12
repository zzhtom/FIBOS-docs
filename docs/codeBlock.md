# Code Block

## Create Account

```javascript
let r = fibos.transactionSync(tr => {

	tr.newaccount({
		creator: 'creator_account',
		name: 'your_account',
		owner: 'your_owner_publicKey',
		active: 'your_active_publicKey'
	});

	tr.buyrambytes({
		payer: 'creator_account',
		receiver: 'your_account',
		bytes: 4096
	});

	tr.delegatebw({
		from: 'creator_account',
		receiver: 'your_account',
		stake_net_quantity: '0.1000 FO',
		stake_cpu_quantity: '0.1000 FO',
		transfer: 1
	});
});
```
