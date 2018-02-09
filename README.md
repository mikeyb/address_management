#  What is this method?
This method of generating, storing and tracking Bitcoin and compatible alt coin private/public keypairs from a single [BIP39 mnemonic](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

This BIP39 mnemonic is then used to derive [BIP44 namespace](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) keypairs.

BIP44 is a protocol designed to make tracking keys under a [BIP32 hiearchal deterministic](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) tree possible.

A BIP32 HD tree takes the BIP32 root key generated from the BIP39 mnemonic and assigns it as the `master` key of the BIP44 namespace.

##  Review
Entropy - Think of this as randomness.  You generate randomness, and this randomness is used to generate a BIP39 mnemonic

Mnemonic - A phrase of words, to be BIP39 compatible it should be 12 or 24 words (You can do other sizes, but to remain compatible stick with 12 or 24, 24 preferred)

BIP44 namespace - BIP44 is simply a protocol that tells you what coin types go where in your tree.  Each supported coin goes under its own sub-tree in the main tree.

BIP39 HD Wallet - Hiearchal Deterministic wallets use a single point/key as the top of the chain/tree and derives keypairs based off the master key

##  Examples
-  Emtropy: Binary, Base 6 (dice), Base 10 (0-9), Hexadecimal (0-9A-F), Cards ([A2-9TJQK][CDHS])
   -  Binary Entropy: 0110101010010100101010101
   -  Base 6 Entropy: 1025015023023051230350512 or 51234615243615243165324
   -  Base 10 Entropy: 8190257894816789123405127
   -  Hexadecimanl Entropy: ABCDEF0123456789
   -  Card Entropy: ahts9h3ckd (ace of hearts, ten of spades, 9 of hearts, 3 of clubs, king of diamonds)

-  Mnemonic: Any string of characters, generally words seperated by spaces.  For bip39 compatibility the wordlist is static.  For non BIP39 HD wallets, you can use any words.  Some wallets only support importing BIP39 compatible words.
   -  The goat jumped over the moon while racing his subyachtmarine
   -  seed alley history pact miss build neck rain damp oval tag army glance love meadow cube negative myth climb achieve couch vital tide lift

-  BIP 44 Namespace:  All BIP44 namespaces start off from master, or `m`.  The hierarchy proposed in this spec is quite comprehensive. It allows the handling of multiple coins, multiple accounts, external and internal chains per account and millions of addresses per chain. 
   -  Structure: m / COIN TYPE / ACCOUNT / CHANGE [0 or 1] / ADDRESS
   -  First Bitcoin address in BIP44 is located at: m/44'/0'/0'/0/0
   -  First GAME address in BIP44 is located at: m/44'/101'/0'/0/0
   -  First ETH address in BIP44 is located at: m/44'/60'/0'/0/0
   -  The apostrophes are important when notating paths, a path with a `'` can sign transactions for any key below it in the tree.  Non `'` keys cannot be used to sign transactions underneath it, but it can be used to derive keypairs

-  BIP39 HD Wallet:  The actual method for using a master key to derive sub-tree address keypairs.

#  ELI5
-  You generate random entropy, I use 6 sided dice.
-  You feed the entropy into the BIP39 algorithm and it creates a mnemonic based on your entropy bits.
   -  You save this!  Forever, DO NOT LOSE THE BIP39 PHRASE
-  The BIP39 mnemonic is then fed into another hashing algo and a BIP32 root key is generated
   -  Can also be saved, but most wallets wont know how to import BIP32 root key, but if you have the root key, you can derive your addresses

#  Get started
##  Download BIP39 Tool
-  Download the offline BIP39 generator: https://github.com/iancoleman/bip39/releases
   -  You want the `bip39-standalone.html` file.  This is a single file that can be used offline with all necessary functionality compiled in.
-  Open the HTML file in your browser and you will see the 'Mnemonic Code Converter' page

##  Mnemonic Generation
-  Click the box on this page for `Show entropy details`
-  Remove any entropy in field if there is any
-  Roll dice or use other methro of Entropy creation
   -  Keep adding entropy until it says `Raw Entropy Words: 24`
-  Save the BIP39 Mnemonic, safely, securely, and forever.

##  BIP32 Root Key
-  I also would save a copy of this.

##  Derivation Path
This is the part where you start generating private/public keypairs for the coins you want addresses for.
-  Leave the tab set to BIP44
-  Right above the BIP32 Root Key above, there is a `Coin` dropdown.  Here you can choose the token you want to generate keys for.
-  When switching to another token, you will see the `Coin` number change underneath the BIP44 tab in the Derivation Path section
-  With the token you want a keypair for selected in the dropdown.  Scroll down to `Derived Addresses`

##  Derived Addresses
-  Here is a list of keypairs under the Path noted above.
   -  Address is the public address you request funds be sent to.
   -  Public Key is the non-compressed public key.  You can convert this to Address, but it is already done for you.  Public key is needed for certain things, but not normal use.
   -  Private Key is the actual private key of the Address listed to the left.  This would be the key you import into other wallets (generally called 'sweeping') that do not support BIP39 mnemonics.
      -  For example:  If you selected the `ETH` coin from the dropdown, you would see a list of ETH addresses and the corresponding private key to the right.  This private key is what you would use in MyEtherWallet to unlock the address on the left side.

##  Important
-  You DO NOT need to keep the private key for each coin address you use, you can simply store your Mnemonic and pop it in the BIP39 tool and it will derive the addresses under it.
-  You DO need to remember the path to the left of the Address.  If you stick to BIP44 address derivation, the path is easy to figure out.  But if you use custom paths that stray from the BIP44 namespace, you will want to track these paths somewhere so you know where to find the keys in the HD tree.
   -  If you forget the path and do not use a standard BIP44 path, finding the correct address keypair can be a pain in the ass if you chose something like `m/420'/710'/4'/2/0`.  This is not a standard path and if you use it, you will want to track that path in a note somewhere.   
