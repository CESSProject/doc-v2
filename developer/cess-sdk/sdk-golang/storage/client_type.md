The storage node needs to interact with the tee worker node for data exchange. According to the different functions of the interface, P2P clients can be divided into 5 types, as shown in the table below:

| Type | Description |
| ---- | ----------- |
| PubkeysProvider | 1.Get the identity pubkey of the tee worker<br> 2.Get the master pubkey of the tee worker<br> 3.Get the podr2 pubkey of the tee worker |
| Podr2Verifier | Request tee worker to validate proof of service data |
| Podr2 | Request tee worker calculation service file tag |
| PoisVerifier | 1.Request tee worker to validate a single block of idle data<br> 2.Request tee worker to validate total idle data proofs |
| PoisCertifier | 1.Get the public key for storing miner registrations<br> 2.Request tee worker to generate idle data challenge<br> 3.Request tee worker to validate idle data challenge<br> 4.Request tee worker to verify proof of deletion of idle data|